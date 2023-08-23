<script lang="ts">
  import Box from "$lib/Box.svelte";
  import Character from "$lib/Character.svelte";
  import Navbar from "$lib/Nav/Navbar.svelte";
  import { ArrowRightSquare, Delete } from "lucide-svelte";

  const characters = {
    letterRows: ["qwertyuiop", "asdfghjkl", "<zxcvbnm>"],
  };

  const word = "tears";

  let wordRange = [...Array(6).keys()];
  let letterRange = [...Array(5).keys()];

  let currentUserWord: Array<string> = [];
  let wordPosition = 0;

  let submittedWords: Array<Array<string>> = [];

  function handleCharacterEntry(letter: string) {
    if (currentUserWord.length >= word.length) {
      return;
    }

    currentUserWord = [...currentUserWord, letter.toUpperCase()];
  }

  function submitWord() {
    submittedWords = [...submittedWords, currentUserWord];
    wordPosition++;

    currentUserWord = [];
  }
</script>

<Navbar />

<div class="grid grid-cols-1 gap-1.5 mt-6 place-items-center">
  {#each wordRange as wordRangePosition}
    <div class="flex gap-1.5">
      {#each letterRange as letterPosition}
        {#if submittedWords[wordRangePosition]}
          <Box>{submittedWords[wordRangePosition][letterPosition]}</Box>
        {:else if wordRangePosition === wordPosition && currentUserWord[letterPosition] !== undefined}
          <Box>{currentUserWord[letterPosition]}</Box>
        {:else}
          <Box />
        {/if}
      {/each}
    </div>
  {/each}
</div>

<div class="grid grid-cols-1 gap-3 mt-4 place-items-center">
  {#each characters["letterRows"] as letterRow}
    <div class="flex gap-1.5">
      {#each letterRow as letter}
        {#if letter === ">"}
          <Character on:click={submitWord}><ArrowRightSquare /></Character>
        {:else if letter === "<"}
          <Character><Delete /></Character>
        {:else}
          <Character on:click={() => handleCharacterEntry(letter)}
            >{letter.toUpperCase()}</Character
          >
        {/if}
      {/each}
    </div>
  {/each}
</div>
