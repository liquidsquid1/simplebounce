<script lang="ts">

    export let closeOnInternalClick: boolean;

    let expanded = false;
    let selectElement: HTMLElement;
    let headerElement: HTMLElement;

    function handleWindowClick(e: MouseEvent) {
        if (!selectElement.contains(e.target as Node)) {
            expanded = false;
        }
    }

    function handleSelectClick(e:MouseEvent) {
        if (closeOnInternalClick) {
            expanded = !expanded;
        } else {
            if (!expanded) {
                expanded = true;
            } else {
                expanded = !headerElement.contains(e.target as Node);
            }
        }
    }
</script>

<svelte:window on:click={handleWindowClick}/>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="select" class:expanded bind:this={selectElement} on:click={handleSelectClick}>
    <div class="header" bind:this={headerElement}>
        <span class="title">
            <slot name="title"/>
        </span>
        <img src="img/menu/icon-select-arrow.svg" alt="expand">
    </div>
    {#if expanded}
        <div class="options">
            <slot name="options"></slot>
        </div>
    {/if}
</div>

<style lang="scss">
  @import "../../../../../colors.scss";

  .select {
    cursor: pointer;
    min-width: 250px;
    position: relative;

    &.expanded {
      .header {
        border-radius: 5px 5px 0 0;
      }
    }
  }

  .header {
    background-color: $accent-color-1;
    padding: 20px;
    display: flex;
    column-gap: 20px;
    align-items: center;
    justify-content: space-between;
    border-radius: 5px;
    transition: ease border-radius .2s;

    .title {
      color: $menu-text-color;
      font-size: 20px;
      font-weight: 500;

      span {
        font-weight: 600;
      }
    }
  }

  .options {
    position: absolute;
    z-index: 1000;
    width: 100%;
    border-radius: 0 0 5px 5px;
    max-height: 250px;
    overflow: auto;
    background-color: rgba($menu-base-color, 0.9);
  }
</style>