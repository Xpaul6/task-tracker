<script lang="ts">
  import ToDo from "./components/Todo.svelte";
  import Timer from "./components/Timer.svelte";
  import ThemeSwitch from "./components/ThemeSwitch.svelte";

  let formatedTimerString = $state<string>();
  let tasksRemaining = $state<string>();
  let theme = $state<string>();
</script>

<svelte:head>
  <title>{formatedTimerString} | {tasksRemaining} left</title>
</svelte:head>

<main
  class={`absolute overflow-scroll w-full min-h-full flex items-center justify-around`}
>
  <ThemeSwitch bind:theme />
  <div class="w-full md:w-[80%] flex flex-row flex-wrap justify-around items-center rounded-xl">
    <Timer bind:formatedTimerString />
    <ToDo bind:tasksRemaining />
  </div>
</main>
