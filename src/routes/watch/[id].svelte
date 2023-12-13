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
  let bgURL = ``;

  const url = `https://omdbapi.com/?i=${watchId}&apikey=${apiKey}`;

  const fetchOMDB = async () => {
    const urlParts = window.location.pathname.split("/");
    const urlType = urlParts.find((part) => part === "watch")
      ? urlParts[urlParts.indexOf("watch") + 1]
      : null;

    try {
      const response = await fetch(url);
      const data = await response.json();
      if ("Title" in data) {
        movieData = data;
        bgURL = movieData.Poster;
        document.body.style.backgroundImage = `url('${bgURL}')`;
        applyBodyAfterStyles();
        if (movieData.Type === "movie") type = "movie";
        else type = "tv";
      } else {
        console.error("Invalid response format");
      }
    } catch (error) {
      console.error("Error fetching movie data:", error);
    }
    console.log(movieData);
  };

  const applyBodyAfterStyles = () => {
    const styleElement = document.createElement("style");
    styleElement.textContent = `
      body::after {
        -webkit-backdrop-filter: blur(20px);
        backdrop-filter: blur(20px);
        filter: brightness(0.4);
        content: "";
        display: block;
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
      }
    `;
    document.head.appendChild(styleElement);
  };

  onMount(() => {
    fetchOMDB();
  });
</script>

<div class="header">
  <a href="/">MovieMate</a>
</div>
<main>
  {#if movieData}
    <div class="movie-container">
      <div class="right">
        <h1>{movieData.Title}</h1>
        <p>{movieData.Plot}</p>
      </div>
      <div class="left">
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
      </div>
    </div>
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  .header {
    margin: 1rem 0;
  }
  .movie-container {
    display: flex;
    flex-direction: row-reverse;
  }
  iframe {
    border-radius: 2rem;
    box-shadow: 1px 4px 30px rgb(0 0 0 / 62%);
  }
  h1 {
    margin: 1rem 0;
  }
  .left {
    flex: 3;
  }
  .right {
    flex: 1;
    display: grid;
    align-content: center;
    padding: 15px;
  }
</style>
