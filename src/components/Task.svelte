<script lang="ts">
  import Icon from "@iconify/svelte";
  import { type Task, addTask, removeTask, updateTask } from "../stores/tasks";
  import BaseButton from "./BaseButton.svelte";
  import { fade, slide } from "svelte/transition";

  let {
    task,
    isShown = $bindable(false),
    class: className,
    ...props
  }: {
    task?: Task;
    isNotCanceled?: boolean;
    class?: string;
    [key: string]: any;
  } = $props();

  let taskName = $state(task?.title || "");
  let taskDesc = $state(task?.description || "");
  let isEditable = $state(false);
</script>

<div
  class="mb-2 careful-text-overflow p-2 rounded-lg bg-slate-200 {className}"
  in:slide={{ duration: 200 }}
  out:slide={{ duration: 200 }}
  {...props}
  role="region"
  aria-labelledby="task"
>
  {#if task && !isEditable}
    <div class="flex justify-between items-center pl-2 w-full" in:fade={{ duration: 250 }}>
      <div class="flex flex-col justify-center">
        <p class="mx-0 text-lg font-bold leading-5">{task.title}</p>
        {#if task.description}
          <p class="pb-2 mt-2 w-full italic leading-5" aria-describedby="task-description">{task.description}</p>
        {/if}
      </div>
      <div class="flex">
        <BaseButton
          class="px-2 py-3 hover:text-red-500"
          onclick={() => removeTask(task)}
          aria-label="Delete task"
        >
          <Icon icon="fa6-solid:trash" />
        </BaseButton>
        <BaseButton
          class="px-2 py-3 hover:text-yellow-600"
          onclick={() => (isEditable = true)}
          aria-label="Edit task"
        >
          <Icon icon="fa6-solid:pencil" />
        </BaseButton>
        <BaseButton
          class="py-3 pr-2 pl-1 text-2xl hover:text-sky-500"
          onclick={() =>
            task && updateTask(task.id, { ...task, completed: !task.completed })
          }
          aria-label={task.completed ? "Mark task as incomplete" : "Mark task as complete"}
        >
          {#if task.completed}
            <Icon icon="mdi:checkbox-outline" />
          {:else}
            <Icon icon="mdi:checkbox-blank-outline" />
          {/if}
        </BaseButton>
      </div>
    </div>
  {:else}
    <form class="flex flex-col gap-y-2" in:fade={{ duration: 150 }} action="#" onsubmit={(e) => e.preventDefault()} aria-labelledby="task-edit">
      <div class="flex flex-col gap-y-2">
        <label for="task-title-input" class="sr-only">Task name</label>
        <input
          class="px-4 py-2 w-full rounded-lg bg-slate-300"
          id="task-title-input"
          placeholder="Task name"
          type="text"
          name="task-title"
          required
          bind:value={taskName}
        />
        <label for="task-description-input" class="sr-only">Task description</label>
        <input
        id="task-description-input"
          class="px-4 py-2 w-full rounded-lg bg-slate-300"
          type="text"
          name="description"
          placeholder="Task description"
          bind:value={taskDesc}
        />
      </div>

      <div class="flex gap-x-2 justify-center items-center">
        <BaseButton
          class="p-2 w-full rounded-xl"
          onclick={() => {
            if (isEditable) {
              taskName = task!.title;
              taskDesc = task!.description || taskDesc;
              isEditable = false;
            } else {
              isShown = false;
            }
          }}
          aria-label="Cancel editing task"
        >
          Cancel
        </BaseButton>
        <BaseButton
          action="submit"
          class="p-2 w-full bg-sky-300 rounded-xl"
          onclick={() => {
            if (isEditable && taskName !== "") {
              updateTask(task!.id, {
                id: task!.id,
                title: taskName,
                description: taskDesc,
                completed: task!.completed,
              });
              isEditable = false;
            } else if (taskName !== "") {
              addTask({
                id: Math.random(),
                title: taskName,
                description: taskDesc,
                completed: false,
              });
              isShown = false;
            }
          }}
          aria-label="Finish editing task"
        >
          Done
        </BaseButton>
      </div>
    </form>
  {/if}
</div>

<style>
  .careful-text-overflow {
    word-wrap: break-word;
    white-space: pre-wrap;
    word-break: break-word;
  }
</style>
