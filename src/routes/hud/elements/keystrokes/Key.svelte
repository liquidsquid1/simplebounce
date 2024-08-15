<script lang="ts">
    import {listen} from "../../../../integration/ws";
    import type {KeyEvent} from "../../../../integration/events";
    import type {MinecraftKeybind} from "../../../../integration/types";

    export let gridArea: string;
    export let key: MinecraftKeybind | undefined;

    let active = false;

    listen("key", (e: KeyEvent) => {
        if (e.key.name !== key?.key.translationKey) {
            return;
        }

        active = e.action === 1 || e.action === 2;
    });
</script>

<div class="key" style="grid-area: {gridArea};" class:active>
    {key?.key.localized ?? "???"}
</div>

<style lang="scss">
  @import "../../../../colors.scss";

  .key {
    height: 48px;
    width: 48px;
    background-color: rgba(24, 24, 24, .68);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    font-weight: 500;
    transition: fade .2s;
    position: relative;
    box-shadow: inset 0 0 0 0 $accent-color-1;
    text-align: center;

    &.active {
      background: linear-gradient(90deg, $accent-color-1, $accent-color-2);
    }
  }
</style>
