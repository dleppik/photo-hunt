<script lang="ts">
    import type {PhotoStep} from "./lib/photoStep";

    export let stepNum: number
    export let allSteps: PhotoStep[]

    $: step = allSteps[stepNum]
</script>

{#if step}
<div style="display: none">
    <img src={"./clues/"+step.src} alt="" style="width: 1px; height: 1px">
</div>
{/if}