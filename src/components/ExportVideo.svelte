 
 <main>
 <h3>Click start to transcode images to mp4 (x264) and play!</h3>
  <video id="output-video" controls></video><br />
  <button id="start-btn" on:click={image2video}>Start</button>
  <p id="message">{message}</p>
 
 </main>
  
  
  <script>
  	export let images;
	let message = "Press start to convert";
	
	
	  
	  
	const {
	  createFFmpeg,
	  fetchFile
	} = FFmpeg;
	const ffmpeg = createFFmpeg({
	  log: true
	});

	const image2video = async () => {
		
		console.log(images)

	  message = 'Loading ffmpeg-core.js';
	  await ffmpeg.load();
	  
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
	  
	  
	
	  
	  /*
	  for (let i = 0; i < 60; i += 1) {
		const num = `00${i}`.slice(-3);
		ffmpeg.FS('writeFile', `tmp.${num}.png`, await fetchFile(`assets/triangle/tmp.${num}.png`));
	  */
	  
	  message = 'Start transcoding...';
	  await ffmpeg.run('-framerate', '0.25', '-pattern_type', 'glob', '-i', '*.jpg', '-i', 'audio.ogg', '-c:a', 'copy', '-shortest', '-c:v', 'libx264',  '-filter_complex', "scale=-2:2*ih,zoompan=z='min(zoom+0.0015,1.5)':d=125:x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)',scale=-2:720", '-r', '25', '-pix_fmt', 'yuv420p', 'out.mp4');
	  
	  
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

	  const video = document.getElementById('output-video');
	  video.src = URL.createObjectURL(new Blob([data.buffer], {
		type: 'video/mp4'
	  }));

	}



  </script>