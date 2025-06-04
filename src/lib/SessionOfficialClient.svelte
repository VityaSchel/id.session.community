<script lang="ts">
  import Button from './Button.svelte'
</script>

<div class="flex flex-col items-start rounded-lg border border-neutral-300/30 p-4">
  <div class="flex">
    <div class="flex-1 text-sm">
      <h3 class="mb-4 text-base font-medium">Session</h3>
      <p class="my-[14px] text-[#333333] dark:text-[#d1d1d1]">
        Fully-featured official client, used by millions. <br />
        <a
          href="https://getsession.org/"
          target="_blank"
          rel="noopener noreferrer nofollow"
          class="text-[#0098d4] dark:text-[#75d5fa]"
        >
          Learn more
        </a>
      </p>
      <p class="my-[14px] text-[#333333] dark:text-[#d1d1d1]">
        Windows, Mac, Linux, Android &amp; iOS
      </p>
    </div>
    <div class="ml-4">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 303.06 336.3" width="60" height="60">
        <path
          fill="#00F782"
          d="M255.67,170.86l-63.48-35.17h43.03c37.41,0,67.85-30.43,67.85-67.84S272.63,0,235.22,0H85C38.13,0,0,38.13,0,85
c0,33.42,18.16,64.25,47.4,80.45l63.48,35.17H67.85C30.44,200.62,0,231.05,0,268.46s30.44,67.85,67.85,67.85h150.22
c46.87,0,85-38.13,85-85C303.06,217.88,284.9,187.06,255.67,170.86z M57.58,147.05c-22.06-12.22-35.95-35.25-36.54-60.39
C20.19,50.57,50.5,21.02,86.61,21.02h147.25c25.18,0,46.88,19.31,48.12,44.46c1.33,26.88-20.16,49.18-46.76,49.18
c0,0-60.99,0.01-84.81,0.01c-5.19,0-9.37,4.21-9.38,9.39l-0.02,69.22L57.58,147.05z M216.46,315.28H69.2
c-25.18,0-46.88-19.31-48.12-44.46c-1.33-26.88,20.16-49.18,46.76-49.18h84.81c5.19,0,9.39-4.21,9.39-9.39v-69.23l83.44,46.23
c22.06,12.22,35.95,35.25,36.54,60.39C282.87,285.73,252.56,315.28,216.46,315.28z"
        />
      </svg>
    </div>
  </div>
  <a
    href="https://getsession.org/download"
    class="mt-2 rounded-full focus:outline-none focus-visible:outline-none"
  >
    <Button tabindex={-1}>Download</Button>
  </a>
</div>
