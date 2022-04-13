<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let name = "";
    export let min = 0;
    export let max = 100;
    export let value = 0;

    let lastDragX = 0;

    function change(dir: number) {
        value += dir;

        if (value < min) {
            value = min;
        }
        if (value > max) {
            value = max;
        }

        changeTo(value);
    }

    function changeTo(val: number) {
        value = val;
        dispatch("changed", value);
    }

    function startDragging(event: MouseEvent) {
        event.preventDefault();
        event.stopPropagation();

        if (event.ctrlKey) {
            changeTo(0);
        } else {
            lastDragX = event.clientX;
            document.addEventListener("mousemove", drag);
            document.addEventListener("mouseup", stopDragging);
            document.body.style.cursor = "ew-resize";
        }
    }

    function drag(event: MouseEvent) {
        const delta = event.clientX - lastDragX;
        change(Math.round(delta / 3));

        lastDragX = event.clientX;
    }

    function stopDragging() {
        document.removeEventListener("mousemove", drag);
        document.removeEventListener("mouseup", stopDragging);

        document.body.style.cursor = "auto";
    }
</script>

<div class="slider">
    <div class="name">{name}</div>
    <div class="body">
        <div
            class="button decrease"
            on:click={() => {
                change(-1);
            }}>
            &lt;
        </div>
        <div class="value" on:mousedown={startDragging}>{value}</div>
        <div
            class="button increase"
            on:click={() => {
                change(1);
            }}>
            &gt;
        </div>
    </div>
</div>

<style>
    .slider {
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
        align-items: center;
        justify-content: center;

        width: 32px;
        height: 100%;

        background-color: #bdbdbd;

        user-select: none;
    }

    .button:hover {
        background-color: #b5b5b5;
        border: 1px solid #898989;

        cursor: pointer;
    }

    .button:active {
        background-color: #b0b0b0;
        font-weight: bold;
    }

    .button.decrease {
        border-top-left-radius: 0.2rem;
        border-bottom-left-radius: 0.2rem;
    }

    .button.increase {
        border-top-right-radius: 0.2rem;
        border-bottom-right-radius: 0.2rem;
    }

    .value {
        display: flex;
        flex: 1;
        align-items: center;
        justify-content: center;

        height: 100%;

        background-color: #cfcece;
    }

    .value:hover {
        cursor: ew-resize;
    }
</style>
