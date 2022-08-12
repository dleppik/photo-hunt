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
            <div class="action" on:click={toggle}>Close</div>
            {#each photos as photo}
                <div>
                    <img src={photo} alt="">
                </div>
            {/each}
        {:else}
            <div class="action" on:click={toggle}>Review snapshots</div>
        {/if}
    </div>
{/if}