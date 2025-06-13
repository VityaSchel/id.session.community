<script lang="ts">
  import { goto } from '$app/navigation'
  import Button from '$lib/Button.svelte'

  let {
    onCreated
  }: {
    onCreated: () => void
  } = $props()

  let value = $state('')
  let error = $derived('')
  let submitting = $state(false)

  const onsRegex = /^\w([\w-]*[\w])?$/
</script>

<form
  action="/"
  class="flex w-[670px] max-w-full flex-col items-center justify-center gap-6"
  method="GET"
  onsubmit={async (e) => {
    e.preventDefault()
    if (!value) {
      error = 'Enter a Session/Account ID or ONS'
      return false
    }
    if (value.length > 66) {
      error = 'Session ID must be exactly 66 characters long'
      return false
    }
    if (onsRegex.test(value) && value.length <= 64) {
      try {
        submitting = true
        const response = await fetch(
          'https://cf-ons-resolver.hloth.workers.dev/resolve/' + value
        ).then(
          (res) =>
            res.json() as Promise<
              { ok: true; sessionId: string | null } | { ok: false; error: string }
            >
        )
        if (!response.ok) {
          throw new Error(response.error)
        }
        if (!response.sessionId) {
          error = 'ONS not found'
          return false
        }
        value = response.sessionId
      } catch (err) {
        console.error(err)
        error = 'Failed to resolve ONS, please try again later'
        return false
      } finally {
        submitting = false
      }
    } else if (!/^05[a-f0-9]+$/.test(value)) {
      error = 'Session ID must contain only hexadecimal characters (0-9, a-f)'
      return false
    }
    goto('/#' + value, {
      // replaceState: true,
      noScroll: true,
      keepFocus: false
    }).then(onCreated)
    value = ''
    return false
  }}
>
  <input
    bind:value={
      () => value,
      (newValue) => {
        value = newValue.toLowerCase()
        error = ''
      }
    }
    class="w-full border border-neutral-500 px-4 py-4 font-mono focus:border-neutral-600 focus:outline-2 focus:outline-[#00A85A] focus:outline-dotted focus:dark:outline-[#00f782]"
    placeholder="ONS or 05..."
    maxlength="66"
    name="id"
    autocomplete="off"
    autocorrect="off"
    autocapitalize="off"
    spellcheck="false"
    type="text"
    required
  />
  <Button disabled={submitting}>Get a link</Button>
  <div class="min-h-5 w-[600px] max-w-full text-center text-sm font-medium text-red-500">
    {#if error}
      <span>{error}</span>
    {/if}
  </div>
</form>
