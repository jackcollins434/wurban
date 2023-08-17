<script lang="ts">
  export let href: string;
  export let button = false;
  export let icon = false;
</script>

{#if button}
  <button class="mx-4 text-white text-md font-semibold relative" on:click
    ><slot /></button
  >
{:else if icon}
  <button class="mx-2 text-white text-2xl font-semibold"><slot /></button>
{:else}
  <a {href} class="mx-4 text-white text-md font-semibold relative"><slot /></a>
{/if}

<style>
  a::before,
  button::before {
    content: "";
    position: absolute;
    left: 0;
    bottom: -2px;
    width: 0;
    height: 2px; /* Adjust the border height as needed */
    background-color: white; /* Change this to your desired border color */
    transition: width 0.25s ease; /* Adjust the transition timing as needed */
    width: 0;
  }
  a:hover::before,
  button:hover::before {
    width: 100%;
  }
</style>
