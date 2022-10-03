


<div class="backdrop">
	
	<div class="wdgt-modal">
		
		<div class="close" on:click="{() => showEditor = false}">&times;</div>
		
		<h3>Edit Scene</h3>
		
		<div></div>
		<br>
		
		<!--
		<div class="alert alert-warning p-2"><strong>Tip:</strong> You can drag and resize the image</div>
	-->
		
		
		<ImageUpload img_width="1920" bind:images="{images}" bind:index="{index}" bind:showEditor="{showEditor}" />
		
		<button class="btn btn-success float-end" on:click="{saveScene}"><i class="fas fa-save"></i> &nbsp;Save</button>
		
		
		<button class="btn btn-outline-danger float-end me-2" on:click="{deleteScene}">Delete</button>
		
		<div class="mt-3"></div>
		
	
		<canvas id="c" width="1920" height="1080" style="border:2px solid #000000;transform: scale(0.4);transform-origin: top left;"></canvas>
		
	<!--	
		<div class="label">Description</div>
			<div class="input-group mb-3">
			<input type="text" class="form-control" bind:value="{textcontent}" />
			<button class="btn btn-outline-secondary" on:click="{updateText}">Update</button>
		</div>
	-->

	
	

		
	</div>
	
	
</div>

<script>
  import ImageUpload from './ImageUpload.svelte'


export let showEditor;
export let images;
export let index;

let canvas;
let text;
let textcontent = "Your description here";


function deleteScene(){
	if(confirm('Are you sure you want to delete this scene?')){
		images.splice(index, 1);
		images = images;
		showEditor = false;
	}
}

import {onMount} from 'svelte'

onMount(() => {
	canvas = new fabric.Canvas('c')
	
	/*
	let rect =  new fabric.Rect({
		left:120, top: 10, fill: 'teal', width: 150, height: 150, angle: 45
	})        
	canvas.add(rect)
	*/
	

	
	fabric.Image.fromURL(images[index], function(img) {
		img.set({ left: 0, top: 0});
		img.scaleToWidth(1920)
		canvas.add(img);   
		canvas.moveTo(img, 0);    
	});
	
	/*
	text = new fabric.Text(textcontent, {
		fill: 'white',
		top: 400,
		left: 20,
		fontFamily: "Helvetica",
		fontSize: 25
	});
	
	canvas.add(text);
	canvas.moveTo(text, 10);
	*/

	

})

	function updateText(){
		text.set('text',textcontent);
		canvas.renderAll();
	}
	
	function saveScene(){
		let data = canvas.toDataURL({format: 'jpeg'});
		console.log(data);
		images[index] = data;
		showEditor = false;
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
	
	.wdgt-modal{
		background-color: white;
		border-radius: 8px;
		padding: 20px;
		max-width: 810px;
		height: 640px;
		margin: 40px auto;
	}
	
	.close{
		float: right;
		font-size: 35px;
		margin-top: -10px;
	}
	
	.close:hover{
		color: #999;
		cursor: pointer;
	}
	
	#c{
		margin-bottom: 10px;
	}
	

	
	h3{
		font-size: 20px;
	}
	

	.label{
		font-size: 14px;
		text-transform: uppercase;
		letter-spacing: 0.03em;
		margin-top: 10px;
		margin-bottom: 5px;
	}
	
	
	
	@media only screen and (max-width: 800px) {
		.wdgt-modal{
			margin: 0;
			height: 100%;
		}
	}
</style>