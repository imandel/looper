<script lang="ts">
    let files
    import {  videoFile  } from "./stores";

    $: if (files) {
		console.log(files);

		for (const file of files) {
			// console.log(`${file.name}: ${file.size} bytes`);
			if (file.type == 'video/mp4' || file.type == 'video/webm' ) {
				$videoFile = URL.createObjectURL(file);
				console.log($videoFile);
			}
			else{
				console.log(file);
			}
		}
	}
</script>
<input type="file" bind:files />
