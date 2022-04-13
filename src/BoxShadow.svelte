<script lang="ts">
    import ColorPicker from "./ColorPicker.svelte";
    import Slider from "./Slider.svelte";
    import Toggle from "./Toggle.svelte";

    interface BoxShadow {
        id: number;
        x: number;
        y: number;
        blur: number;
        spread: number;
        inset: boolean;
        color: string;
        opacity: number;
    }

    const MAX_SHADOWS = 12;
    let nextId = 0;

    let shadows: BoxShadow[] = [];
    shadows[0] = newShadow();

    let css = `box-shadow: 0px 0px 0px 0px rgba(0, 0, 0, 1)`;

    function propertyChanged(shadow: BoxShadow, property: string, event: CustomEvent<any>) {
        shadow[property] = event.detail;
        generateCSS();
    }

    function addShadow() {
        if (shadows.length == MAX_SHADOWS) {
            return;
        }

        shadows = [...shadows, newShadow()];
        generateCSS();
    }

    function removeShadow(index) {
        shadows.splice(index, 1);
        shadows = [...shadows];
        generateCSS();
    }

    function newShadow() {
        return {
            id: nextId++,
            x: 0,
            y: 0,
            blur: 0,
            spread: 0,
            inset: false,
            color: "#000000",
            opacity: 100,
        };
    }

    function generateCSS() {
        const cssLines: string[] = [];
        for (const shadow of shadows) {
            cssLines.push(generateShadowCSS(shadow));
        }

        css = `box-shadow: ${cssLines.join(", ")};`;
    }

    function generateShadowCSS(shadow: BoxShadow) {
        const r = parseInt(shadow.color.substring(1, 3), 16);
        const g = parseInt(shadow.color.substring(3, 5), 16);
        const b = parseInt(shadow.color.substring(5, 7), 16);

        const insetString = shadow.inset ? "inset " : "";
        const alpha = shadow.opacity / 100;

        return `${insetString}${shadow.x}px ${shadow.y}px ${shadow.blur}px ${shadow.spread}px rgba(${r}, ${g}, ${b}, ${alpha})`;
    }

    async function copy() {
        await window.navigator.clipboard.writeText(css);
    }
</script>

<div class="box-shadow">
    <div class="controls">
        {#each shadows as shadow, index (shadow.id)}
            <div class="control">
                <div class="horizontal-group">
                    <Slider
                        name={"X Offset"}
                        min={-100}
                        max={100}
                        on:changed={(e) => {
                            propertyChanged(shadow, "x", e);
                        }} />
                    <Slider
                        name={"Y Offset"}
                        min={-100}
                        max={100}
                        on:changed={(e) => {
                            propertyChanged(shadow, "y", e);
                        }} />
                </div>
                <div class="horizontal-group">
                    <Slider
                        name={"Blur radius"}
                        on:changed={(e) => {
                            propertyChanged(shadow, "blur", e);
                        }} />
                    <Slider
                        name={"Spread radius"}
                        on:changed={(e) => {
                            propertyChanged(shadow, "spread", e);
                        }} />
                </div>
                <Toggle
                    name={"Mode"}
                    offName={"Default"}
                    onName={"Inset"}
                    on:changed={(e) => {
                        propertyChanged(shadow, "inset", e);
                    }} />
                <div class="horizontal-group">
                    <ColorPicker
                        name={"Color"}
                        on:changed={(e) => {
                            propertyChanged(shadow, "color", e);
                        }} />
                    <Slider
                        name={"Opacity"}
                        value={100}
                        on:changed={(e) => {
                            propertyChanged(shadow, "opacity", e);
                        }} />
                </div>
                {#if index > 0}
                    <div
                        class="remove"
                        on:click={() => {
                            removeShadow(index);
                        }}>
                        Remove
                    </div>
                {/if}
            </div>
        {/each}
        {#if shadows.length < MAX_SHADOWS}
            <div class="control new" on:click={addShadow}>+</div>
        {/if}
    </div>
    <div class="final">
        <div class="preview">
            <div class="preview-box" style={css} />
        </div>
        <div class="css">
            {css}
            <button on:click={copy}>Copy</button>
        </div>
    </div>
</div>

<style>
    .box-shadow {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        width: 100%;
        height: 100%;
    }

    .controls {
        display: grid;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: repeat(2, 1fr);
        column-gap: 1rem;
        row-gap: 1rem;

        width: 100%;
        height: 70%;

        padding: 1rem;
    }

    .control {
        position: relative;

        background-color: #eee;
        border-radius: 0.2rem;

        padding: 1rem;
    }

    .horizontal-group {
        display: flex;
        column-gap: 1rem;

        width: 100%;
    }

    .remove {
        display: flex;
        align-items: center;
        justify-content: center;

        position: absolute;
        bottom: 0;
        left: 0;

        width: 100%;
        height: 32px;

        background-color: #ffafaf7a;
        color: #ff5a5a;
        border-bottom-left-radius: 0.2rem;
        border-bottom-right-radius: 0.2rem;

        font-weight: bold;

        cursor: pointer;
        user-select: none;
    }

    .remove:hover {
        background-color: #faa1a181;
        border: 1px solid rgb(218, 139, 139);
    }

    .remove:active {
        background-color: #fa91918b;
    }

    .control.new {
        display: flex;
        align-items: center;
        justify-content: center;

        color: #bdbdbd;

        font-weight: bold;
        font-size: 2rem;

        cursor: pointer;
    }

    .final {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 100%;
        height: 30%;

        background-color: #d9d9d9;

        padding: 1rem;
    }

    .preview {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 40%;
        height: 100%;

        padding: 1rem;
    }

    .preview-box {
        width: 50%;
        height: 50%;

        background-color: #a0a0a0;
        border-radius: 0.2rem;
    }

    .css {
        position: relative;

        width: 60%;
        height: 100%;

        background-color: #eee;
        border-radius: 0.2rem;

        padding: 1rem;
    }

    .css button {
        position: absolute;
        bottom: 1rem;
        right: 1rem;

        background-color: #bdbdbd;
        border: none;
        border-radius: 0.2rem;

        padding: 0.5rem;

        cursor: pointer;
        user-select: none;

        transition: transform 0.1s;
    }

    .css button:hover {
        background-color: #b5b5b5;
        border: 1px solid #898989;
        transform: scale(1.05);
    }

    .css button:active {
        background-color: #b0b0b0;
        font-weight: bold;
    }
</style>
