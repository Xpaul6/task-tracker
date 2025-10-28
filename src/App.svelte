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
  class="absolute w-full h-full flex items-center justify-around flex-wrap flex-row-reverse"
>
  <ToDo bind:tasksRemaining />
  <Timer bind:formatedTimerString />
</main>
