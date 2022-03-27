<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let active = false;

    function onClick() {
        dispatch("click");
    }
</script>

<div class="toolbar-button" on:click={onClick} class:active>
    <slot />
</div>

<style>
    .toolbar-button {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 64px;
        height: 64px;

        background-color: #bdbdbd;

        border-radius: 0.2rem;

        margin-bottom: 1rem;

        cursor: pointer;
        user-select: none;

        transition: transform 0.1s;
    }

    .toolbar-button:not(.active):hover {
        background-color: #b5b5b5;
        border: 1px solid #898989;
        transform: scale(1.05);
    }

    .toolbar-button:not(.active):active {
        background-color: #b0b0b0;
        font-weight: bold;
    }

    .active {
        background-color: #afc3ff;
        color: #5a83ff;
        font-weight: bold;
    }

    .active:hover {
        background-color: #a1b7fa;
        border: 1px solid rgb(139, 161, 218);
        transform: scale(1.05);
    }

    .active:active {
        background-color: #91abfa;
    }
</style>
