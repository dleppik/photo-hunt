<script lang="ts">
    import type {PhotoStep} from "./lib/photoStep";
    import VideoOverlay from "./VideoOverlay.svelte";
    import Prefetch from "./Prefetch.svelte";

    export let stepNum: number
    export let allSteps: PhotoStep[]
    export let onFinish: () => void
    export let addPhoto: (data) => void

    $: step = allSteps[stepNum]
    $: remainingText = remainingCluesText(stepNum, allSteps)

    // Needs to be a pure function or it won't get rerun
    function remainingCluesText(n: number, ps: PhotoStep[]): string {
        let clueCount = 0;
        console.log("clues from", n)
        for (let i=n; i<ps.length; i++) {
            if (ps[i].type === 'clue') {
                clueCount++
            }
        }
        const s = clueCount === 1 ? "" : "s"
        return `${clueCount} clue${s} left`;
    }
</script>

<p>
    {remainingText}
</p>

{#if step.type === 'direction'}
    <div class="overlay-banner">
        Start Here
    </div>
    <img class="clue" src={"./clues/"+step.src} alt="clue">
    <button on:click={onFinish}>Ready</button>
{:else}
    <VideoOverlay src={"./clues/"+step.src} onFinish={onFinish} addPhoto={addPhoto}/>
{/if}

<Prefetch allSteps={allSteps} stepNum={stepNum + 1} />