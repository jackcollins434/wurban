<script lang="ts">
  import NavLink from "./NavLink.svelte";
  import { BookMarked, Menu, Moon } from "lucide-svelte";
</script>

<div class="h-20 bg-primary flex justify-between shadow-lg">
  <div class="py-7 px-2 md:px-7">
    <div class="tracking-wider">
      <NavLink href="/"
        >wUrban <BookMarked class="text-white float-left ml-2" /></NavLink
      >
    </div>
  </div>
  <div class="py-7 px-2 mr-2">
    <NavLink href="#" icon={true}><Moon strokeWidth={1.75} /></NavLink>
    <NavLink href="#" icon={true}><Menu /></NavLink>
  </div>
</div>
