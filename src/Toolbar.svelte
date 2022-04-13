<script lang="ts">
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    import type { Tool } from "./scripts/types";
    import ToolbarButton from "./ToolbarButton.svelte";

    export let tools: Tool[] = [];

    $: activeTool = tools[0].name;

    function openTab(toolName: string) {
        activeTool = toolName;
        dispatch("tool-opened", activeTool);
    }
</script>

<div class="toolbar">
    {#each tools as tool}
        <ToolbarButton
            on:click={() => {
                openTab(tool.name);
            }}
            active={activeTool === tool.name}>{tool.title}</ToolbarButton>
    {/each}
</div>

<style>
    .toolbar {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        width: fit-content;
        height: 100%;

        padding-left: 1rem;
        padding-right: 1rem;

        background-color: #eee;
        border-right: 1px solid #ddd;
    }
</style>
