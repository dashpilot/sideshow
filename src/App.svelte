<script>
  import ImageUpload from './components/ImageUpload.svelte'
  import SceneEditor from './components/SceneEditor.svelte'
  import ExportVideo from './components/ExportVideo.svelte'
  let images = [];
  let showEditor = false;
  let index = false;
  
  function openEditor(i){
    showEditor = true;
    index = i;
  }
</script>

<main>

  
 <div class="timeline"> 
  {#each images as image, i}
  <div class="scene button" style="background-image: url({image});" on:click="{() => openEditor(i)}">
    <div class="center text-white"><i class="fas fa-cog"></i></div>
  </div>
  {/each}<ImageUpload img_width="1920" bind:images="{images}" />
 </div>
  
  
  <ExportVideo bind:images="{images}" />
    
    
    {#if showEditor}
    <SceneEditor bind:showEditor bind:index bind:images />
    {/if}
    
 

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
