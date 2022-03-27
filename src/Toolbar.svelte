<script lang="ts">
    import ToolbarButton from "./ToolbarButton.svelte";

    const tabs = [
        { name: "box-shadow", title: "BS" },
        { name: "text-shadow", title: "TS" },
        { name: "gradient", title: "Grdnt" },
        { name: "font", title: "Fnt" },
        { name: "grid", title: "Grd" },
        { name: "borders", title: "Brdrs" },
        { name: "animations", title: "Anmtin" },
    ];

    let activeTab = "box-shadow";

    function openTab(tab: string) {
        activeTab = tab;
    }
</script>

<div class="toolbar">
    {#each tabs as tab}
        <ToolbarButton
            on:click={() => {
                openTab(tab.name);
            }}
            active={activeTab === tab.name}>{tab.title}</ToolbarButton>
    {/each}
</div>

<style>
    .toolbar {
        position: absolute;
        top: 50%;
        left: 1rem;
        transform: translateY(-50%);

        width: fit-content;
        height: fit-content;
    }
</style>
