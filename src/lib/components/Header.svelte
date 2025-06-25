<script lang="ts">
  import NavButton from "./NavButton.svelte";

    const mockPages = [
        {
            title: 'Bunch of neon stuff',
            path: '/Neon'
        }
    ]
</script>

<div class="flex justify-between h-[70px] items-center text-[#bdd469] text-[24px] font-bold bg-[#000c0a] p-3" >
    Cool Stuff
    <nav>
        {#each mockPages as p}
            <NavButton path={p.path} title={p.title} />
        {/each}
    </nav>
</div>