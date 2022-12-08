<script>
  import supabase from "../../lib/supabaseClient";
  import { onMount } from "svelte";
  import Loading from "../../components/loading.svelte";
  import autoAnimate from "@formkit/auto-animate";
  let allUser = [];
  let loading = true;
  onMount(async () => {
    // read all users
    const { data, error } = await supabase.from("all_users").select("*");
    if (error) {
      console.log("error", error);
    } else {
      console.log("data", data);
      allUser = data;
      loading = false;
    }
  });
  let sortByName = () => {
    allUser = allUser.sort((a, b) => {
      if (a.name < b.name) {
        return -1;
      }
      if (a.name > b.name) {
        return 1;
      }
      return 0;
    });
  };
</script>

<main>
  <div class="container mx-auto flex flex-col items-center justify-center">
    <button on:click={sortByName}>Sort by name</button>
    <table class="max-w-2xl w-full text-sm">
      <div class=" sticky top-0 left-0 z-10">
        <div
          class="bg-zinc-700  border border-zinc-500 border-opacity-40 grid grid-cols-3 rounded-lg "
        >
          <div
            class="whitespace-nowrap px-4 py-3   text-left font-medium text-zinc-900 dark:text-white"
          >
            Name
          </div>
          <div
            class="whitespace-nowrap px-4 py-3 text-left font-medium text-zinc-900 dark:text-white"
          >
            Email
          </div>
          <div
            class="whitespace-nowrap px-4 py-3  text-left font-medium text-zinc-900 dark:text-white"
          >
            Phone
          </div>
        </div>
      </div>

      <tbody use:autoAnimate class=" rounded-lg overflow-hidden">
        {#each allUser as user (user)}
          <div
            class=" bg-zinc-800 border border-zinc-700 my-1 grid grid-cols-3 w-full rounded-lg"
          >
            <div
              class="whitespace-nowrap my-1 px-4 py-2 font-medium  text-zinc-100"
            >
              {user.name}
            </div>
            <div class="whitespace-nowrap px-4 py-2 text-zinc-300">
              {user.email}
            </div>
            <div class="whitespace-nowrap px-4 py-2 text-zinc-300">
              {user.phone}
            </div>
          </div>
        {/each}
      </tbody>
    </table>
  </div>
  {#if loading}
    <Loading />
  {/if}
</main>
