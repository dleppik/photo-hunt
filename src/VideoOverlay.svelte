<script lang="ts">
    export let src: string;
    export let onFinish: () => void
    export let addPhoto: (data) => void

    let img
    let video
    let currentImageSrc = src
    let active = false
    let videoWidth = 10
    let videoHeight = 10
    let videoStarted = false

    $: currentImageSrc = src

    function startAction() {
        active = !active
        imageResize()
        if (active) {
            currentImageSrc = src
            startVideo()
        }
        else {
            stopVideo()
        }
    }

    function imageResize() {
        if (img) {
            videoWidth = img.width;
            videoHeight = img.height;
        }
    }

    function handleMouseMove() {
        console.log("move")
    }

    function startVideo() {
        const constraints = {
            audio: false,
            video: {
                height: {min: videoHeight},
                facingMode: "environment"
            }
        }
        if (navigator.mediaDevices.getUserMedia) {
            navigator.mediaDevices.getUserMedia(constraints)
                .then(function (stream) {
                    video.srcObject = stream
                    videoStarted = true
                    decorateImageForVideo()
                    video.width = videoWidth
                    video.height = videoHeight
                })
                .catch((err) => {
                    console.log(err)
                    unDecorateImage()
                    videoStarted = false
                })
        }
    }

    function stopVideo() {
        if (video && video.srcObject) {
            const stream = video.srcObject;
            const tracks = stream.getTracks();
            for (let i = 0; i < tracks.length; i++) {
                tracks[i].stop();
            }
            video.srcObject = null;
            unDecorateImage()
        }
        videoStarted = false
    }

    function saveImage() {
        const canvas = document.createElement("canvas");
        const ctx = canvas.getContext('2d');
        canvas.width = videoWidth
        canvas.height = videoHeight
        ctx.drawImage(video, 0, 0)
        currentImageSrc = canvas.toDataURL("image/png")
        stopVideo()
        active = false
    }

    function decorateImageForVideo() {
        img.style.filter = "contrast(200%) grayscale(100%)"
    }

    function unDecorateImage() {
        img.style.filter = ""
    }

    function handleDone() {
        if (currentImageSrc !== src) {
            addPhoto(currentImageSrc)
        }
        onFinish()
    }
</script>

{#if active}
    <div style="position: absolute; z-index: 10; width: {videoWidth}px; height: {videoHeight}px">
        <video bind:this={video} playsinline autoplay on:click={startAction}
               style="opacity: 0.8"
        ></video>
    </div>
{/if}

<img class="clue" bind:this={img} src={currentImageSrc} alt="clue" on:click={startAction} style="transition-duration: 1s">

{#if !active}
    {#if currentImageSrc === src}
        <p>Use your own photo app to take the picture, or tap on the photo.</p>
    {:else }
        <p>Tap the photo if you want to reshoot.</p>
    {/if}
{/if}
{#if videoStarted}
    <button on:click={saveImage}>Take Picture</button>
{:else }
    <button on:click={handleDone}>Found it</button>
{/if}

