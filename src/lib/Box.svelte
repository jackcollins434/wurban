<script lang="ts">
  import { cva, type VariantProps } from "class-variance-authority";
  import type { HTMLAttributes } from "svelte/elements";

  const box = cva(
    "h-12 w-12 relative p-2 flex justify-center text-3xl font-bold border shadow-md text-center",
    {
      variants: {
        intent: {
          unused: "border-gray-300",
        },
      },
    }
  );

  interface $$Props
    extends HTMLAttributes<HTMLDivElement>,
      VariantProps<typeof box> {}

  export let intent: $$Props["intent"] = "unused";
</script>

<div {...$$props} class={box({ intent, class: $$props.class })}>
  <slot />
</div>
