<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let flipKey;
  export let variant;
  export let flipped;

  function handleClick() {
    if (!flipped) dispatch("flip", { flipKey, variant });
  }
</script>

<style>
  .card {
    border: 1px solid #000;
    border-radius: 10px;
    padding: 5px;
    margin-top: 5px;
    height: 30px;
    width: 70px;
  }
</style>

<div class="card" on:click={handleClick}>{flipKey} - {flipped}</div>
