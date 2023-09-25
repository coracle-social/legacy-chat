<script lang="ts">
  import {fly} from "src/util/transition"
  import {toast, appName} from "src/partials/state"
  import Field from "src/partials/Field.svelte"
  import FieldInline from "src/partials/FieldInline.svelte"
  import Toggle from "src/partials/Toggle.svelte"
  import Input from "src/partials/Input.svelte"
  import Anchor from "src/partials/Anchor.svelte"
  import Content from "src/partials/Content.svelte"
  import Heading from "src/partials/Heading.svelte"
  import {getSettings, publishSettings} from "src/engine"

  let values = getSettings()

  const submit = () => {
    publishSettings(values)

    toast.show("info", "Your settings have been saved!")
  }

  document.title = "Settings"
</script>

<form on:submit|preventDefault={submit} in:fly={{y: 20}}>
  <Content>
    <div class="mb-4 flex flex-col items-center justify-center">
      <Heading>App Settings</Heading>
      <p>Make {appName} work the way you want it to.</p>
    </div>
    <div class="flex w-full flex-col gap-8">
      <FieldInline label="Show images and link previews">
        <Toggle bind:value={values.show_media} />
        <p slot="info">
          If enabled, {appName} will automatically retrieve a link preview for the last link in any note.
        </p>
      </FieldInline>
      <Field label="Imgproxy URL">
        <Input bind:value={values.imgproxy_url}>
          <i slot="before" class="fa-solid fa-image" />
        </Input>
        <p slot="info">
          Enter a custom imgproxy url for resizing images on the fly to reduce bandwidth and improve
          privacy. You can set up your own proxy <Anchor href="https://imgproxy.net/">here</Anchor>.
        </p>
      </Field>
      <Field label="Report errors and analytics">
        <Toggle bind:value={values.report_analytics} />
        <p slot="info">
          Keep this enabled if you would like developers to be able to know what features are used,
          and to diagnose and fix bugs.
        </p>
      </Field>
      <Anchor tag="button" theme="button" type="submit" class="text-center">Save</Anchor>
    </div>
  </Content>
</form>
