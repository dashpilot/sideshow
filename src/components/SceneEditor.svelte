<div class="backdrop">
	
	<div class="modal">
		
		<div class="close" on:click="{() => showEditor = false}">&times;</div>
		
		<h3>Edit Scene</h3>
		
		<img src="{images[index]}" />
		
		<button on:click="{deleteScene}">Delete</button>
		
	</div>
	
	
</div>

<script>
export let showEditor;
export let images;
export let index;


function deleteScene(){
	if(confirm('Are you sure you want to delete this scene?')){
		images.splice(index, 1);
		images = images;
		showEditor = false;
	}
}
</script>


<style>
	.backdrop{
		background-color: rgba(0,0,0,0.5);
		position:fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}	
	
	.modal{
		background-color: white;
		border-radius: 8px;
		padding: 20px;
		width: 800px;
		min-height: 70%;
		margin: 40px auto;
	}
	
	.close{
		float: right;
		font-size: 35px;
	}
	
	.close:hover{
		color: #999;
		cursor: pointer;
	}
	
	img{
		max-width: 100%;
	}
</style>