<script lang="ts">
</script>

<div class="option-bar">
    <slot />
</div>

<style lang="scss">
    @import "../../../../colors";

    .option-bar {
      background-color: rgba($menu-base-color, 0.68);
      padding: 15px 30px;
      display: flex;
      border-radius: 5px;
      align-items: center;
      column-gap: 30px;
      margin-bottom: 25px;
    }
</style>