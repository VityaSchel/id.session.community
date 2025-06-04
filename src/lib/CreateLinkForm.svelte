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
</script>

<form
  action="/"
  class="flex w-[670px] max-w-full flex-col items-center justify-center gap-6"
  method="GET"
  onsubmit={(e) => {
    e.preventDefault()
    if (!value) {
      error = 'Enter a Session/Account ID'
      return false
    }
    if (value.length !== 66) {
      error = 'Session ID must be exactly 66 characters long'
      return false
    }
    if (!value.startsWith('05')) {
      error = 'Session ID must start with "05"'
      return false
    }
    if (!/^[a-f0-9]+$/.test(value)) {
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
    placeholder="05..."
    maxlength="66"
    name="id"
    autocomplete="off"
    autocorrect="off"
    autocapitalize="off"
    spellcheck="false"
    type="text"
    required
  />
  <Button>Get a link</Button>
  <div class="min-h-5 w-[600px] max-w-full text-center text-sm font-medium text-red-500">
    {#if error}
      <span>{error}</span>
    {/if}
  </div>
</form>
