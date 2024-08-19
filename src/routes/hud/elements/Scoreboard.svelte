<script lang="ts">
    import {listen} from "../../../integration/ws";
    import type {PlayerData, Scoreboard} from "../../../integration/types";
    import TextComponent from "../../menu/common/TextComponent.svelte";
    import type {ClientPlayerDataEvent} from "../../../integration/events";

    let scoreboard: Scoreboard | null = null;

    listen("clientPlayerData", (e: ClientPlayerDataEvent) => {
        const playerData: PlayerData = e.playerData;
        scoreboard = playerData.scoreboard;
    });
</script>

{#if scoreboard}
    <div class="scoreboard">
        {#if scoreboard.header}
            <div class="header">
                <TextComponent fontSize={14} allowPreformatting={true} textComponent={scoreboard.header}/>
            </div>
        {/if}
        <div class="entries">
            {#each scoreboard.entries as {name, score}}
                <div class="row">
                    <TextComponent fontSize={14} allowPreformatting={true} textComponent={name}/>
                    <TextComponent fontSize={14} allowPreformatting={true} textComponent={score}/>
                </div>
            {/each}
        </div>
    </div>
{/if}

<style lang="scss">
  @import "../../../colors.scss";

  .scoreboard {
    width: max-content;
    //position: fixed;
    //left: 15px;
    //top: 550px;
    border-right: 2px solid $accent-color-1;
    overflow: hidden;
    font-size: 14px;
  }

  .entries {
    background-color: rgba($scoreboard-base-color, 0.8);
    padding: 4px;
  }

  .row {
    display: flex;
    column-gap: 8px;
    justify-content: space-between;
  }

  .header {
    text-align: center;
    background-color: rgba($scoreboard-base-color, 0.8);
    padding: 4px 10px;
  }
</style>
