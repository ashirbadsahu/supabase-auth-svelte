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
