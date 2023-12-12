<script context="module" lang="ts">
  export const params = ["id"];
</script>

<script>
  import { onMount } from "svelte";

  let apiKey = import.meta.env.VITE_OMDB_API_KEY;
  export let id;
  let movieData;
  let watchId = id;
  let type = "tv";

  const url = `https://omdbapi.com/?i=${watchId}&apikey=${apiKey}`;

  const fetchOMDB = async () => {
    const urlParts = window.location.pathname.split("/");
    const urlType = urlParts.find((part) => part === "watch")
      ? urlParts[urlParts.indexOf("watch") + 1]
      : null;

    if (urlType === "movie") {
      type = urlType;
    }
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
    if (type === "movie") {
      console.log("movie");
    }
  };

  onMount(() => {
    fetchOMDB();
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
      frameborder="0"
      allowfullscreen
      width="100%"
      height="700px"
      class="movie"
      id="testframe"
    ></iframe>
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  iframe {
    border-radius: 2rem;
  }
</style>
