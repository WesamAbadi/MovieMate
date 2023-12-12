<script>
  export let selectedMovie;
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  console.log(selectedMovie);

  function closeOverlay() {
    dispatch("closeOverlay");
  }
</script>

<overlay>
  <div class="overlay-container">
    <div class="overlay-content">
      <div class="img"><img src={selectedMovie.Poster} alt="" /></div>
      <div class="movie-info">
        <table>
          <tr>
            <td>Title:</td>
            <td>{selectedMovie.Title}</td>
          </tr>
          <tr>
            <td>IMDB:</td>
            <td
              ><a href={"https://www.imdb.com/title/" + selectedMovie.imdbID}
                >View on IMDB</a
              ></td
            >
          </tr>
          <tr>
            <td>Watch:</td>
            <td>
                <a
                  href={`/watch/${selectedMovie.Type}/${selectedMovie.imdbID}`}
                  >Watch</a
                >
            </td>
          </tr>
          <tr>
            <td>Type:</td>
            <td>{selectedMovie.Type}</td>
          </tr>
          <tr>
            <td>Year:</td>
            <td>{selectedMovie.Year}</td>
          </tr>
        </table>
      </div>
      <button class="close-button" on:click={closeOverlay}>&times;</button>
    </div>
  </div>
</overlay>
