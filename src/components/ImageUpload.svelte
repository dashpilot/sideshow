<script>
import { onMount } from 'svelte';

export let img_width;
export let images;
let loading = false;

function startUpload(){
	document.getElementById('fileInput').click();

}


onMount(async () => {


document.getElementById('fileInput').addEventListener('change', function(e) {
	
 loading = true;
 


  var img = new Image();
 
  img.onload = function() {
	 
	var canvas = document.createElement('canvas'),
	  ctx = canvas.getContext("2d"),
	  oc = document.createElement('canvas'),
	  octx = oc.getContext('2d');

	canvas.width = img_width; // destination canvas size
	canvas.height = canvas.width * img.height / img.width;

	var cur = {
	  width: Math.floor(img.width * 0.5),
	  height: Math.floor(img.height * 0.5)
	}
	oc.width = cur.width;
	oc.height = cur.height;
	octx.drawImage(img, 0, 0, cur.width, cur.height);
	while (cur.width * 0.5 > img_width) {
	  cur = {
		width: Math.floor(cur.width * 0.5),
		height: Math.floor(cur.height * 0.5)
	  };
	  octx.drawImage(oc, 0, 0, cur.width * 2, cur.height * 2, 0, 0, cur.width, cur.height);
	}
	ctx.drawImage(oc, 0, 0, cur.width, cur.height, 0, 0, canvas.width, canvas.height);
	var base64Image = canvas.toDataURL('image/jpeg')

	console.log(base64Image);
	images.push(base64Image);
	images = images;
	
	loading = false;


  }

  img.src = URL.createObjectURL(e.target.files[0]);
  
  });
});
</script>

<input type="file" id="fileInput" class="fileInput" accept="image/png, image/jpeg, image/gif" style="display: none; " />

<div class="scene button" on:click="{startUpload}">
	<div class="center">
		{#if loading}
		<i class="fas fa-spinner fa-spin"></i>
		{:else}
		<i class="fas fa-plus"></i>
		{/if}
	</div>
</div>

