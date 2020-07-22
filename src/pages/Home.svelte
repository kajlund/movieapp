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

  const handleSearch = event => {
    searchTerm = event.detail.searchText;
    movies.movies = [];
    handleFetchMovies(false, searchTerm);
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

<Search on:search={handleSearch} />
<Grid header={searchTerm ? 'Search Results' : 'Popular Movies'}>
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
</Grid>

<Thumb />
<LoadMoreButton />
<Spinner />

<style>

</style>
