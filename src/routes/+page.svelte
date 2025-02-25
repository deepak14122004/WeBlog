<script lang='ts'>
    import * as Card from "$lib/components/ui/card"
    import Button from "$lib/components/ui/button/button.svelte";
    import { goto } from '$app/navigation';
    
</script>

<!-- svelte-ignore a11y_missing_attribute -->
<div >
        
        <div class="mt-60 flex justify-center items-center">
            <h1 class="text-4xl font-mono">Welcome to </h1>
            <img class="w-60" src="WeBlog.png" alt="Logo">
            
        </div>
        <h1 class="text-center text-3xl m-10">A Simple Blogging site at your fingertips</h1>
        <h2 class="text-center m-5">Please select the option :  </h2>
        <div class="justify-center flex flex-row gap-5 m-5">
            <Button onclick={() => goto("/signup")} class="w-20">SignUp</Button>
            <Button onclick={() => goto("/login")} class=w-20>LogIn</Button>
        </div>
        
            
            
        
    
</div>