<script lang="ts">
    import {onMount, tick} from "svelte";
    import type {Module} from "../../../integration/types";
    import {getModules} from "../../../integration/rest";
    import {listen} from "../../../integration/ws";
    import {getTextWidth} from "../../../integration/text_measurement";
    import {flip} from "svelte/animate";
    import {fly} from "svelte/transition";
    import {convertToSpacedString, spaceSeperatedNames} from "../../../theme/theme_config";

    let enabledModules: Module[] = [];

    async function updateEnabledModules() {
        const modules = await getModules();
        const visibleModules = modules.filter(m => m.enabled && !m.hidden);

        const modulesWithWidths = visibleModules.map(module => {
                let formattedName = $spaceSeperatedNames ? convertToSpacedString(module.name) : module.name;
                let fullName = module.tag == null ? formattedName : formattedName + " " + module.tag;

                return {
                    ...module,
                    width: getTextWidth(fullName.toLowerCase(), "400 14px Minecraftia")
                };
            }
        );

        modulesWithWidths.sort((a, b) => b.width - a.width);

        enabledModules = modulesWithWidths;
        await tick();
    }

    spaceSeperatedNames.subscribe(async () => {
        await updateEnabledModules();
    });

    onMount(async () => {
        await updateEnabledModules();
    });

    listen("toggleModule", async () => {
        await updateEnabledModules();
    });

    listen("refreshArrayList", async () => {
        await updateEnabledModules();
    });

    let speed = 10;

    interface RGBColor {
        r: number;
        g: number;
        b: number;
    }

    let progress = 0;
    const tSpeed = (0.1 / 20) * speed;

    export function arraylistGradient() {
        const arraylist = document.getElementById("arraylist");
        if (arraylist == null) return;
        var color1 = window.getComputedStyle(arraylist).getPropertyValue("--accent-color-1").trim();
        var color2 = window.getComputedStyle(arraylist).getPropertyValue("--accent-color-2").trim();
        const modules = arraylist.children as HTMLCollectionOf<HTMLElement>;
        for (let i = 0; i < modules.length; i++) {
            const element = modules[i];
            if (element.id != "module") continue;
            const percentage = 1 - (i / modules.length) + (.5 * Math.sin(.5 * i + progress));;
            const rgb = colorInterpolate(color1, color2, percentage);
            element.style.color = `rgb( ${rgb.r}, ${rgb.g}, ${rgb.b})`;
            element.style.borderRight = `2px solid rgb( ${rgb.r}, ${rgb.g}, ${rgb.b})`;
            element.style.textShadow = `rgb( ${rgb.r}, ${rgb.g}, ${rgb.b}) 0px 0px 4px, rgb( ${rgb.r}, ${rgb.g}, ${rgb.b}) 0px 0px 12px`;
        }

        progress += tSpeed;
        if (progress >= Math.PI * 2) {
            progress = 0;
        }

    }

    export function getRgb(color: string): RGBColor {
        const match = color.match(/^rgb\((\d+),\s*(\d+),\s*(\d+)\)$/);
        if (!match) {
          throw new Error(`Invalid color format: ${color}`);
        }
        return {
          r: parseInt(match[1]),
          g: parseInt(match[2]),
          b: parseInt(match[3]),
        };
    }
    // https://stackoverflow.com/questions/66123016/interpolate-between-two-colours-based-on-a-percentage-value
    export function colorInterpolate(colorA: string, colorB: string, interpolation: number): RGBColor {
        const rgbA = getRgb(colorA);
        const rgbB = getRgb(colorB);

        interpolation = Math.min(1, Math.max(0, interpolation));

        const interpolateComponent = (prop: keyof RGBColor) =>
          Math.round(rgbA[prop] * (1 - interpolation) + rgbB[prop] * interpolation);

        return {
          r: interpolateComponent('r'),
          g: interpolateComponent('g'),
          b: interpolateComponent('b'),
        };
    }

    setInterval(arraylistGradient, speed)
</script>

<div class="arraylist" id="arraylist">
    {#each enabledModules as {name, tag} (name)}
        <div class="module" id="module" animate:flip={{ duration: 200 }} in:fly={{ x: 50, duration: 200 }}>
            {$spaceSeperatedNames ? convertToSpacedString(name) : name}
            {#if tag}
                <span class="tag" id="tag"> {tag}</span>
            {/if}
        </div>
    {/each}
</div>

<style lang="scss">
  @import "../../../colors.scss";

  :root {
    --accent-color-1: #{$accent-color-1};
    --accent-color-2: #{$accent-color-2};
  }

  .module {
    text-transform: lowercase;
    background-color: rgba(0, 0, 0, 0.5);
    color: $accent-color-1;
    border-right: 2px solid $accent-color-1;
    text-shadow: $accent-color-1 0px 0px 4px, $accent-color-1 0px 0px 12px;
    font-size: 14px;
    padding: 0px 6px;
    padding-bottom: 1px;
    width: max-content;
    font-weight: 400;
    margin-left: auto;
  }

  .tag {
    color: rgb(200, 200, 200);
    text-shadow: none;
    text-transform: lowercase;
  }
</style>
