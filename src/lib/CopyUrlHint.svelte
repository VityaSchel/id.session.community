<script>
  import { expoOut } from 'svelte/easing'
  import { fly } from 'svelte/transition'
</script>

<div
  in:fly={{ y: 20, duration: 300, easing: expoOut }}
  class="bg-wave fade-out fixed top-6 left-1/2 flex -translate-x-1/2 flex-col items-center gap-4 rounded-xl px-3 py-2 select-none md:top-3 md:left-[15%] md:translate-x-0"
>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    width="32"
    height="32"
    viewBox="0 0 24 24"
    class="hidden md:block"
  >
    <!-- Icon from Material Symbols by Google - https://github.com/google/material-design-icons/blob/master/LICENSE -->
    <path fill="currentColor" d="m13.775 22l-3.625-7.8L6 20V2l14 11h-7.1l3.6 7.725z" />
  </svg>
  <span class="text-sm font-medium">Now copy the URL</span>
</div>

<style>
  .bg-wave {
    --bg-wave: rgba(0, 0, 0, 0.1);
    --bg: #ffffff;
  }
  @media (prefers-color-scheme: dark) {
    .bg-wave {
      --bg-wave: rgba(255, 255, 255, 0.2);
      --bg: #333132;
    }
  }
  .bg-wave {
    background-image:
      linear-gradient(
        60deg,
        transparent 30%,
        var(--bg-wave) 40%,
        var(--bg-wave) 50%,
        transparent 60%
      ),
      linear-gradient(var(--bg), var(--bg));
    background-size: cover;
    background-position-x: -150px, 0;
    background-repeat: no-repeat, repeat;
    animation:
      wave 500ms linear,
      fade-out 100ms ease-out 3s forwards;
  }

  @keyframes wave {
    from {
      background-position-x: -150px, 0;
    }
    to {
      background-position-x: 150px, 0;
    }
  }

  @keyframes fade-out {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }
</style>
