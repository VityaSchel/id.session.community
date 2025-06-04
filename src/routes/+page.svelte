<script lang="ts">
  import { page } from '$app/state'
  import SessionOfficialClient from '$lib/SessionOfficialClient.svelte'
  import CreateLinkForm from '$lib/CreateLinkForm.svelte'
  import copy from 'copy-to-clipboard'
  import { scale } from 'svelte/transition'
  import { expoOut } from 'svelte/easing'
  import { onMount } from 'svelte'
  import CopyUrlHint from '$lib/CopyUrlHint.svelte'
  import { onNavigate } from '$app/navigation'
  // import { browser } from '$app/environment'

  let hash = $derived(page.url.hash)
  let sessionId = $derived(hash.substring(1))
  let copied = $state(false)

  let browser = $state(false)
  onMount(() => (browser = true))

  let justCreated = $state(false)
  onNavigate(() => {
    justCreated = false
  })

  let sessionIdSpecified = $derived(Boolean(sessionId))
  let sessionIdValid = $derived(sessionIdSpecified && /^05[a-f0-9]{64}$/.test(sessionId))
  let sessionIdShort = $derived(`${sessionId.substring(0, 4)}...${sessionId.slice(-4)}`)
</script>

<svelte:head>
  {#if sessionIdSpecified}
    {#if sessionIdValid}
      <title>Chat with {sessionIdShort}</title>
    {:else}
      <title>Invalid Session ID</title>
    {/if}
  {:else}
    <title>Create a link to your Session ID</title>
  {/if}
  <meta name="description" content="Create a link to your Session ID in Session messenger" />
</svelte:head>
<svelte:window
  on:hashchange={() => {
    justCreated = false
    hash = window.location.hash
  }}
/>
{#if browser}
  <main
    class="flex min-h-screen flex-col items-center justify-center p-4 sm:p-8 md:p-16"
    transition:scale={{ duration: 350, easing: expoOut, opacity: 0, start: 0.98 }}
  >
    {#if sessionIdSpecified}
      {#if justCreated}
        <CopyUrlHint />
      {/if}
      {#if sessionIdValid}
        <h1 class="mb-8 text-center text-3xl font-medium sm:text-4xl">
          Chat with <span class="text-[#00A85A] dark:text-[#00f782]">
            {sessionIdShort}
          </span>
        </h1>
        <div
          class="flex w-[450px] max-w-full flex-col gap-4 rounded-2xl bg-white/10 px-5 py-4 shadow-lg sm:rounded-3xl sm:px-8 sm:py-7"
        >
          <ol class="list-decimal space-y-3 sm:space-y-4">
            <li class="ms-4 font-medium">Download one of Session clients</li>
            <ul>
              <li>
                <SessionOfficialClient />
              </li>
            </ul>
            <li class="ms-4.5 mt-6 font-medium sm:mt-8">
              Use the following Account ID (Session ID):
            </li>
            <button
              class="block cursor-pointer border border-black/20 bg-black/10 p-2 text-left font-mono text-sm break-all select-all"
              onpointerdown={() => {
                copy(sessionId)
                copied = true
              }}
              onclick={() => {
                copy(sessionId)
                copied = true
              }}
            >
              {sessionId}
              <span
                class="pointer-events-none float-end mt-0.5 mr-1 font-sans text-xs text-neutral-400 select-none"
              >
                {browser ? (copied ? 'copied' : 'click to copy') : 'click to select'}
              </span>
            </button>
          </ol>
        </div>
      {:else}
        <span class="text-xl font-medium">Sorry, the URL seems to be incorrect</span>
      {/if}
    {:else}
      <h1 class="mb-8 text-center text-4xl font-medium">
        Create a link to your <a
          href="https://getsession.org/"
          class="text-[#00A85A] dark:text-[#00f782]"
        >
          Session ID
        </a>
      </h1>
      <CreateLinkForm onCreated={() => (justCreated = true)} />
    {/if}
    <footer class="mt-8 text-center text-xs text-gray-400">
      <span>
        <a href="/" class="font-medium">id.session.community</a>
        by
        <a href="https://hloth.dev" class="font-medium">hloth.dev</a>
      </span>
      ·
      <span>this website is not affiliated with Session Technology Foundation and OXEN</span>
      <br />
      <span>
        author of the website is not responsible or liable for the content shared in Session
      </span>
    </footer>
  </main>
{/if}
<noscript>
  <span
    class="py-auto mx-auto flex min-h-screen w-[640px] max-w-full flex-col items-center justify-center gap-8 px-8 text-center"
  >
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 256 256" class="block w-48 max-w-full">
      <!-- Icon from Phosphor by Phosphor Icons - https://github.com/phosphor-icons/core/blob/main/LICENSE -->
      <path
        fill="currentColor"
        d="M158.66 188.43a8 8 0 0 1-11.09 2.23C141.07 186.34 136 184 128 184s-13.07 2.34-19.57 6.66a8 8 0 0 1-8.86-13.32C108 171.73 116.06 168 128 168s20 3.73 28.43 9.34a8 8 0 0 1 2.23 11.09M224 48v55.77c0 35.84-9.65 69.65-27.18 95.18c-18.16 26.46-42.6 41-68.82 41s-50.66-14.57-68.82-41C41.65 173.44 32 139.63 32 103.79V48a16 16 0 0 1 21.79-14.91C67.84 38.55 96.18 47.71 128 47.71s60.15-9.16 74.21-14.62A16 16 0 0 1 224 48m-16 0c-15.1 5.89-45.57 15.73-80 15.73S63.1 53.87 48 48v55.79c0 32.64 8.66 63.23 24.37 86.13C87.46 211.9 107.21 224 128 224s40.54-12.1 55.63-34.08C199.34 167 208 136.43 208 103.79Zm-18.66 66a8 8 0 0 0-11.3.62c-2.68 3-8.85 5.34-14 5.34s-11.36-2.35-14-5.34A8 8 0 0 0 138 125.33c5.71 6.38 16.14 10.67 26 10.67s20.25-4.29 26-10.67a8 8 0 0 0-.66-11.33M118 125.33a8 8 0 0 0-12-10.67c-2.68 3-8.85 5.34-14 5.34s-11.36-2.35-14-5.34a8 8 0 0 0-12 10.67C71.75 131.71 82.18 136 92 136s20.25-4.29 26-10.67"
      />
    </svg>
    Sorry, this website requires JavaScript because it's fully static and clientside!
    <br class="my-2" />
    On the other hand, we'll never know what Session IDs you have visited — URL fragment after «#» character
    is not sent to the server!
  </span>
</noscript>
