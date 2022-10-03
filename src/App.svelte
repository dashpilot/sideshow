<script>

  import SceneEditor from './components/SceneEditor.svelte'
  import ExportVideo from './components/ExportVideo.svelte'
  let scenes = [];
  let showEditor = false;
  let showExport = false;
  let index = false;
  
  function openEditor(i){
    showEditor = true;
    index = i;
  }
  
  function addScene(){
    scenes.push({"image": "", "title": "Title", "description": "Lorem ipsum dolor slide amet", "rendered": ""});
    index = parseInt(scenes.length) - 1;
    console.log(index)
    showEditor = true;
  }
</script>



<nav class="navbar navbar-dark bg-dark ps-3">
  <div class="row w-100">
    <div class="col-6 brand"><a class="navbar-brand" href="#">StorySlider</a></div>
    <div class="col-6 text-end"><button class="btn btn-success" on:click="{() => showExport = true}">Export Video</button></div>
  </div>
</nav>


  
 <div class="timeline"> 
  {#each scenes as scene, i}
  <div class="scene button" style="background-image: url({scene.rendered});" on:click="{() => openEditor(i)}">
    <div class="center text-white"><i class="fas fa-cog"></i></div>
  </div>
  {/each}
  <div class="scene button" on:click="{addScene}">
    <div class="center">
    
      <i class="fas fa-plus"></i>

    </div>
  </div>
 </div>


<main>
  
  
    
    
    {#if showEditor}
    <SceneEditor bind:showEditor bind:index bind:scenes />
    {/if}
    
    
    {#if showExport}
    <ExportVideo bind:scenes="{scenes}" bind:showExport="{showExport}" />
    {/if}
    
 

</main>

<style>
main{
  max-width: 800px;
  margin: 40px auto;
}

.timeline{
  padding: 10px 10px 0 15px;
  margin-bottom: 20px;
  width: 100%;
  border-bottom: 3px solid #999;
}

.brand{
  padding-top: 2px;
}
</style>
