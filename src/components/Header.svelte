<script lang="ts">
  import Icon from "@iconify/svelte";
  import BaseButton from "./BaseButton.svelte";
  import Task from "./Task.svelte";
  import { fade } from "svelte/transition";

  let {
    class: className,
    toggleNewTask = $bindable(false),
    ...props
  }: { class?: string; [key: string]: any } = $props();
</script>

<header
  class="flex justify-between items-center px-5 py-2 text-white bg-blue-500 rounded-br-lg rounded-bl-lg {className}"
  {...props}
>
  <h1 class="text-xl font-bold">Tasks</h1>

  <BaseButton
    onclick={() => (toggleNewTask = !toggleNewTask)}
    class="p-2 text-2xl rounded-full"
    aria-label="Add task"
  >
    {#if toggleNewTask}
      <div in:fade={{ duration: 150 }}>
        <Icon icon="fa6-solid:xmark" />
      </div>
    {:else}
      <div in:fade={{ duration: 150 }}>
        <Icon icon="fa6-solid:plus" />
      </div>
    {/if}
  </BaseButton>
</header>

{#if toggleNewTask}
  <div class="m-2 min-[400px]:mx-0">
    <Task bind:isShown={toggleNewTask} />
  </div>
{/if}
