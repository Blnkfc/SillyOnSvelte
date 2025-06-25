<script lang="ts">
  let { title, path } = $props();
  let hovered = $state(false);
</script>

<a
  onpointerover={() => (hovered = true)}
  onpointerleave={() => (hovered = false)}
  class="relative flex justify-center items-center text-[16px] text-[#f5d1ff] p-3 mx-2 h-[40px] "
  href={path}
>
  <p class="block w-full h-full drop-shadow-[1px_0px_1px_#d642ff,-1px_0px_1px_#d642ff,0px_1px_1px_#d642ff,0px_-1px_1px_#d642ff]">
    {title}
  </p>
  {#if hovered}
    <div class="w-full h-[1px] absolute bottom-0 bg-[#fff] blur-[0.2px] drop-shadow-[0_-1px_1px_#fff,0_-1px_2px_#d642ff,0_-1px_2px_#d642ff]"></div>
  {/if}
</a>
