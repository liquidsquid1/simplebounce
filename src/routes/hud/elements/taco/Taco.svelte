<div class="window">
  <div class="titlebar">
    <span class="title">Thanks for using SimpleBounce</span>
  </div>
  <div class="content">
    <p>If you enjoy this theme, be sure to star it on Github</p>
    <p>You may close this message by turning off the "Taco" section in the HUD module (look at the cool new ClickGUI while you're at it)</p>
  </div>
</div>

<style lang="scss">

  @import "../../../../colors.scss";

  .window {
    width: 480px;
    font-family: "Minecraftia";
  }

  .titlebar {
    background-color: rgb(12, 12, 12);
    border-top: 1px solid $accent-color-1;
    color: white;
    padding: 10px;
    font-size: 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .titlebar .title {
    margin: 0;
  }

  .content {
    padding: 20px;
    background-color: rgb(24, 24, 24);
    color: white;
    border-bottom: 1px solid $accent-color-2;
  }

</style>
