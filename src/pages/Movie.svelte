<script>
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition/';

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
</script>

<div transition:fade={{ duration: 300 }}>
  <Navigation />
  <MovieInfo />
  <MovieInfoBar />
  <Grid header="Actors" />
  <Actor />
  <Spinner />
</div>

<style>

</style>
