 
 <main>


  <button id="start-btn" on:click={image2video}>{@html start}</button>  <span id="message" class="message">{message}</span>

  
  
  <br /> <br />
  <video id="output-video" controls style="display: none"></video>
 
 </main>
  
  
  <script>
  	export let images;
	let message = "";
	let start = "Generate Video"
	
	const {
	  createFFmpeg,
	  fetchFile
	} = FFmpeg;
	const ffmpeg = createFFmpeg({
	  log: true
	});

	const image2video = async () => {
		
	    const video = document.getElementById('output-video');
		video.style.display = 'none';
		
	  start = '<i class="fas fa-spinner fa-spin"></i> &nbsp;Converting...'

	  message = 'Loading ffmpeg-core.js';
	  
	  if(!ffmpeg.isLoaded()){
		  await ffmpeg.load();
	  }
	  
	  
	  message = 'Loading data';
	  ffmpeg.FS('writeFile', 'audio.ogg', await fetchFile('assets/triangle/audio.ogg'));
	  
	  var i = 0;
	  for (const image of images) {
		  const num = `00${i}`.slice(-3);
		  let img = image; // image.split(',')[1]
		  ffmpeg.FS('writeFile', `tmp.${num}.jpg`, await fetchFile(img));
		  i++;
		  console.log(i)
	  }

	  
	  message = 'Creating video... This may take a while.';
	  await ffmpeg.run('-framerate', '0.25', '-pattern_type', 'glob', '-i', '*.jpg', '-i', 'audio.ogg', '-c:a', 'copy', '-shortest', '-c:v', 'libx264',  '-filter_complex', "scale=-2:2*ih,zoompan=z='min(zoom+0.0015,1.5)':d=125:x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)',scale=-2:450", '-r', '25', '-pix_fmt', 'yuv420p', 'out.mp4');
	   
	  // -framerate -> set input framerate
	  // -r -> set output framerate
	  

	  const data = ffmpeg.FS('readFile', 'out.mp4');
	  //ffmpeg.FS('unlink', 'audio.ogg')
	  var i = 0;
	  for (const image2 of images) {
	    const num = `00${i}`.slice(-3);
		ffmpeg.FS('unlink', `tmp.${num}.jpg`);
		i++
	  }

	 
	  video.src = URL.createObjectURL(new Blob([data.buffer], {
		type: 'video/mp4'
	  }));
	  video.style.display = 'block';
	  
	  message = 'Done!'
	  start = 'Generate Video'

	}



  </script>
  
  <style>
	  .message{
		  padding-left: 10px;
	  }
  </style>