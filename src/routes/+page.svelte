<script lang="ts">
  import Character from "$lib/Character.svelte";
  import Navbar from "$lib/Nav/Navbar.svelte";

  const characters = {
    letterRows: ["qwertyuiop", "asdfghjkl", "zxcvbnm"],
  };
</script>

<Navbar />

<div class="grid grid-cols-1 mt-40 place-items-center gap-3">
  {#each characters["letterRows"] as letterRow}
    <div class="flex gap-1.5 row-">
      {#each letterRow as letter}
        <Character character={letter} />
      {/each}
    </div>
  {/each}
</div>
