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
  //For Logout
  const logout = async () => {
    {
      await supabase.auth.signOut();
    }
    goto("/");
  };
</script>
{#if isLoggedIn}
  Hii {$user.email}!
  <button on:click={() => logout()}>Logout</button>
{:else}
<Login />
{/if}
