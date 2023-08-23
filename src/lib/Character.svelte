<script lang="ts">
  import { cva, type VariantProps } from "class-variance-authority";
  import type { HTMLButtonAttributes } from "svelte/elements";

  const character = cva(
    "shadow-lg flex justify-center border px-2 py-4 w-8 md:w-9 text-lg rounded-sm font-bold relative cursor-pointer touch-manipulation",
    {
      variants: {
        intent: {
          unused: "border-gray-300 bg-gray-300 text-black",
          correct: "border-green-600 bg-green-600 text-white",
          wrong: "border-gray-800 bg-gray-800 text-white",
          exists: "border-yellow-500 bg-yellow-500 text-white",
        },
      },
    }
  );

  interface $$Props
    extends HTMLButtonAttributes,
      VariantProps<typeof character> {}

  export let intent: $$Props["intent"] = "unused";
</script>

<button
  {...$$props}
  class={character({ intent, class: $$props.class })}
  on:click
>
  <slot />
</button>
