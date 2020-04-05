<script>
  import { onMount } from "svelte";
  import FlipCard from "./components/flipCard.svelte";

  onMount(async () => {
    initGame();
  });

  const flipKeys = ["apples", "oranges", "melons"];
  let flippedKey = "";
  let flippedVariant = "";
  let cards = [];
  let boardState = [];
  let matched = [];

  $: {
    boardState = boardState.map(card => {
      card.flipped =
        matched.includes(card.flipKey) ||
        (flippedKey === card.flipKey && flippedVariant === card.variant);
      return card;
    });
  }

  function initGame() {
    console.log("Initialising flippa game");
    cards = flipKeys.reduce((acc, flipKey) => {
      acc.push({
        flipKey,
        variant: "A"
      });
      acc.push({
        flipKey,
        variant: "B"
      });
      return acc;
    }, []);
    shuffleArray(cards);
    boardState = cards;
  }

  function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
  }

  function handleFlip({ detail }) {
    const { flipKey, variant } = detail;
    if (flippedKey === "") {
      flippedKey = flipKey;
      flippedVariant = variant;
    } else {
      if (flippedKey === flipKey && flippedVariant !== variant) {
        console.log("Match!", flipKey);
        matched.push(flipKey);
        if (matched.length === cards.length) {
          console.log("All matched! Resetting game...");
          matched = [];
        }
      } else {
        console.log("No match");
      }
      flippedKey = "";
      flippedVariant = "";
    }
  }
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Flippa</h1>
  {#each boardState as card}
    <FlipCard
      flipKey={card.flipKey}
      variant={card.variant}
      flipped={card.flipped}
      on:flip={handleFlip} />
  {/each}
</main>
