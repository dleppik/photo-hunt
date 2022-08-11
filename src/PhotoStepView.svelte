<script lang="ts">
    import type {PhotoStep} from "./lib/photoStep";

    export let stepNum: number
    export let allSteps: PhotoStep[]
    export let onFinish: () => void

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

<img class="clue" src={"./clues/"+step.src} alt="clue">

{#if step.type === 'direction'}
    <p>Look for clues in this area</p>
    <button on:click={onFinish}>Ready</button>
{:else}
    <p>Use your own photo app to take the picture.</p>
    <button on:click={onFinish}>Found it</button>
{/if}

