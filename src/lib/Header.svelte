<script>
  import { writable } from "svelte/store";
  import IconButton, { Icon } from "@smui/icon-button";
  import { Svg } from "@smui/common/elements";
  import { mdiThemeLightDark } from "@mdi/js";

  export const theme = writable(localStorage.getItem("theme"));
  theme.subscribe((value) => {
    localStorage.setItem("theme", value === "dark" ? "dark" : "light");
  });

  function handleThemeClick() {
    if ($theme === "dark") {
      theme.set("light");
    } else {
      theme.set("dark");
    }
  }

  /* let darkTheme = window.matchMedia("(prefers-color-scheme: dark)").matches; */
</script>

<svelte:head>
  {#if $theme === "dark"}
    <link rel="stylesheet" href="/smui-dark.css" media="all" />
  {:else}
    <link rel="stylesheet" href="/smui.css" media="all" />
  {/if}
</svelte:head>

<div class="pageTitle">
  <h1>QR-Code Generator</h1>
</div>

<div class="themeSwitch">
  <IconButton on:click={handleThemeClick}>
    <Icon component={Svg} viewBox="0 0 24 24">
      <path fill="currentColor" d={mdiThemeLightDark} />
    </Icon>
  </IconButton>
</div>

<style>
  .pageTitle {
    text-align: center;
  }

  .themeSwitch {
    position: absolute;
    top: 0px;
    right: 0px;
    padding: 8px;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
  }
</style>
