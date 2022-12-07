<script>
  // active class for nav links
  import { onMount } from "svelte";
  let links = [];
  let activeLink = "";
  onMount(() => {
    links = document.querySelectorAll("nav a");
    activeLink = window.location.pathname;
    links.forEach((link) => {
      if (link.getAttribute("href") === activeLink) {
        link.classList.add("bg-zinc-800");
      }
    });
  });

  function handleClick(e) {
    links.forEach((link) => {
      link.classList.remove("bg-zinc-800");
    });
    e.target.classList.add("bg-zinc-800");
  }
</script>

<main>
  <nav
    aria-label="Site Nav"
    class="mx-auto flex max-w-3xl items-center justify-between p-4 bg-zinc-900"
  >
    <a
      on:click={handleClick}
      href="/"
      class="inline-flex h-10 w-10 items-center justify-center rounded-lg bg-zinc-900 text-zinc-100"
    >
      <span class="sr-only">SupaSave</span>
      SupaSave
    </a>

    <ul class="flex items-center gap-2 text-sm font-medium text-gray-500">
      <li class="hidden lg:block">
        <a on:click={handleClick} class="rounded-lg px-3 py-2" href="/">
          Home
        </a>
      </li>

      <li>
        <a on:click={handleClick} class="rounded-lg px-3 py-2" href="/read">
          Read
        </a>
      </li>
      <li>
        <a on:click={handleClick} class="rounded-lg px-3 py-2" href="/update">
          Update
        </a>
      </li>
      <li>
        <a on:click={handleClick} class="rounded-lg px-3 py-2" href="/delete">
          Delete
        </a>
      </li>
    </ul>
  </nav>
</main>
