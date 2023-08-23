<script lang="ts">
  import Box from "$lib/Box.svelte";
  import Character from "$lib/Character.svelte";
  import Navbar from "$lib/Nav/Navbar.svelte";
  import { ArrowRightSquare, Delete } from "lucide-svelte";
  import { onMount } from "svelte";

  type SubmittedLetter = {
    letter: string;
    status: "correct" | "exists" | "wrong";
  };

  const characters = {
    letterRows: ["qwertyuiop", "asdfghjkl", "<zxcvbnm>"],
  };

  const targetWord = "tears";

  let wordRange = [...Array(6).keys()];
  let letterRange = [...Array(5).keys()];

  let currentUserWord: Array<string> = [];
  let wordPosition = 0;
  let attemptedLetters: {
    [letter: string]: "correct" | "unused" | "wrong" | "exists";
  } = {};

  let submittedWords: Array<Array<SubmittedLetter>> = [];

  $: console.log(attemptedLetters);

  function handleCharacterEntry(letter: string) {
    if (currentUserWord.length >= targetWord.length) {
      return;
    }

    currentUserWord = [...currentUserWord, letter.toUpperCase()];
  }

  function handleCharacterDeletion() {
    currentUserWord = currentUserWord.slice(0, currentUserWord.length - 1);
  }

  function submitWord() {
    if (currentUserWord.length !== targetWord.length) {
      return;
    }

    const targetWordPieces = [...targetWord.toLowerCase()];
    const submittedWord: Array<SubmittedLetter> = [];

    currentUserWord.forEach((letter, index) => {
      letter = letter.toLowerCase();
      if (targetWordPieces[index] === letter) {
        submittedWord.push({ letter: letter.toUpperCase(), status: "correct" });
        attemptedLetters[letter] = "correct";
      } else if (targetWordPieces.includes(letter)) {
        submittedWord.push({ letter: letter.toUpperCase(), status: "exists" });
        attemptedLetters[letter] = "exists";
      } else {
        submittedWord.push({ letter: letter.toUpperCase(), status: "wrong" });
        attemptedLetters[letter] = "wrong";
      }
    });

    submittedWords = [...submittedWords, submittedWord];
    wordPosition++;

    currentUserWord = [];
  }

  function handleKeydown(e: KeyboardEvent) {
    if (e.key.match(/^[a-zA-Z]$/) && !e.metaKey && !e.ctrlKey) {
      handleCharacterEntry(e.key);
    }

    if (e.key === "Enter") {
      submitWord();
    }

    if (e.key === "Backspace") {
      handleCharacterDeletion();
    }
  }

  onMount(() => {
    document.addEventListener("keydown", handleKeydown);
  });
</script>

<Navbar />

<div class="grid grid-cols-1 gap-1.5 mt-6 place-items-center">
  {#each wordRange as wordRangePosition}
    <div class="flex gap-1.5">
      {#each letterRange as letterPosition}
        {#if submittedWords[wordRangePosition]}
          <Box
            intent={submittedWords[wordRangePosition][letterPosition]["status"]}
            >{submittedWords[wordRangePosition][letterPosition]["letter"]}</Box
          >
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
          <Character on:click={() => handleCharacterDeletion()}
            ><Delete /></Character
          >
        {:else}
          <Character
            intent={attemptedLetters[letter]
              ? attemptedLetters[letter]
              : "unused"}
            on:click={() => handleCharacterEntry(letter)}
            >{letter.toUpperCase()}</Character
          >
        {/if}
      {/each}
    </div>
  {/each}
</div>
