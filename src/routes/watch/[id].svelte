<script context="module" lang="ts">
  export const params = ["id"];
</script>

<script>
  import { onMount } from "svelte";

  let apiKey = import.meta.env.VITE_OMDB_API_KEY;
  export let id;
  const url = `https://omdbapi.com/?i=${id}&apikey=${apiKey}`;
  let movieData;
  let watchId = id;
  let type = "tv";

  onMount(async () => {
    try {
      const response = await fetch(url);
      const data = await response.json();
      if ("Title" in data) {
        movieData = data;
      } else {
        console.error("Invalid response format");
      }
    } catch (error) {
      console.error("Error fetching movie data:", error);
    }
    console.log(movieData);
    if (movieData.Type === "movie") {
      console.log("movie");
      type = "movie";
    }
  });
</script>

<main>
  {#if movieData}
    <a href="/"><h2>MovieMate</h2></a>
    <h1>{movieData.Title}</h1>
    <p>{movieData.Plot}</p>
    <iframe
      src="https://vidsrc.to/embed/{type}/{watchId}"
      title="Iframe example"
      frameborder="0" allowfullscreen width="100%" height="700px" class="movie" id="testframe"></iframe>
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  .jw-icon[button=vidsrc]{
    display: none !important;
  }
  iframe{
    border-radius: 2rem;
  }
</style>
