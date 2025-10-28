<script lang="ts">
  import { onMount } from "svelte";

  const baseFocusInterval = 25 * 60;

  // settings
  let focusInterval = $state<number>(baseFocusInterval);
  let shortPauseInterval = $state<number>(5 * 60);
  let longPauseInterval = $state<number>(15 * 60);

  // dynamic state values
  let remainingTime = $state<number>(baseFocusInterval);
  let formatedTime = $derived<string>(formatTime(remainingTime));
  let isGoing = $state<boolean>(false);
  let stateSwitchCounter = $state<number>(1);
  let timerTitle = $state<string>("Focus");

  // functions
  function formatTime(s: number): string {
    let hours: number = Math.floor(s / 60);
    let fhours: string = hours < 10 ? `0${hours}` : `${hours}`;
    let seconds: number = s % 60;
    let fseconds: string = seconds < 10 ? `0${seconds}` : `${seconds}`;
    return `${fhours}:${fseconds}`;
  }

  function toggleTimer(): void {
    isGoing = !isGoing;
  }

  function resetTimer(): void {
    isGoing = false;
    if (stateSwitchCounter % 2 != 0) {
      remainingTime = focusInterval;
      timerTitle = "Focus";
    } else if (stateSwitchCounter % 6 == 0) {
      remainingTime = longPauseInterval;
      timerTitle = "Long break";
    } else {
      remainingTime = shortPauseInterval;
      timerTitle = "Short break";
    }
  }

  function skipState(): void {
    isGoing = false;
    stateSwitchCounter++;
  }

  // timer controller
  $effect(() => {
    let timer: any;
    if (remainingTime > 0 && isGoing) {
      timer = setInterval(() => (remainingTime -= 1), 1000);
    } else if (remainingTime == 0) {
      stateSwitchCounter++;
    }
    return () => clearInterval(timer);
  });

  // timer state controller
  $effect(() => {
    if (stateSwitchCounter > 1) {
      // TODO: play sound
      resetTimer();
    }
  });

  // onMount(() => console.log(remainingTime));
</script>

<div class="component-box">
  <h1 class="text-center">Focus timer</h1>
  <h2 class="text-center">{timerTitle}</h2>
  <div class="flex flex-col">
    <!-- timer display -->
    <div class="text-6xl text-center my-5 hover:cursor-default">
      {formatedTime}
    </div>
    <!-- timer controls -->
    <div class="flex flex-row justify-around">
      <button class="timer-button" onclick={toggleTimer}>P</button>
      <button class="timer-button" onclick={resetTimer}>R</button>
      <button class="timer-button" onclick={skipState}>S</button>
    </div>
  </div>
</div>
