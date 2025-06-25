<script lang="ts">
  import Header from "$lib/components/Header.svelte";
  import "../app.css";
</script>


<main class="flex flex-col h-screen w-screen overflow-hidden m-0 p-0">
    <Header />
    <div class="flex justify-center items-center grow">
        <slot />
    </div>
</main>

<style lang="postcss">
    @reference "tailwindcss";
    :global(html) {
      background-color: theme(--color-gray-100);
    }
  </style>