<script lang="ts">
    import {listen} from "../../../../integration/ws.js";
    import type {PlayerData} from "../../../../integration/types";
    import {REST_BASE} from "../../../../integration/host";
    import {fly} from "svelte/transition";
    import HealthProgress from "./HealthProgress.svelte";
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
    <div class="targethud" transition:fly={{ y: -10, duration: 200 }}>
        <div class="main-wrapper">
            <div class="avatar">
                <img src="{REST_BASE}/api/v1/client/resource/skin?uuid={target.uuid}" alt="avatar" />
            </div>
            <div class="name">{target.username}</div>
            <div class="health-stats">
                <div class="stat">
                    <div class="value">{(Math.floor(((target.actualHealth + target.absorption) / (target.maxHealth + target.absorption)) * 100))}%</div>
                </div>
            </div>
        </div>

        <HealthProgress maxHealth={target.maxHealth + target.absorption} health={target.actualHealth + target.absorption} />
    </div>
{/if}

<style lang="scss">
    @import "../../../../colors.scss";

    .targethud {
        //position: fixed;
        //top: 50%;
        //left: calc(50% + 20px);
        //transform: translateY(-50%); // overwrites the component transform
        width: 250px;
        height: 70px;
        background: rgba($color: #202020, $alpha: 1.0);
        overflow: hidden;
    }
    .main-wrapper {
        display: grid;
        grid-template-areas:
            "a b d"
            "a c d";
        column-gap: 0px;
        padding: 10px 10px;
    }

    .name {
        grid-area: a;
        padding-left: 65px;
        padding-top: 1px;
        color: $targethud-text-color;
        font-weight: 500;
        align-self: flex-start;
    }

    .health-stats {
        grid-area: b;
        display: flex;
        position: absolute;
        column-gap: 25px;
        top: 35px;
        right: 1px;
        left: 140px;
        z-index: 1;

        .stat {
            .value {
                color: #ffffff;
                font-size: 14px;
                min-width: 18px;
                display: inline-block;
            }
        }
    }

    
    .avatar {
        grid-area: a;
        height: 50px;
        width: 50px;
        scale: 1.3;
        position: relative;
        image-rendering: pixelated;
        background-image: url("/img/steve.png");
        background-repeat: no-repeat;
        background-size: cover;
        overflow: hidden;

        img {
            position: absolute;
            
            left: 118px;
            top: 118px;
        }
    }
</style>
