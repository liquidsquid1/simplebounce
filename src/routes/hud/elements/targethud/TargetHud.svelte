<script lang="ts">
    import {listen} from "../../../../integration/ws.js";
    import type {PlayerData} from "../../../../integration/types";
    import {REST_BASE} from "../../../../integration/host";
    import type {TargetChangeEvent} from "../../../../integration/events";

    let target: PlayerData | null = null;
    let visible = true;

    let hideTimeout: number;

    function startHideTimeout() {
        hideTimeout = setTimeout(() => {
            visible = false;
        }, 500);
    }

    listen("targetChange", (data: TargetChangeEvent) => {
        target = data.target;
        visible = true;
        clearTimeout(hideTimeout);
        startHideTimeout();
    });

    startHideTimeout();
</script>

{#if visible && target != null}
    <div class="targethud">
        <div class="main-wrapper">
            <div class="avatar">
                <img src="{REST_BASE}/api/v1/client/resource/skin?uuid={target.uuid}" alt="avatar" />
            </div>
    
            <span class="name">{target.username}</span><span class="health">{Math.round(target.actualHealth)}hp</span>
        </div>    
    </div>
{/if}

<style lang="scss">
    @import "../../../../colors.scss";

    .targethud {
        //position: fixed;
        //top: 50%;
        //left: calc(50% + 20px);
        width: 330px;
        background-color: rgba(36, 36, 36, 1);
        overflow: hidden;
        border-bottom: 6px solid $accent-color-1;
        max-height: 96px;
    }

    .main-wrapper {
        display: grid;
        grid-template-areas:
            "a b d"
            "a c d";
        column-gap: 10px;
        padding-top: 10px;
        padding-bottom: 7px;
        padding-left: 10px;
    }

    .name {
        grid-area: a;
        color: white;
        align-self: flex-start;
        padding-left: 60px;
        padding-top: 5px;
        font-family: 'Minecraftia';
    }

    .health {
        grid-area: a;
        color: $targethud-text-color;
        align-self: flex-start;
        padding-left: 60px;
        padding-top: 30px;
        color: white;
        font-family: 'Minecraftia';
    }

    .avatar {
        grid-area: a;
        height: 50px;
        width: 50px;
        position: relative;
        image-rendering: pixelated;
        background-image: url("/img/steve.png");
        background-repeat: no-repeat;
        background-size: cover;
        overflow: hidden;
        padding-left: 5px;

        img {
            position: absolute;
            scale: 6.25;
            left: 118px;
            top: 118px;
        }
    }
</style>