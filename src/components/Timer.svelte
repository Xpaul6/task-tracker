<script lang="ts">
  import { onMount } from "svelte";

  import TimerSettings from "./TimerSettings.svelte";

  import sound_fx from "../assets/notification.mp3";
  import play_icon from "../assets/play.svg";
  import pause_icon from "../assets/pause.svg";
  import reset_icon from "../assets/reset.svg";
  import skip_icon from "../assets/skip.svg";

  const notificationFx = new Audio(sound_fx);
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
  let { formatedTimerString = $bindable<string>() } = $props();

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
    formatedTimerString = formatedTime;
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
      notificationFx.play();
    }
  });

  $effect(() => {
    resetTimer();
  });

  onMount(() => {
    formatedTimerString = formatedTime;
  });
</script>

<div class="component-box pt-0">
  <TimerSettings bind:focusInterval bind:shortPauseInterval bind:longPauseInterval/>
  <h1 class="text-center">Focus timer</h1>
  <h2 class="text-center">{timerTitle}</h2>
  <div class="flex flex-col">
    <!-- timer display -->
    <div class="text-6xl text-center my-5 hover:cursor-default">
      {formatedTime}
    </div>
    <!-- timer controls -->
    <div class="flex flex-row justify-around">
      <button class="timer-button" onclick={toggleTimer}>
        <img src={isGoing ? pause_icon : play_icon} alt="P" />
      </button>
      <button class="timer-button" onclick={resetTimer}>
        <img src={reset_icon} alt="R" />
      </button>
      <button class="timer-button" onclick={skipState}>
        <img src={skip_icon} alt="S" />
      </button>
    </div>
  </div>
</div>
