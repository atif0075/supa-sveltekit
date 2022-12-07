<script>
  import supabase from "../../lib/supabaseClient";
  import { onMount } from "svelte";
  import Loading from "../../components/loading.svelte";
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
  let deleteUser = async (id) => {
    loading = true;
    const { data, error } = await supabase
      .from("all_users")
      .delete()
      .match({ id: id });
    if (error) {
      console.log("error", error);
      loading = false;
    } else {
      loading = false;
      console.log("data", data);
      allUser = allUser.filter((user) => user.id !== id);
    }
  };
</script>

<main>
  <div class="overflow-x-auto pt-10 container mx-auto flex justify-center">
    <table
      class="max-w-3xl w-full divide-y-2 divide-zinc-200 text-sm dark:divide-zinc-700"
    >
      <thead>
        <tr>
          <th
            class="whitespace-nowrap px-4 py-2 text-left font-medium text-zinc-900 dark:text-white"
          >
            Name
          </th>
          <th
            class="whitespace-nowrap px-4 py-2 text-left font-medium text-zinc-900 dark:text-white"
          >
            Email
          </th>
          <th
            class="whitespace-nowrap px-4 py-2 text-left font-medium text-zinc-900 dark:text-white"
          >
            Phone
          </th>
          <th
            class="whitespace-nowrap px-4 py-2 text-left font-medium text-zinc-900 dark:text-white"
          >
            Delete
          </th>
        </tr>
      </thead>

      <tbody class="divide-y divide-zinc-200 dark:divide-zinc-700">
        {#each allUser as user}
          <tr>
            <td
              class="whitespace-nowrap px-4 py-2 font-medium text-zinc-900 dark:text-white"
            >
              {user.name}
            </td>
            <td
              class="whitespace-nowrap px-4 py-2 text-zinc-700 dark:text-zinc-200"
            >
              {user.email}
            </td>
            <td
              class="whitespace-nowrap px-4 py-2 text-zinc-700 dark:text-zinc-200"
            >
              {user.phone}
            </td>
            <td
              class="whitespace-nowrap px-4 py-2 text-zinc-700 dark:text-zinc-200"
            >
              <button on:click={deleteUser(user.id)}>
                <svg
                  class="w-6 h-6 text-red-500 cursor-pointer"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                  ><path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 18L18 6M6 6l12 12"
                  /></svg
                >
              </button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  {#if loading}
    <Loading />
  {/if}
</main>
