<script>
  import MovieTile from "../components/MovieTile.svelte";
  import MovieOverlay from "../components/MovieOverlay.svelte";
  let name = "MovieMate";
  import { onMount } from "svelte";
  import axios from "axios";

  let movieList = [];
  let movieData = null;
  let searchValue = "";
  let apiKey = import.meta.env.VITE_OMDB_API_KEY;
  let errors = "";
  let selectedMovie = null;

  const handleInput = (e) => {
    searchValue = e.target.value;
    searchValue?.length > 0 ? searchData() : (errors = "");
  };
  const searchData = async () => {
    const url = `https://omdbapi.com/?s=${searchValue}&apikey=${apiKey}`;

    const response = await axios.get(url);
    if (response.data.Response === "True") {
      movieList = response.data.Search;
      console.log("movieList: ", response.data);
      errors = "";
    } else {
      console.log("error: ", response.data.Error);
      errors = response.data.Error;
      if (response.data.Error === "Too many results.") {
        errors = errors + " Please keep typing!";
      } else if (response.data.Error === "Movie not found!") {
        errors = errors + " Please check your spelling.";
      }
      movieList = [];
    }
  };

  const recommendData = async () => {
    const url =
      "https://api.consumet.org/movies/dramacool/info?id=drama-detail/vincenzo";
    const url2 = "https://vidsrc.to/vapi/movie/new/";
    try {
      const response = await fetch(url2);
      const data = await response.json();
      if ("Title" in data) {
        movieData = data;
      } else {
        console.error("Invalid response format");
      }
    } catch (error) {
      console.error("Error fetching movie data:", error);
    }
  };

  function handleMovieClick(movie) {
    selectedMovie = movie;
    window.scrollTo(0, 0);
    console.log(window.scroll);
  }

  function handleCloseOverlay() {
    selectedMovie = null;
  }

  onMount(() => {
    // recommendData();
  });
</script>

<main style={movieList.length === 0 ? "padding: 22vh 1em" : "padding: 2em"}>
  <h1 class="app-title">
    Welcome to {name}!
  </h1>
  <div class="controlls">
    <input type="text" placeholder="Start typing.." on:input={handleInput} />
    {#if movieList.length > 0}
      <button type="button" on:click={searchData}>Update</button>
    {/if}
  </div>
  {#if selectedMovie}
    <MovieOverlay {selectedMovie} on:closeOverlay={handleCloseOverlay} />
  {/if}
  <div>
    {#if movieList.length > 0}
      <h5>
        ({movieList.length} results)
      </h5>
      <div class="movie-list">
        {#each movieList as movie}
          {#if movie}
            <button class="tile-button" on:click={handleMovieClick(movie)}>
              <MovieTile {movie} />
            </button>
          {:else}
            <p>not found</p>
          {/if}
        {/each}
      </div>
    {:else}
      <p>{errors}</p>
    {/if}
  </div>
  <div class="latest">
    <!-- <h5>Latest</h5> -->
  </div>
</main>
