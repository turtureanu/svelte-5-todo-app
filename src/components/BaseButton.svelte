<script lang="ts">
  import type { Snippet } from "svelte";

	let { class: className,  children, ...props }: { class?: string, children: Snippet, [key: string]: any}= $props();

</script>

<button class={className} {...props}>{@render children()}</button>

