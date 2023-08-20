<script lang="ts">
  import { cva, type VariantProps } from "class-variance-authority";
  import type { HTMLAttributes } from "svelte/elements";

  const character = cva(
    "shadow-lg flex justify-center border text-white px-2 py-4 sm:w-8 md:w-9 text-lg rounded-sm font-bold relative cursor-pointer",
    {
      variants: {
        intent: {
          unused: "border-gray-500 bg-gray-500",
          correct: "border-green-600 bg-green-600",
          incorrect: "border-gray-800 bg-gray-800",
          wrongPosition: "border-yellow-500 bg-yellow-500",
        },
      },
    }
  );

  interface $$Props
    extends HTMLAttributes<HTMLDivElement>,
      VariantProps<typeof character> {}

  export let intent: $$Props["intent"] = "unused";
</script>

<div {...$$props} class={character({ intent, class: $$props.class })}>
  <slot />
</div>
