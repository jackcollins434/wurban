<script lang="ts">
  export let character: string;
  export let status: "unused" | "correct" | "incorrect" | "wrongPosition" =
    "unused";

  let characterColor: string;

  switch (status) {
    case "unused":
      characterColor = "border-gray-500 bg-gray-500";
    case "correct":
      characterColor = "border-green-500 bg-green-500";
    case "incorrect":
      characterColor = "border-gray-800 bg-gray-800";
    case "wrongPosition":
      characterColor = "border-yellow-500 bg-yellow-500";
  }
</script>

<div
  class={`shadow-lg flex justify-center border text-white px-2 py-4 sm:w-8 md:w-9 text-lg rounded-sm font-bold relative cursor-pointer ${characterColor}`}
>
  {character.toUpperCase()}
</div>
