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
    handleFetchMovie();
  });
</script>

{#if error}
  <p>Fetching movie data failed</p>
{:else if movie}
  <div transition:fade={{ duration: 300 }}>
    <Navigation movie={movie.original_title} />
    <MovieInfo />
    <MovieInfoBar />
    <Grid header="Actors" />
    <Actor />
  </div>
{/if}

{#if isLoading}
  <Spinner />
{/if}

<style>

</style>
