<script lang="ts">
  import { goto } from '$app/navigation'
  import Button from '$lib/Button.svelte'
  import { onMount } from 'svelte'
  import { onsRegex } from '../ons'
  import tippy from 'tippy.js'

  let {
    onCreated
  }: {
    onCreated: () => void
  } = $props()

  let value = $state('')
  let error = $derived('')
  let submitting = $state(false)
  const resolveOnsToSessionIdsKey = 'id.session.community-resolveOnsToSessionIds'
  let resolveOnsToSessionIds = $state(
    window.localStorage.getItem(resolveOnsToSessionIdsKey) === 'true'
  )

  let resolveOnsTooltipTarget: HTMLButtonElement
  onMount(() => {
    const instance = tippy(resolveOnsTooltipTarget, {
      content:
        'Check to resolve the inputted ONS to Session ID;<br />Uncheck to get a link directly to the inputted ONS;<br />Option has no effect when you input Session ID;<br /><b>Resolving ONS exposes your IP address and the resolving name to Cloudflare Worker</b>',
      allowHTML: true,
      theme: 'light-border'
    })
    return () => {
      instance.destroy()
    }
  })
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
      if (resolveOnsToSessionIds) {
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
    class="w-full border border-neutral-500 px-4 py-4 font-mono focus:border-neutral-600 focus:outline-2 focus:outline-[#00A85A] focus:outline-dotted disabled:opacity-50 focus:dark:outline-[#00f782]"
    placeholder="ONS or 05..."
    maxlength="66"
    name="id"
    autocomplete="off"
    autocorrect="off"
    autocapitalize="off"
    spellcheck="false"
    type="text"
    required
    disabled={submitting}
  />
  <div class="flex w-full items-center gap-4">
    <Button disabled={submitting}>Get a link</Button>
    <div class="flex items-center gap-2">
      <input
        type="checkbox"
        bind:checked={
          () => resolveOnsToSessionIds,
          (value) => {
            resolveOnsToSessionIds = value
            window.localStorage.setItem(resolveOnsToSessionIdsKey, String(value))
          }
        }
        class="green-checkbox peer disabled:opacity-50"
        id="resolveOnsToSessionIds"
        name="resolveOnsToSessionIds"
        disabled={submitting}
      />
      <label for="resolveOnsToSessionIds" class="peer-disabled:opacity-50">
        Resolve ONS to Session ID
      </label>
      <button bind:this={resolveOnsTooltipTarget} aria-label="Help tooltip">
        <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
          <g fill="none">
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M2 12C2 6.477 6.477 2 12 2s10 4.477 10 10s-4.477 10-10 10S2 17.523 2 12zm10-5a2 2 0 0 0-2 2a1 1 0 0 1-2 0a4 4 0 1 1 5.31 3.78a.674.674 0 0 0-.273.169a.177.177 0 0 0-.037.054v.497a1 1 0 1 1-2 0V13c0-1.152.924-1.856 1.655-2.11A2.001 2.001 0 0 0 12 7zm1 6.007v-.004v.004zM13 17a1 1 0 1 1-2 0a1 1 0 0 1 2 0z"
              fill="currentColor"
            />
          </g>
        </svg>
      </button>
    </div>
  </div>
  <div class="min-h-5 w-[600px] max-w-full text-center text-sm font-medium text-red-500">
    {#if error}
      <span>{error}</span>
    {/if}
  </div>
</form>
