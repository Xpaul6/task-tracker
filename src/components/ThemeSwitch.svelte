<script>
  import { onMount } from "svelte";

  let { theme = $bindable(), ...props } = $props();

  function changeTheme() {
    if (theme == "light") {
      theme = "dark";
      localStorage.setItem("theme", "dark");
    } else {
      theme = "light";
      localStorage.setItem("theme", "light");
    }
  }

  $effect(() => {
    
  })

  onMount(() => {
    if (!localStorage.getItem("theme")) {
      localStorage.setItem("theme", "dark");
    }
    theme = localStorage.getItem("theme");
  });
</script>

<div class="fixed top-0 right-0 mt-2 mr-2 hover:cursor-pointer">
  <button onclick={changeTheme}> S </button>
</div>
