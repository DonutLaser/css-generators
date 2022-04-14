<script lang="ts">
    import BoxShadow from "./BoxShadow.svelte";
    import TextShadow from "./TextShadow.svelte";

    import Main from "./Main.svelte";
    import type { Tool } from "./scripts/types";

    import Toolbar from "./Toolbar.svelte";

    const tools: Tool[] = [
        { name: "box-shadow", title: "BS", component: BoxShadow },
        { name: "text-shadow", title: "TS", component: TextShadow },
        { name: "gradient", title: "Grdnt", component: null },
        { name: "font", title: "Fnt", component: null },
        { name: "grid", title: "Grd", component: null },
        { name: "borders", title: "Brdrs", component: null },
        { name: "animations", title: "Anmtn", component: null },
    ];

    let activeTool = tools[0];

    function onToolOpened(event: CustomEvent<string>) {
        const tool = event.detail;
        activeTool = tools.find((t) => t.name === tool);
    }
</script>

<div class="app">
    <Toolbar {tools} on:tool-opened={onToolOpened} />
    <Main>
        <svelte:component this={activeTool.component} />
    </Main>
</div>

<style>
    .app {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 100%;
        height: 100%;
    }
</style>
