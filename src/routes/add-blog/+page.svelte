<script lang='ts'>
    import Label from "$lib/components/ui/label/label.svelte";
    import Input from "$lib/components/ui/input/input.svelte";
    import Textarea from "$lib/components/ui/textarea/textarea.svelte";
    import Button from "$lib/components/ui/button/button.svelte";
    import { db } from "$lib/firebase.client";
    import { collection, addDoc, Firestore } from "firebase/firestore"; 
    import { Timestamp } from "firebase/firestore";
    import { doc, setDoc } from "firebase/firestore"; 
    import { session } from "$lib/session";
    import { toast } from "svelte-sonner";
	import { goto } from "$app/navigation";

    let title="";
    let description="";
    let content="";

    

    async function addblog(){
        const docRef = await addDoc(collection(db, "blogs"), {
        title: title,
        email: $session.user?.email,
        description: description,
        date: Timestamp.now(),
        content: content

        });
        console.log("Document written with ID: ", docRef.id);
    
        toast.success("Blog Added", {
        description: Timestamp.now().toDate().toLocaleString(),
        action: {
            label: "Close",
            onClick: () => goto("all-blogs")
        }
        })
    }

</script>
<main class="flex flex-col w-full items-center ">


    <!-- svelte-ignore a11y_missing_attribute -->
    <img src="WeBlog.png" class="w-[300px] mt-[90px]">
    <br>
    <div class="w-[500px]">
       
        <h1 class="text-4xl">Lets Create you blog now....!!!</h1>
        <br>
        <Label for="title">Blog title :</Label>

        <Input bind:value={title} name="title" />

        <Label for="desc">Enter blog desc :</Label>

        <Input bind:value={description} name="desc" />

        <Label for="content">Blog text :</Label>

        <Textarea bind:value={content} name="desc"/>
        <br>
        <Button onclick={()=>addblog()}>Add Blog</Button>

    </div>
    
</main>

    
    
    
    
    
    