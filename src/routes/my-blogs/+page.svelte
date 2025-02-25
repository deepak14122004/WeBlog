<script lang="ts">
    import * as Card from "$lib/components/ui/card";
    import * as Avatar from "$lib/components/ui/avatar";
    import { db } from "$lib/firebase.client";
    import { collection, getDocs } from "firebase/firestore";
    import { onMount } from "svelte";
    import Button from "$lib/components/ui/button/button.svelte";
    import { session } from "$lib/session";
    import { goto } from "$app/navigation";
    import { where } from "firebase/firestore";
    import { query } from "firebase/firestore";

    let blogs :any = [];
    onMount(async () => {
        
        const querySnapshot = await getDocs(query(collection(db, "blogs"),where("email","==",$session.user?.email)));
        blogs = querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
        blogs = [...blogs]; 


    })
</script>

<main>
    
    <div class="flex flex-row items-center justify-between">
        <div class="flex flex-row m-5 gap-5 items-center">
            <div>
                <img class="w-[120px]" src="WeBlog.png" alt="logo">
            </div>
            <div>
                <Button onclick={() => goto("/all-blogs")}>Home</Button>
            </div>

            <div>
                <Button onclick={() => goto("/my-blogs")}>My Blogs</Button>
            </div>
            <div>
                <Button onclick={() => goto("/add-blog")}>Add Blog</Button>
            </div>
        </div>
        <div>
            <h1 class="mr-10"> Hello {$session.user?.email}</h1>
        </div>
    </div>
    <div class=" m-11">
        <h1 class="text-4xl">Your Blogs!!!!!</h1>
        <br>

    {#if blogs.length===0}
    <div class="flex items-center">
        <h1 class="text-4xl w-full text-center">Oops!<br>Ig you are a new user <br>Try creating a blog....!!!!!</h1>
    </div>
    {:else}
    {#each blogs as blog}
    
        <div class="m-5">
            <Card.Root>
                <Card.Header>
                    <Avatar.Root>
                        <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
                        <Avatar.Fallback>{blog.email}</Avatar.Fallback>
                    </Avatar.Root>
                    <br>
                    <Card.Title>{blog.title}</Card.Title>
                    <Card.Description>{blog.description}</Card.Description>
                </Card.Header>
                <Card.Content>
                    {blog.content}
                </Card.Content>
                <Card.Footer>
                    {blog.date.toDate().toLocaleString()}
                </Card.Footer>
            </Card.Root>
        </div>

        {/each}


    {/if}
        

    </div>
</main>
