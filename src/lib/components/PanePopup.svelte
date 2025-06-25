<script lang="ts" module>
  export type PopupOptions = "accountForRight" | "accountForBottom" | "accountForBoth" | "none";

  export interface PopupProps {
    x: number;
    y: number;
    handleAddLamp: (lamp: NeonLampProps) => void;
    onClose: () => void;
    options: PopupOptions;
    providerWidth: number;
    providerHeight: number;
  }
</script>

<script lang="ts">
  import type { Direction, NeonLampProps } from "./NeonLamp.svelte";
  import { POPUP_HEIGHT, POPUP_WIDTH } from "./NeonProvider.svelte";

  let direction: Direction | null = $state(null);
  const { x, y, handleAddLamp, onClose, options, providerWidth, providerHeight }: PopupProps = $props();
  let length = $state(0);
  let triggerMaxLength = $state(false);

  $inspect(triggerMaxLength);

  const handleDirection = (e: MouseEvent, d: Direction) => {
    e.stopPropagation();
    direction = d;
  };

  const checkForMaxLength = (length: number) => {
    const vertice = direction === "to-bottom" || direction === "to-top" ? "x" : "y";
    if (vertice === "x") {
      if (providerWidth - x > length) return length;
      else return providerWidth - x;
    } else if (vertice === "y") {
      if (providerHeight - y > length) return length;
      else return providerHeight - y;
    }
    return length;
  };

  $effect(() => {
  if (!direction) return; 

  const vertice = direction === "to-bottom" || direction === "to-top" ? "x" : "y";
  console.log('vertice', vertice);
  
  if (vertice === "x" && providerWidth - x <= length) {
    length = providerWidth - x;
    triggerMaxLength = true;
  } else if (vertice === "y" && providerHeight - y <= length) {
    length = providerHeight - y;
    triggerMaxLength = true;
  } else {
    triggerMaxLength = false;
  }
});

  const composeAndAddLamp = () => {
    const newLamp = {
      direction: direction ? direction : "to-bottom",
      length: length,
      depth: 20,
      x: x,
      y: y,
    };
    handleAddLamp(newLamp);
    onClose();
  };

  const optionsLayout: Record<PopupOptions, string> = {
    accountForRight: "translate-x-[-100%]",
    accountForBottom: "translate-y-[-100%]",
    accountForBoth: "translate-x-[-100%] translate-y-[-100%]",
    none: "",
  };

  console.log(x, y);
</script>

<div
  role="button"
  tabindex="0"
  onkeydown={() => {}}
  onclick={(e) => e.stopPropagation()}
  class={`absolute flex flex-col gap-2 justify-center items-start border-2 border-[#f0f04d] z-[2] px-2 ${optionsLayout[options as PopupOptions]} `}
  style={`top: ${y}px; left: ${x}px; width: ${POPUP_WIDTH}px; height: ${POPUP_HEIGHT}px; max-height: ${POPUP_HEIGHT}px`}
>
  <div class="flex gap-1">
    <button
      onclick={(e) => handleDirection(e, "to-right")}
      class="relative flex justify-center items-center border-2 border-[#828282] rounded-md w-[40px] h-[40px] p-2"
      style={`color: ${direction === "to-right" ? "#f0f04d" : "#828282"}`}
      ><span class="absolute rotate-0">&#10148;</span></button
    >
    <button
      onclick={(e) => handleDirection(e, "to-left")}
      class="relative flex justify-center items-center border-2 border-[#828282] rounded-md w-[40px] h-[40px] p-2"
      style={`color: ${direction === "to-left" ? "#f0f04d" : "#828282"}`}
      ><span class="absolute rotate-180">&#10148;</span></button
    >
    <button
      onclick={(e) => handleDirection(e, "to-bottom")}
      class="relative flex justify-center items-center border-2 border-[#828282] rounded-md w-[40px] h-[40px] p-2"
      style={`color: ${direction === "to-bottom" ? "#f0f04d" : "#828282"}`}
      ><span class="absolute rotate-90">&#10148;</span></button
    >
    <button
      onclick={(e) => handleDirection(e, "to-top")}
      class="relative flex justify-center items-center border-2 border-[#828282] rounded-md w-[40px] h-[40px] p-2"
      style={`color: ${direction === "to-top" ? "#f0f04d" : "#828282"}`}
      ><span class="absolute rotate-270">&#10148;</span></button
    >
  </div>
  <button class="flex" onclick={(e: MouseEvent) => e.stopPropagation()}>
    <span>length:</span>
    <input bind:value={length} type="number" class="w-[40px] border-[2] border-[#f0f04d]" />px
    {#if triggerMaxLength}
      <span class="text-[red]">MAX</span>
    {/if}
  </button>
  <button
    onclick={composeAndAddLamp}
    class="absolute bottom-2 right-2 border-[1px] border-[#4df060] text-[#4df060] p-1 rounded-md"
  >
    Add lamp
  </button>
</div>
