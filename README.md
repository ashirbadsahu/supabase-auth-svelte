# supabase Auth using sveltekit



## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in my-app
npm create svelte@latest my-app
cd myapp
```

## Adding Supabase to it

Use this command to apply supabase to your sveltekit app. credits - [svelteadd/supabase](https://github.com/supabase-community/svelte-supabase)

```bash
npx apply supabase-community/svelte-supabase --no-ssh
```

## Install npm and run

To create a production version of your app:

```bash
npm i
npm run dev
```
## Setting up enviorment variables

1. Goto **.env** file in your project

2. Now goto supabase ->your project

3. In the left sidebar goto project settings and then API and copy and paste your 

  1. Project url in "VITE_SUPABASE_URL"

  1. Anon public key in "VITE_SUPABASE_ANON_KEY"

  1. Servicerole secret in "SUPABASE_PRIVATE_KEY"

  1. JWT secret in "SUPABASE_JWT_SECRET"

## Making Login file
Make a **login.svelte** in **src/lib**
Now add the following code there.
>[!Note]  
>While local development disable email conformation in Auth -> Providers -> Email -> Confirm email

```bash
<script>
    import supabase from "$lib/db";
    import { goto } from "$app/navigation";

    let email = "";
    let password = "";

    //to show login and signup button according to usecase
    let newUser = true;
    function handleUser(){
        newUser = false;
    }
    function handlenewUser(){
        newUser = true;
    }
    //SignUp and LogIn functions
    const signUp = async () => {
        let { data, error } = await supabase.auth.signUp({
            email: email,
            password: password,
        });
        // to redirect to home page
        goto('/')
     };

    const signIn = async () => {
   let { data, error } = await supabase.auth.signIn({
   email: email,
   password: password
    })
    //to redirect to home page
    goto("/");
    };
</script>

<div class="input-field">
    <label for="text">Name</label>
    <input type="email" placeholder="enter email" bind:value={email}>
    <label for="email">Email:
        <input type="email" placeholder="enter email" bind:value={email}>
    </label>
    <label for="password">Password:
        <input type="password" placeholder="enter password" bind:value={password}>
    </label>
    {#if password.length < 6}
    <p>Password must be 6 characters long</p> 
    {/if}

    
    {#if newUser}
    <div><button on:click={() => handleUser()}>Already have an account? LogIn</button></div>
        <button on:click={signUp}>SignUp</button>
    {/if}

    {#if !newUser}
    <div><button on:click={() => handlenewUser()}>Don't have an account? SignUp</button></div>
        <button on:click={signIn}>LogIn</button>
    {/if}
    
</div>

```
Also if you want to display userdata in any page first create a **stores.js** file in lib and you can use this code.
```bash
import { writable } from "svelte/store";

// Create the writable store
export const user = writable(false);
```

Now use this code in any page to get userdata something like this
```bash
  <script>
    import Login from "../lib/login.svelte";
    import { user } from "$lib/stores";
    import supabase from "$lib/db";
    import { onMount } from "svelte";

   let isLoggedIn = false;

    //to check if the user is loggedin or not
  const updateUserStatus = (session) => {
    if (session?.user) {
      user.set(session.user);
      isLoggedIn = true;
    } else {
      user.set(null);
      isLoggedIn = false;
    }
  };

  onMount(() => {
    const session = supabase.auth.session();
    updateUserStatus(session);

    supabase.auth.onAuthStateChange((_, session) => {
      updateUserStatus(session);
    });
  });
</script>
{#if isLoggedIn}
 Hii {$user.email}!
{:else}
<Login />
{/if}
```

Hope it helps!!!