<script lang="ts">
import PudleTitle from "./PudleTitle.svelte";
import RightPanel from "@/lib/components/panels/RightPanel.svelte";
import LeftPanel from "@/lib/components/panels/LeftPanel.svelte";
import Keyboard from "@/lib/components/board/Keyboard.svelte";
import Letterboard from "@/lib/components/board/Letterboard.svelte";
import { fade } from "svelte/transition";
import { onDataLoad, uiState } from "../state/uiState.svelte";
import { setupInitialLoad } from "../state/initialLoadState.svelte";
import { onMount } from "svelte";
import { addMessage, NoticeMessage, noticeState } from "../state/noticeState.svelte";
    import { statsState } from "../state/statsState.svelte";
    import SettingsPanel from "./panels/SettingsPanel.svelte";
    import RightPanelOnlySettingsButton from "./panels/RightPanelOnlySettingsButton.svelte";

const removeLoadingMessage = addMessage(NoticeMessage.Loading);

onMount(async () => {
    try {
        await setupInitialLoad();
    } catch {
        removeLoadingMessage();

        addMessage(NoticeMessage.LoadingFailed);
        return;
    }

    removeLoadingMessage();
    onDataLoad();
});
</script>

<game-page>
    <PudleTitle fades />

    <Keyboard />

    <Letterboard />
    
    {#if !statsState().isFirstGuess}
        <LeftPanel />

        <RightPanel />
    {/if}
    <RightPanelOnlySettingsButton />

    {#if uiState().paused}
        <SettingsPanel />
    {/if}
</game-page>

<style lang="scss">
* {
    transform-style: preserve-3d;
}

game-page {
    display: grid;
    align-items: center;
    justify-items: center;

    grid-template-columns: 15rem auto 15rem;
    grid-template-rows: auto auto auto 5rem;
    margin: 0 -6rem;

    gap: 0 2rem;
}
</style>