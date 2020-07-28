<script>
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition/';

  import { fetchMovie } from '../api';

  import Actor from '../components/Actor.svelte';
  import Grid from '../components/Grid.svelte';
  import Navigation from '../components/Navigation.svelte';
  import MovieInfo from '../components/MovieInfo.svelte';
  import MovieInfoBar from '../components/MovieInfoBar.svelte';
  import Spinner from '../components/Spinner.svelte';

  export let params;

  let isLoading;
  let error;
  let movie;

  const handleFetchMovie = async () => {
    try {
      isLoading = true;
      error = false;
      movie = await fetchMovie(params.id);
      console.log(movie);
    } catch (err) {
      error = true;
      console.log(err);
    }
    isLoading = false;
  };

  onMount(async () => {
    const localMovie = window.localStorage.getItem(params.id);
    if (localMovie) {
      console.log(`Loading movie ${params.id} from cache`);
      movie = JSON.parse(localMovie);
    } else {
      console.log(`Loading movie ${params.id} from API`);
      handleFetchMovie();
    }
  });

  $: {
    if (movie) {
      window.localStorage.setItem(params.id, JSON.stringify(movie));
    }
  }
</script>

{#if error}
  <p>Fetching movie data failed</p>
{:else if movie}
  <div transition:fade={{ duration: 300 }}>
    <Navigation movie={movie.original_title} />
    <MovieInfo {movie} />
    <MovieInfoBar
      time={movie.runtime}
      budget={movie.budget}
      revenue={movie.revenue} />
    <Grid header="Actors">
      {#each movie.actors as actor}
        <Actor {actor} />
      {/each}
    </Grid>
  </div>
{/if}

{#if isLoading}
  <Spinner />
{/if}

<style>

</style>
