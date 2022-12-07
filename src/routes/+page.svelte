<script>
  import supabase from "$lib/supabaseClient";
  let name = "";
  let email = "";
  let phone = "";

  let addData = async () => {
    let { data, error } = await supabase
      .from("all_users")
      .insert([{ name, email, phone }]);
    if (error) {
      console.log("error", error);
    } else {
      name = "";
      email = "";
      phone = "";
    }
  };
</script>

<main>
  <div class=" w-full h-screen bg-zinc-900 flex justify-center items-center ">
    <div>
      <div class="grid grid-cols-1 md:min-w-[32rem] gap-6">
        <label class="block">
          <span class="text-gray-200">Full name</span>
          <input
            type="text"
            bind:value={name}
            class="
                  mt-1  block w-full rounded-md  py-2.5 px-1 bg-zinc-700
                  border-transparent
                  text-gray-100
                  focus:border-gray-500  focus:ring-0
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
                  mt-1  block  w-full  rounded-md py-2.5 px-1 bg-zinc-700 border-transparent   text-gray-100
                  focus:border-gray-500  focus:ring-0
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
                  mt-1  block  w-full  rounded-md py-2.5 px-1 bg-zinc-700 border-transparent   text-gray-100
                  focus:border-gray-500  focus:ring-0 text-white2
                "
            placeholder="123 456 7890"
          />
        </label>

        <div class="block">
          <div class="mt-2">
            <div class=" flex justify-end">
              <button
                on:click={addData}
                class=" px-4 py-2 bg-green-600 text-white rounded-lg"
                >Submit
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
