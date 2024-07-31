<script lang="ts">
  import { fade } from "svelte/transition";

  import { tasks } from "../stores/tasks";

  import Header from "../components/Header.svelte";
  import Task from "../components/Task.svelte";

  let toggleNewTask = $state(false);
</script>

<Header bind:toggleNewTask />
<main class="min-[400px]:mx-0 : m-2">
  {#if $tasks.length === 0 && !toggleNewTask}
    <div
      in:fade={{ delay: 200, duration: 150 }}
      class="mt-10 text-2xl text-center"
    >
      no tasks...
    </div>
  {:else}
    {#each $tasks as task (task.id)}
      <Task {task} />
    {/each}
  {/if}
</main>

<style lang="scss">
  :global(html, body) {
    height: 100%;
    width: 100%;
  }
  :global(body) {
    margin: 0;
    padding: 0;
    @media screen and (min-width: 400px) {
      max-width: 400px;
      margin: auto;
    }

    font-family:
      system-ui,
      -apple-system,
      BlinkMacSystemFont,
      "Segoe UI",
      Roboto,
      Oxygen,
      Ubuntu,
      Cantarell,
      "Open Sans",
      "Helvetica Neue",
      sans-serif;
  }
</style>
