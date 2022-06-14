<script lang="ts">
  import Input from "./Input.svelte";
  import { videoFile } from "./stores";
  import defaultVid from './assets/door.webm'
  let recording = false;
  let positions = [];
  let recorderID: number;
  let sampleDelta = 15;
  let video: HTMLVideoElement
  let start: number
  let end: number;
  $videoFile = defaultVid
  


  const getData = ()=> {
    console.log(video.currentTime)
    positions.push(video.currentTime)
  }
  const stopRec = () => {
    recording=false
    clearInterval(recorderID)
    console.log(positions)
  }
  const startRec = () => {
    recording = true
    positions = []
    start = video.currentTime
    recorderID = setInterval(getData, sampleDelta)
  }


</script>

<Input />

{#key $videoFile}
  <div>
    <!-- svelte-ignore a11y-media-has-caption -->
    <video style="height:70vh"controls bind:this={video}><source src={$videoFile} type="video/mp4" /></video>
  </div>
  {#if recording}
  <button on:click={stopRec}>stop</button>
  {:else}
  <button
    on:click={startRec}
    >rec</button
  >
{/if}
<button on:click={()=>{
  let i =0
  // video.play()
  let playInterval =setInterval(()=>{
    video.currentTime= positions[i]
    i+=1
    if(i==positions.length){
      clearInterval(playInterval)
      // video.pause()
    }
  }, sampleDelta)
}}>play</button>
{/key}

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }
</style>
