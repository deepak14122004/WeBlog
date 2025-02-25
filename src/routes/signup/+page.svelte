<script lang='ts'>
    import * as Card from "$lib/components/ui/card"
    import Label from "$lib/components/ui/label/label.svelte";
    import Input from "$lib/components/ui/input/input.svelte";
    import Button from "$lib/components/ui/button/button.svelte";

    import { auth } from '$lib/firebase.client';
    import { createUserWithEmailAndPassword } from 'firebase/auth';
    import { goto } from '$app/navigation';
    import { session } from '$lib/session';

    let email: string = '';
    let password: string = '';

    async function handleRegister() {
    await createUserWithEmailAndPassword(auth, email, password)
    .then((result) => {
        const { user } = result;
        session.update((cur: any) => {
        return {
        ...cur,
        user,
        loggedIn: true,
        loading: false
        };
        });
        goto('/all-blogs');
    })
    .catch((error) => {
        throw new Error(error);
    });
 }

</script>

<main class="flex flex-row gap-2 justify-center items-center p-8 w-full">

    <Card.Root class="mt-60 w-[540px]" >
    <Card.Header>
        <div class="flex flex-row items-center justify-center w-full">
            <img class="w-40" src="WeBlog.png" alt="Logo">
        </div>
        <Card.Title>SignUp</Card.Title> 
        <Card.Description><p>New here HUH!!!</p></Card.Description>
    </Card.Header>
    <Card.Content>
        

        <Label for="email">Enter Email :</Label>
        
        <Input type="email" bind:value={email} name="email" />
        <br>
        <Label for="password">Enter Password :</Label>
        
        <Input type="password" bind:value={password} name="password" />
        
    </Card.Content>
    <Card.Footer>
        <Button onclick={()=>{
           handleRegister() }}>SignUp</Button>
    </Card.Footer>
    </Card.Root>
</main>