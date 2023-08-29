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

