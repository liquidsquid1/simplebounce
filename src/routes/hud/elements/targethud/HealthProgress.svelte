<script lang="ts">
    export let maxHealth: number;
    export let health: number;

    let width = Math.ceil(health / maxHealth * 100);
    $: width = Math.ceil(health / maxHealth * 100);
</script>

<div class="health-progress">
    <div class="thumb" style="width: {width}%;"></div>
</div>

<style lang="scss">;
    @import "../../../../colors.scss";

    .health-progress {
        position: relative;
        bottom: 30px;
        left: 75px;
        border-radius: 0px;
        max-width: 65%;
    }

     .thumb {
       height: 20px;
       background: #c051f3;
       box-shadow: rgba(128,0,255,0.9) 0px 0px 12px;;
       background-size: 400% 400%;
       z-index: 0;
     }
</style>

