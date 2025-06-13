<script lang="ts">
  import '../app.css'
  import '../tailwind.css'
  import 'tippy.js/dist/tippy.css'
  import 'tippy.js/themes/light-border.css'

  let { children } = $props()
</script>

{@render children()}
