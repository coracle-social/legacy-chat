<script lang="ts">
  import {navigate} from "svelte-routing"
  import {modal} from "src/partials/state"
  import OverflowMenu from "src/partials/OverflowMenu.svelte"
  import {env, session, canSign} from "src/engine"

  export let pubkey

  const isSelf = $session?.pubkey === pubkey

  let actions = []

  $: {
    actions = []

    if ($env.FORCE_RELAYS.length === 0) {
      actions.push({onClick: openProfileInfo, label: "Details", icon: "info"})
    }

    if (isSelf && $canSign) {
      actions.push({
        onClick: () => navigate("/profile"),
        label: "Edit",
        icon: "edit",
      })
    }
  }

  const openProfileInfo = () => modal.push({type: "person/info", pubkey})
</script>

<div class="flex items-center gap-3" on:click|stopPropagation>
  <OverflowMenu {actions} />
</div>
