<script lang='ts'>
    import * as Card from "$lib/components/ui/card"
    import Label from "$lib/components/ui/label/label.svelte";
    import Input from "$lib/components/ui/input/input.svelte";
    import Button from "$lib/components/ui/button/button.svelte";


    import { session } from '$lib/session';
    import { auth } from '$lib/firebase.client';
    import {
    GoogleAuthProvider,
    signInWithPopup,
    signInWithEmailAndPassword,
    type UserCredential
    } from 'firebase/auth';
    import { goto } from '$app/navigation';

    let email: string = '';
    let password: string = '';

    async function loginWithMail() {
    await signInWithEmailAndPassword(auth, email, password)
    .then((result) => {
        const { user }: UserCredential = result;
        session.set({
        loggedIn: true,
        user: {
        displayName: user?.displayName,
        email: user?.email,
        photoURL: user?.photoURL,
        uid: user?.uid
        }
        });
        goto('/all-blogs');
    })
    .catch((error) => {
        return error;
    });
    }

    async function loginWithGoogle() {
    const provider = new GoogleAuthProvider();
    await signInWithPopup(auth, provider)
    .then((result) => {
        const { displayName, email, photoURL, uid } = result?.user;
        session.set({
        loggedIn: true,
        user: {
        displayName,
        email,
        photoURL,
        uid
        }
        });

        goto('/all-blogs');
    })
    .catch((error) => {
        return error;
    });
    }

</script>

<link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/6.0.1/firebase-ui-auth.css" />

<main class="flex flex-row gap-2 justify-center items-center p-8 w-full">

        <Card.Root class="mt-60 w-[540px]" >
        <Card.Header>
            <div class="flex flex-row items-center justify-center w-full">
                <img class="w-40" src="WeBlog.png" alt="Logo">
            </div>
            <Card.Title>LogIn</Card.Title> 
            <Card.Description><p>Welcome Back Blogger!!!</p></Card.Description>
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
                loginWithMail();
            }}>LogIn</Button>
        </Card.Footer>
        </Card.Root>
</main>