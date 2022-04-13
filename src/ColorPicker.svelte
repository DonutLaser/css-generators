<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    let realPicker: HTMLInputElement;

    export let name = "";

    let color = "#000000";

    function open() {
        realPicker.click();
    }

    function changed(event: Event) {
        color = (event.target as HTMLInputElement).value;
        dispatch("changed", color);
    }
</script>

<div class="color-picker">
    <div class="name">{name}</div>
    <div class="body" style="background-color: {color}" on:click={open}>
        <input bind:this={realPicker} type="color" on:input={changed} />
    </div>
</div>

<style>
    .color-picker {
        width: inherit;
        height: fit-content;

        margin-bottom: 0.4rem;
    }

    .name {
        margin-bottom: 0.1rem;
    }

    .body {
        width: 100%;
        height: 32px;

        border-radius: 0.2rem;

        cursor: pointer;
    }

    input {
        visibility: hidden;
        width: 100%;
        height: 100%;

        pointer-events: none;
    }
</style>
