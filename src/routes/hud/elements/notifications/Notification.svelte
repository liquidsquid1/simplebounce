<script lang="ts">
    export let title: string;
    export let message: string;
</script>

<div class="notification">
    <div class="title">{title}</div>
    <div class="message">{message}</div>
</div>

<style lang="scss">

  @import "../../../../colors.scss";

  .notification {
    display: flex;
    flex-flow: row wrap;
    border-left: 2px solid $accent-color-1;
    column-gap: 10px;
    background: rgba(24, 24, 24, 1.0);
    width: 250px;
    overflow: hidden;
    padding: 10px;
    margin-bottom: 10px;
  }

  .title {
    font-size: 12px;
    color: white;
    text-shadow: $accent-color-1 0px 0px 12px;
  }

  .message {
    font-size: 12px;
    color: #cbd1e3;
  }
</style>
