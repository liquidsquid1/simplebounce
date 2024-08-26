<script lang="ts">
    import {onMount} from 'svelte';
    import {getSession} from "../../../integration/rest";
    import {listen} from "../../../integration/ws";
    
    let username = "";

    async function refreshSession() {
        const session = await getSession();
        username = session.username;
    }
  
    onMount(async () => {
        await refreshSession();
    });
  
    listen("session", async () => {
        await refreshSession();
    });
  </script>

<span class="watermark">
    <span class="clientName" id="clientName">
        liquidbounce
    </span>
    <span class="username">{username}</span> <span class="fakeuuid">(0000)</span>
</span>

<style lang="scss">

    @import "../../../colors.scss";

    @keyframes cycle {
        0% {color: $accent-color-1}
        50% {color: $accent-color-2}
        100% {color: $accent-color-1}
    }

    .watermark {
        font-size: 16px;
        min-width: 350px;
        background-color: rgba(0, 0, 0, 0.5);
        padding: 4px;
        border-top: 1px solid $accent-color-1;
    }

    .clientName {
        font-weight: 500;
        color: $accent-color-1;
    }

    .username {
        color: white;
    }

    .fakeuuid {
        color: rgb(127, 127, 127);
    }

</style>
