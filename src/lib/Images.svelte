<script>
 import toml from 'toml';
 import Image from './Image.svelte';

 let images = {}
 let filenames = []
 let current = 0
 let currentImage = {title: "loading..."}

 $: {
     if(filenames.length > 0) {
         currentImage = images[filenames[current]]
     }
 }

 fetch('../images.toml').then(async result => {
     const resultText = await result.text();
     const parsed = toml.parse(resultText)
     images = parsed
     filenames = Object.keys(images)
 });

 const canIncrease = () => current < filenames.length
 const canDecrease = () => current > 0
 const increase = () => {
     if(canIncrease())
         current =+ 1
 }

 const decrease = () => {
     if(canDecrease())
         current = current - 1
 }


</script>
<div>
    <div class="row">
        <button on:click="{decrease}">prev</button>
        <button on:click="{increase}">next</button>
    </div>
    <Image image={currentImage}/>
    <p>Images in gallery: {filenames}</p>
</div>

<style>
 .row {
     display: flex;
     justify-content: space-between;

 }
</style>
