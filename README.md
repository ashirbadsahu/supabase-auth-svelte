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
👉Goto **.env** file in your project
👉 Now goto supabase ->your project
👉In the left sidebar goto project settings and then API and copy and paste your 

Project url in "VITE_SUPABASE_URL"
Anon public key in "VITE_SUPABASE_ANON_KEY"
Servicerole secret in "SUPABASE_PRIVATE_KEY"
JWT secret in "SUPABASE_JWT_SECRET"

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
    const signUp = async () => {
        let { data, error } = await supabase.auth.signUp({
            email: email,
            password: password,
        });
        goto('/')
     };

    const signIn = async () => {
   let { data, error } = await supabase.auth.signIn({
   email: email,
   password: password
    })
    goto("/");
    };
</script>

<div class="input-field">
    <label for="email">Email:
        <input type="email" placeholder="enter email" bind:value={email}>
    </label>
    <label for="password">Password:
        <input type="password" placeholder="enter password" bind:value={password}>
    </label>
    {#if password.length < 8}
    <p>Password must be 8 characters long</p> 
    {/if}
    <button on:click={signUp}>SignUp</button>
    <button on:click={signIn}>LogIn</button>    

</div>

```

That's it now you can imoprt this login.svelte to any page. 