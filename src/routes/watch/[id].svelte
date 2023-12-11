<!-- src/routes/page/[id].svelte -->
<script context="module" lang="ts">
  export const params = ['id'];
</script>

<script>
  export let id;

  // You can use the id variable in your component
</script>

<main>
  <h1>{id}</h1>
  pagepage
</main>

<style>
  /* Your styles go here */
</style>
