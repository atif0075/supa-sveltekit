<script>
  import "../app.css";
  import Navbar from "../components/navbar.svelte";
</script>

<main class=" bg-zinc-900 min-h-screen w-full">
  <Navbar />
  <slot />
</main>

<style>
  *::-webkit-scrollbar {
    width: 0;
  }
</style>
