<script lang="ts">
  import { onMount } from "svelte";

  import sun_img from "../assets/sun.png";
  import moon_img from "../assets/moon.png";

  let { theme = $bindable<string>(), ...props } = $props();

  function changeTheme(): void {
    if (theme == "light") {
      theme = "dark";
      localStorage.setItem("theme", "dark");
      document.documentElement.classList.add("dark");
    } else {
      theme = "light";
      localStorage.setItem("theme", "light");
      document.documentElement.classList.remove("dark");
    }
  }

  onMount(() => {
    if (!localStorage.getItem("theme")) {
      localStorage.setItem("theme", "dark");
    }
    theme = localStorage.getItem("theme") as string;
    if (theme == "dark") {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
    }
  });
</script>

<div class="fixed top-0 right-0 mt-3 mr-3 rounded-md hover:cursor-pointer">
  <button class="p-1 hover:cursor-pointer" onclick={changeTheme}>
    <img src={theme == "dark" ? sun_img : moon_img} class="size-5" alt="S" />
  </button>
</div>
