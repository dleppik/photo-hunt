<script lang="ts">
  import Instructions from "./Instructions.svelte";
  import Done from "./Done.svelte";
  import type {PhotoStep} from "./lib/photoStep";
  import PhotoStepView from "./PhotoStepView.svelte";
  import SnapshotViewer from "./SnapshotViewer.svelte";
  import Prefetch from "./Prefetch.svelte";

  const photos: PhotoStep[] = [
      {type: "direction", src: "IMG_6551.jpeg"},
      {type: "clue", src: "IMG_6552.jpeg"},
      {type: "clue", src: "IMG_6553.jpeg"},
      {type: "clue", src: "IMG_6554.jpeg"},
      {type: "clue", src: "IMG_6555.jpeg"},
      {type: "direction", src: "IMG_6556.jpeg"},
      {type: "clue", src: "IMG_6557.jpeg"},
      {type: "clue", src: "IMG_6558.jpeg"},
      {type: "clue", src: "IMG_6559.jpeg"},
      {type: "clue", src: "IMG_6560.jpeg"},
      {type: "clue", src: "IMG_6561.jpeg"},
      {type: "clue", src: "IMG_6562.jpeg"},
      {type: "clue", src: "IMG_6563.jpeg"},
      {type: "clue", src: "IMG_6564.jpeg"},
      {type: "clue", src: "IMG_6565.jpeg"},
      {type: "clue", src: "IMG_6566.jpeg"},
      {type: "clue", src: "IMG_6567.jpeg"},
      {type: "clue", src: "IMG_6568.jpeg"},
      {type: "direction", src: "IMG_6569.jpeg"},
      {type: "clue", src: "IMG_6574.jpeg"},
      {type: "clue", src: "IMG_6577.jpeg"},
      {type: "clue", src: "IMG_6576.jpeg"},
      {type: "clue", src: "IMG_6580.jpeg"},
      {type: "clue", src: "IMG_6575.jpeg"},
      {type: "clue", src: "IMG_6582.jpeg"},
  ]

  let snapshots = []

  let started = false
  let photoIndex = 0

  function onStart() {
    started = true
    photoIndex = 0
  }

  function nextPhoto() {
    photoIndex++;
  }

  function startOver() {
    started = false
    photoIndex = 0
  }

  function addSnapshot(photoData) {
      snapshots.push(photoData)
      snapshots = snapshots // trigger update
  }
</script>

<main>
  {#if !started}
      <Instructions/>
      <button on:click={onStart}>Start</button>
      <Prefetch allSteps={photos} stepNum={0} />
    {:else if (photoIndex < photos.length)}
      <PhotoStepView stepNum={photoIndex} allSteps={photos} onFinish={nextPhoto} addPhoto={addSnapshot}/>
      <SnapshotViewer photos={snapshots} />
  {:else}
      <Done/>
      {#each snapshots as photo}
          <div>
              <img src={photo} alt="">
          </div>
      {/each}  {/if}
</main>