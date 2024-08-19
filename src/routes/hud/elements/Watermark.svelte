<span class="watermark">
    <span class="coloured">L</span>iquidbounce
</span>

<style lang="scss">

    @import "../../../colors.scss";

    @keyframes cycle {
        0% {color: $accent-color-1}
        50% {color: $accent-color-2}
        100% {color: $accent-color-1}
    }

    .watermark {
        font-size: 24px;
        color: white;
        text-shadow: 3px 3px #000000;
    }
    .coloured {
        animation: linear infinite 5s cycle;
    }
</style>
