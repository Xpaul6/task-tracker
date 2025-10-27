<script lang="ts">
  import { slide } from "svelte/transition";
  import { onMount } from "svelte";

  import bin_img from "./assets/bin.png";
  import checkmark_img from "./assets/check.png";

  interface Task {
    id: string;
    text: string;
    completed: boolean;
  }

  let taskList = $state<Task[]>([]);
  let tasksAmount = $derived<number>(taskList.filter((task) => !task.completed).length);
  let currentInput = $state<string>("");

  function init() {
    if (!localStorage.getItem("tasks")) {
      localStorage.setItem("tasks", JSON.stringify([]));
    } else {
      taskList = JSON.parse(localStorage.getItem("tasks")) as Task[];
    }
  }

  function addTask() {
    if (currentInput.trim() == "") return;

    let newTask: Task = {
      id: crypto.randomUUID(),
      text: currentInput,
      completed: false,
    };
    taskList.push(newTask);
    currentInput = "";
    localStorage.setItem("tasks", JSON.stringify(taskList));
  }

  function deleteTask(deleteId: string) {
    taskList = taskList.filter((task) => task.id != deleteId);
    localStorage.setItem("tasks", JSON.stringify(taskList));
  }

  function completeTask(completeId: string) {
    const task: Task = taskList.find((task) => task.id == completeId);
    if (task) task.completed = true;
    localStorage.setItem("tasks", JSON.stringify(taskList));
  }

  function handleKeyDown(e: KeyboardEvent) {
    if (e.key == "Enter") {
      addTask();
    }
  }

  onMount(() => {
    init();
  });
</script>

<svelte:head>
  <title>{tasksAmount == 0 ? "No " : tasksAmount} tasks left</title>
</svelte:head>
<svelte:window onkeydown={handleKeyDown} />

<main class="absolute w-[100%] h-[100%] flex items-center justify-center">
  <div class="task-tracker min-w-[300px]">
    <h1>Your tasks</h1>
    <!-- input block  -->
    <div class="flex flex-row border-blue-500 border-2 rounded-md">
      <input
        class="relative p-1 w-[100%] focus:outline-none"
        type="text"
        bind:value={currentInput}
        placeholder="Enter new task"
      />
      <button
        class="px-2 relative my-0.5 hover:cursor-pointer active:bg-gray-200 active:rounded-sm text-l"
        onclick={addTask}
      >
        ↵
      </button>
    </div>
    <!-- tasks block -->
    <div class="mt-2">
      <h2>Active:</h2>
      {#each taskList.filter((task) => !task.completed) as task (task.id)}
        <div class="flex flex-row justify-between" transition:slide>
          <button
            class="flex justify-self-start text-xl hover:cursor-pointer"
            onclick={() => completeTask(task.id)}
          >
            <img src={checkmark_img} alt="" class="m-1 size-5" />
          </button>
          <div class="flex text-xl">{task.text}</div>
          <button
            class="flex justify-self-end hover:cursor-pointer"
            onclick={() => deleteTask(task.id)}
          >
            <img src={bin_img} alt="" class="m-1 size-5" />
          </button>
        </div>
      {:else}
        <div transition:slide>None</div>
      {/each}
      <!-- done tasks heading -->
      {#if taskList.filter((task) => task.completed).length != 0}
        <div transition:slide>
          <div class="border-b-2 border-gray-500 my-1"></div>
          <h2>Done:</h2>
        </div>
      {/if}
      {#each taskList.filter((task) => task.completed) as task (task.id)}
        <button
          class="flex flex-row justify-center w-[100%]"
          id={task.id}
          onclick={() => deleteTask(task.id)}
          transition:slide
        >
          <div class="flex text-xl hover:text-red-500 hover:cursor-pointer">
            {task.text}
          </div>
        </button>
      {/each}
    </div>
  </div>
</main>
