<script lang="ts">
  import type {ComponentType, SvelteComponentTyped} from "svelte"
  import {Route} from "svelte-routing"
  import {base64DecodeOrPlainWebSocketURL} from "src/util/misc"
  import Bech32Entity from "src/app/views/Bech32Entity.svelte"
  import ChatDetail from "src/app/views/ChatDetail.svelte"
  import ChatList from "src/app/views/ChatList.svelte"
  import UserKeys from "src/app/views/UserKeys.svelte"
  import About from "src/app/views/About.svelte"
  import Logout from "src/app/views/Logout.svelte"
  import NotFound from "src/app/views/NotFound.svelte"
  import PersonDetail from "src/app/views/PersonDetail.svelte"
  import RelayDetail from "src/app/views/RelayDetail.svelte"
  import RelayList from "src/app/views/RelayList.svelte"
  import UserProfile from "src/app/views/UserProfile.svelte"
  import UserSettings from "src/app/views/UserSettings.svelte"
  import {storage} from "src/engine"

  const TypedRoute = Route as ComponentType<SvelteComponentTyped>

  let ready = false

  storage.ready.then(() => {
    ready = true
  })
</script>

{#if ready}
  <div class="pt-16 text-gray-2 lg:ml-48">
    <TypedRoute path="/people/:npub" let:params>
      {#key params.npub}
        <PersonDetail npub={params.npub} />
      {/key}
    </TypedRoute>
    <TypedRoute path="/chat" component={ChatList} />
    <TypedRoute path="/chat/:entity" let:params>
      {#key params.entity}
        <ChatDetail entity={params.entity} />
      {/key}
    </TypedRoute>
    <TypedRoute path="/about" component={About} />
    <TypedRoute path="/keys" component={UserKeys} />
    <TypedRoute path="/relays" component={RelayList} />
    <TypedRoute path="/relays/:b64OrUrl" let:params>
      {#key params.b64url}
        <RelayDetail url={base64DecodeOrPlainWebSocketURL(params.b64OrUrl)} />
      {/key}
    </TypedRoute>
    <TypedRoute path="/profile" component={UserProfile} />
    <TypedRoute path="/settings" component={UserSettings} />
    <TypedRoute path="/logout" component={Logout} />
    <TypedRoute path="/:entity" let:params>
      {#key params.entity}
        <Bech32Entity entity={params.entity} />
      {/key}
    </TypedRoute>
    <TypedRoute path="*" component={NotFound} />
  </div>
{/if}
