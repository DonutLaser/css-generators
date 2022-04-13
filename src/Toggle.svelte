<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let name = "";
    export let offName = "false";
    export let onName = "true";

    let on = false;

    function change(value: boolean) {
        on = value;
        dispatch("changed", on);
    }
</script>

<div class="toggle">
    <div class="name">{name}</div>
    <div class="body">
        <div
            class="button off"
            class:active={!on}
            on:click={() => {
                change(false);
            }}>
            {offName}
        </div>
        <div
            class="button on"
            class:active={on}
            on:click={() => {
                change(true);
            }}>
            {onName}
        </div>
    </div>
</div>

<style>
    .toggle {
        width: inherit;
        height: fit-content;

        margin-bottom: 0.4rem;
    }

    .name {
        margin-bottom: 0.1rem;
    }

    .body {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 100%;
        height: 32px;
    }

    .button {
        display: flex;
        flex: 1;
        align-items: center;
        justify-content: center;

        height: 100%;

        background-color: #bdbdbd;

        cursor: pointer;
        user-select: none;
    }

    .on {
        border-top-right-radius: 0.2rem;
        border-bottom-right-radius: 0.2rem;
    }

    .off {
        border-top-left-radius: 0.2rem;
        border-bottom-left-radius: 0.2rem;
    }

    .button:not(.active):hover {
        background-color: #b5b5b5;
        border: 1px solid #898989;
    }

    .button:not(.active):active {
        background-color: #b0b0b0;
        font-weight: bold;
    }

    .button.active {
        background-color: #afc3ff;
        color: #5a83ff;
        font-weight: bold;
    }

    .button.active:hover {
        background-color: #a1b7fa;
        border: 1px solid rgb(139, 161, 218);
    }

    .button.active:active {
        background-color: #91abfa;
    }
</style>
