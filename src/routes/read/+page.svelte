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
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  {#if loading}
    <Loading />
  {/if}
</main>
