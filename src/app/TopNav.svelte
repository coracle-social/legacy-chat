<script lang="ts">
  import {appName, modal} from "src/partials/state"
  import Anchor from "src/partials/Anchor.svelte"
  import TopNavMenu from "src/app/TopNavMenu.svelte"
  import {menuIsOpen} from "src/app/state"
  import {session} from "src/engine"

  const logoUrl = import.meta.env.VITE_LOGO_URL || "/images/logo.png"

  const toggleMenu = () => menuIsOpen.update(x => !x)

  const showLogin = () => modal.push({type: "login/intro"})
</script>

<div
  class="fixed top-0 z-10 flex h-16 w-full items-center justify-between border-b
            border-gray-6 bg-gray-7 px-2 text-gray-2">
  <div>
    <div class="app-logo flex cursor-pointer items-center gap-2" on:click={toggleMenu}>
      <img alt="App Logo" src={logoUrl} class="w-10" />
      <h1 class="staatliches pt-1 text-3xl">{appName}</h1>
    </div>
  </div>
  {#if $session}
    <TopNavMenu />
  {:else}
    <Anchor theme="button-accent" on:click={showLogin}>Log In</Anchor>
  {/if}
</div>
