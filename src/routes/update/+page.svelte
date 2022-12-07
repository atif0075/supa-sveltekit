<script>
  import supabase from "../../lib/supabaseClient";
  import { onMount } from "svelte";
  import Loading from "../../components/loading.svelte";
  let name = "";
  let email = "";
  let phone = "";
  let currentID = "";

  let isOpen = false;
  let allUser = [];
  let loading = true;
  let openClose = () => {
    isOpen = !isOpen;
  };
  let fetchData = async () => {
    const { data, error } = await supabase.from("all_users").select("*");
    if (error) {
      console.log("error", error);
    } else {
      console.log("data", data);
      allUser = data;
      loading = false;
    }
  };
  onMount(async () => {
    fetchData();
  });

  let updateUser = async (id) => {
    isOpen = false;
    loading = true;
    const { data, error } = await supabase
      .from("all_users")
      .update({ name: name, email: email, phone: phone })
      .match({ id: id });
    if (error) {
      console.log("error", error);
      isOpen = false;
    } else {
      loading = false;
      console.log("data", data);
      fetchData();
    }
  };
  let editUser = (id) => {
    currentID = id;
    isOpen = true;
    const user = allUser.find((user) => user.id === id);
    name = user.name;
    email = user.email;
    phone = user.phone;
  };
</script>

<main>
  <div
    class="overflow-x-auto pt-10 container mx-auto flex flex-col items-center justify-center"
  >
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
              <button on:click={() => editUser(user.id)}>
                <svg
                  class="w-6 h-6 text-green-600 cursor-pointer"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                  ><path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                  /></svg
                >
              </button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
    {#if loading}
      <Loading />
    {/if}
    {#if isOpen}
      <main
        class=" flex justify-center items-center h-screen w-full left-0 top-0 fixed z-10 "
      >
        <button
          on:click={openClose}
          class=" w-full h-screen top-0 left-0 fixed bg-black/60 z-[1]"
        />
        <div
          class="rounded-2xl relative z-50 min-w-[32rem] border border-zinc-600 bg-zinc-900 p-8 shadow-lg"
          role="alert"
        >
          <div class="items-center sm:flex">
            <span
              class="inline-flex h-8 w-8 flex-shrink-0 items-center justify-center rounded-full bg-green-400 text-white"
            />

            <p class="mt-3 text-lg text-white font-medium sm:mt-0 sm:ml-3">
              Update the user
            </p>
          </div>

          <div class="grid grid-cols-1 pt-3 w-full gap-6">
            <label class="block">
              <span class="text-gray-200">Full name</span>
              <input
                type="text"
                bind:value={name}
                class="
           mt-1 block w-full rounded-md py-2.5 px-1 bg-zinc-700
           border-transparent
           text-gray-100
           focus:border-gray-500 focus:ring-0
          "
                placeholder="John Doe"
              />
            </label>
            <label class="block">
              <span class="text-gray-200">Email address</span>
              <input
                type="email"
                bind:value={email}
                class="
           mt-1 block w-full rounded-md py-2.5 px-1 bg-zinc-700 border-transparent  text-gray-100
           focus:border-gray-500 focus:ring-0
          "
                placeholder="john@example.com"
              />
            </label>
            <label class="block">
              <span class="text-gray-200">Phone</span>
              <input
                type="number"
                bind:value={phone}
                class="
           mt-1 block w-full rounded-md py-2.5 px-1 bg-zinc-700 border-transparent  text-gray-100
           focus:border-gray-500 focus:ring-0 text-white2
          "
                placeholder="123 456 7890"
              />
            </label>
          </div>

          <div class="mt-6 sm:flex">
            <button
              on:click={updateUser(currentID)}
              class="inline-block w-full rounded-lg bg-green-500 px-5 py-3 text-center text-sm font-semibold text-white sm:w-auto"
            >
              Done
            </button>
          </div>
        </div>
      </main>
    {/if}
  </div>
</main>
