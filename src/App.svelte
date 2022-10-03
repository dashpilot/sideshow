<svelte:head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/500/fabric.min.js"></script>
</svelte:head>

<script>

  import SceneEditor from './components/SceneEditor.svelte'
  import ExportVideo from './components/ExportVideo.svelte'
  let images = [];
  let showEditor = false;
  let index = false;
  
  function openEditor(i){
    showEditor = true;
    index = i;
  }
  
  function addScene(){
    images.push('placeholder.jpg');
    index = parseInt(images.length) - 1;
    console.log(index)
    showEditor = true;
  }
</script>

<main>

  
 <div class="timeline"> 
  {#each images as image, i}
  <div class="scene button" style="background-image: url({image});" on:click="{() => openEditor(i)}">
    <div class="center text-white"><i class="fas fa-cog"></i></div>
  </div>
  {/each}
  <div class="scene button" on:click="{addScene}">
    <div class="center">
    
      <i class="fas fa-plus"></i>

    </div>
  </div>
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
