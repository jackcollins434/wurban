<script lang="ts">
  import Box from "$lib/Box.svelte";
  import Character from "$lib/Character.svelte";
  import Navbar from "$lib/Nav/Navbar.svelte";
  import { ArrowRightSquare, Delete } from "lucide-svelte";
  import { onMount } from "svelte";
  import * as Dialog from "$components/ui/dialog";

  type SubmittedLetter = {
    letter: string;
    status: "correct" | "exists" | "wrong";
  };

  const characters = {
    letterRows: ["qwertyuiop", "asdfghjkl", "<zxcvbnm>"],
  };

  const targetWord = "jabol";
  const definition =
    "Jabol means to jerk off your balls. Jabol was derived from the Filipino word “Jakol” which means to jerk off.";

  let wordRange = [...Array(6).keys()];
  let letterRange = [...Array(5).keys()];

  let currentUserWord: Array<string> = [];
  let wordPosition = 0;
  let attemptedLetters: {
    [letter: string]: "correct" | "unused" | "wrong" | "exists";
  } = {};

  let postGameMessageOpen = false;
  let gameStatus = "playing";

  let submittedWords: Array<Array<SubmittedLetter>> = [];

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

    let lettersCorrect = 0;

    currentUserWord.forEach((letter, index) => {
      letter = letter.toLowerCase();
      if (targetWordPieces[index] === letter) {
        submittedWord.push({ letter: letter.toUpperCase(), status: "correct" });
        attemptedLetters[letter] = "correct";
        lettersCorrect++;
      } else if (targetWordPieces.includes(letter)) {
        submittedWord.push({ letter: letter.toUpperCase(), status: "exists" });
        attemptedLetters[letter] = "exists";
      } else {
        submittedWord.push({ letter: letter.toUpperCase(), status: "wrong" });
        attemptedLetters[letter] = "wrong";
      }
    });

    submittedWords = [...submittedWords, submittedWord];

    currentUserWord = [];

    wordPosition++;

    if (lettersCorrect === targetWord.length) {
      gameStatus = "win";
      return (postGameMessageOpen = true);
    }
    if (wordPosition + 1 > wordRange.length) {
      gameStatus = "loss";
      return (postGameMessageOpen = true);
    }
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

<Dialog.Root open={postGameMessageOpen}>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>
        {#if gameStatus === "win"}
          That Was Awesome <span class="text-xl">&#x1F60D;</span>
        {:else}
          Better Luck Next Time <span class="text-2xl">&#x1F614;</span>
        {/if}
      </Dialog.Title>
      <Dialog.Description>
        {#if gameStatus === "win"}
          You got the wUrban in {wordPosition} attempts!
        {:else}
          Maybe try again tomorrow!
        {/if}
        <hr />
        {definition}
      </Dialog.Description>
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>

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
