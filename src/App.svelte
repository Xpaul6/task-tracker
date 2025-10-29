<script lang="ts">
  import ToDo from "./components/Todo.svelte";
  import Timer from "./components/Timer.svelte";

  let formatedTimerString = $state<string>();
  let tasksRemaining = $state<string>();
</script>

<svelte:head>
  <title>{formatedTimerString} | {tasksRemaining} left</title>
</svelte:head>

<main
  class="absolute w-full h-full flex items-center justify-around"
>
  <div class="w-full md:w-[80%] flex flex-row flex-wrap justify-around items-center">
    <Timer bind:formatedTimerString />
    <ToDo bind:tasksRemaining />
  </div>
</main>
