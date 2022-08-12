<script lang="ts">
    export let photos: []

    let open = false

    function toggle() {
        open = !open
    }
</script>

{#if photos.length > 0}
    <div class="snapshot-viewer">
        {#if open}
            <a on:click={toggle}>Close</a>
            {#each photos as photo}
                <div>
                    <img src={photo}>
                </div>
            {/each}
        {:else}
            <a on:click={toggle}>Review snapshots</a>
        {/if}
    </div>
{/if}