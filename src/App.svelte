<script>
  import ImageUpload from './components/ImageUpload.svelte'
  import ExportVideo from './components/ExportVideo.svelte'
  let images = [];
</script>

<main>

  
 <div class="timeline"> 
  {#each images as image}
  <div class="scene" style="background-image: url({image});"></div>
  {/each}<ImageUpload img_width="800" bind:images="{images}" />
 </div>
  
  
  <ExportVideo />
 

</main>

<style>
main{
  max-width: 800px;
  margin: 40px auto;
}


.timeline{
  margin-bottom: 20px;
}


</style>
