<script>
  import { onMount } from 'svelte';

  import Hero from '../components/Hero.svelte';
  import Search from '../components/Search.svelte';
  import Grid from '../components/Grid.svelte';
  import Thumb from '../components/Thumb.svelte';
  import LoadMoreButton from '../components/LoadMoreButton.svelte';
  import Spinner from '../components/Spinner.svelte';

  import { IMAGE_BASE_URL, BACKDROP_SIZE } from '../config';
  import { fetchMovies } from '../api';

  let movies = { movies: [] };
  let isLoading;
  let searchTerm = '';
  let error;

  const handleFetchMovies = async (loadMore, searchTerm) => {
    try {
      isLoading = true;
      error = false;
      movies = await fetchMovies(movies, loadMore, searchTerm);
      console.log(movies);
    } catch (err) {
      error = true;
    }
    isLoading = false;
  };

  onMount(async () => {
    handleFetchMovies(false, searchTerm);
  });
</script>

{#if error}
  <p>API fetch failed...</p>
{:else}
  {#if movies.heroImage && !searchTerm}
    <Hero
      image={`${IMAGE_BASE_URL}${BACKDROP_SIZE}${movies.heroImage.backdrop_path}`}
      title={movies.heroImage.original_title}
      text={movies.heroImage.overview} />
  {/if}
{/if}

<Search />
<Grid />
<Thumb />
<LoadMoreButton />
<Spinner />

<style>

</style>
