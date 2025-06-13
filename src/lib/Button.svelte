<script lang="ts">
  let {
    children,
    disabled,
    class: className,
    ...props
  }: {
    disabled?: boolean
    class?: import('svelte/elements').ClassValue
    children: import('svelte').Snippet
  } & import('svelte/elements').SvelteHTMLElements['button'] = $props()
</script>

<button
  class={[
    'min-h-[34px] min-w-[160px] cursor-pointer rounded-full border border-[#00A85A] px-[18px] font-semibold tracking-[0.3px] text-[#00A85A] hover:border-black hover:bg-black/5 hover:text-black focus:outline-none focus-visible:border-black focus-visible:bg-black/5 focus-visible:text-black focus-visible:outline-none in-focus-visible:border-black in-focus-visible:bg-black/5 in-focus-visible:text-black dark:border-[#00f782] dark:text-[#00f782] hover:dark:border-white hover:dark:bg-white/30 hover:dark:text-white focus-visible:dark:border-white focus-visible:dark:bg-white/30 focus-visible:dark:text-white in-focus-visible:dark:border-white in-focus-visible:dark:bg-white/30 in-focus-visible:dark:text-white',
    className,
    {
      'opacity-50': disabled
    }
  ]}
  {disabled}
  {...props}
>
  {@render children?.()}
</button>
