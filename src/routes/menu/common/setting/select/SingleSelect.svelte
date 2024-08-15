<script lang="ts">
    import {createEventDispatcher} from "svelte";
    import GenericSelect from "./GenericSelect.svelte";

    export let options: string[];
    export let value: string;
    export let title: string;

    const dispatch = createEventDispatcher<{
        change: { value: string }
    }>();

    function handleOptionClick(o: string) {
        value = o;
        dispatch("change", {value: o});
    }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<GenericSelect closeOnInternalClick={true}>
    <span slot="title"><span class="title">{title}</span> {value}</span>

    <svelte:fragment slot="options">
        {#each options as o}
            <div on:click={() => handleOptionClick(o)} class="option" class:active={o === value}>{o}</div>
        {/each}
    </svelte:fragment>
</GenericSelect>

<style lang="scss">
  @import "../../../../../colors.scss";

  .title {
    font-weight: 600;
  }

  .option {
    font-weight: 500;
    color: $menu-text-dimmed-color;
    font-size: 20px;
    padding: 15px 20px;
    transition: ease color .2s;

    &:hover {
      color: $menu-text-color;
    }

    &.active {
      color: $accent-color-1;
    }
  }
</style>
