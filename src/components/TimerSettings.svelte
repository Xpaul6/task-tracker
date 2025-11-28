<script lang="ts">
  import gear_icon from "../assets/gear.png";

  let {
    focusInterval = $bindable<number>(),
    shortPauseInterval = $bindable<number>(),
    longPauseInterval = $bindable<number>()
  } = $props();

  let displayFocusInterval = $derived<number>(focusInterval / 60);
  let displayShortInterval = $derived<number>(shortPauseInterval / 60);
  let displayLongInterval = $derived<number>(longPauseInterval / 60);
  let isOpen = $state<boolean>(false);

  $effect(() => {
    focusInterval = displayFocusInterval * 60;
    shortPauseInterval = displayShortInterval * 60;
    longPauseInterval = displayLongInterval * 60;
  });

  function toggleOpen(): void {
    isOpen = !isOpen;
  }
</script>

<div class="h-8 flex flex-row-reverse items-center">
  <button
    class="animated-button relative -right-6 border-none hover:cursor-pointer"
    onclick={toggleOpen}
  >
    <img class="size-4 hover:rotate-90" src={gear_icon} alt="S" />
  </button>
  <div
    class={`absolute top-[75%] md:top-[20%] left-[50%] translate-[-50%] ${isOpen ? "" : "hidden"}  
      shadow-gray-300 shadow-sm rounded-md p-6 z-50 bg-white dark:bg-zinc-900`}
  >
    <div class="flex flex-row justify-between items-center my-1">
      <label for="">Focus</label>
      <input
        class="border-blue-300 border-2 rounded-md p-1 ml-3 max-w-[70px] focus:outline-none"
        type="number"
        min="1"
        bind:value={displayFocusInterval}
      />
    </div>
    <div class="flex flex-row justify-between items-center my-1">
      <label for="">Short break</label>
      <input
        class="border-blue-300 border-2 rounded-md p-1 ml-3 max-w-[70px] focus:outline-none"
        type="number"
        min="1"
        bind:value={displayShortInterval}
      />
    </div>
    <div class="flex flex-row justify-between items-center my-1">
      <label for="">Long break</label>
      <input
        class="border-blue-300 border-2 rounded-md p-1 ml-3 max-w-[70px] focus:outline-none"
        type="number"
        min="1"
        bind:value={displayLongInterval}
      />
    </div>
  </div>
</div>
