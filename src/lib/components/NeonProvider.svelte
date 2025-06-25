<script lang="ts" module>
  export interface NeonProviderProps {
    width: number;
    height: number;
  }

  export interface PopupCoordinates {
    x: number;
    y: number;
    options?: PopupOptions;
  }

  export const POPUP_WIDTH = 300;
  export const POPUP_HEIGHT = 120;
</script>

<script lang="ts">
  import { onMount } from "svelte";
  import NeonLamp, { type NeonLampProps } from "./NeonLamp.svelte";
  import PanePopup, { type PopupOptions } from "./PanePopup.svelte";
  const initialLamps: NeonLampProps[] = [
    // {
    //   direction: 'to-right',
    //   length: 150,
    //   depth: 20,
    // },
    // {
    //   direction: 'to-bottom',
    //   length: 250,
    //   depth: 20,
    // }
  ];

  let elem: HTMLElement | null = $state(null);
  let popupCords: PopupCoordinates | null = $state(null);
  let mockLamps: NeonLampProps[] = $state(initialLamps);
  let startPoint = $state();
  let firstPoint = $state<{ x: number; y: number } | null>(null);
  let secondPoint = $state<{ x: number; y: number } | null>(null);
  let projectedLamp = $state<NeonLampProps | null>(null)
    let projectedLength = $state<number | null>(null)
  let { width, height } = $props();
  // const handlePaneClick = (e: MouseEvent | null) => {
  //   if(!e) return

  //   const x = e.offsetX
  //   const y = e.offsetY
  //   let options = 'none'
  //   console.log('cords', x ,y);

  //   if(((width - x) < POPUP_WIDTH) && ((height - y) < POPUP_HEIGHT))
  //     options = 'accountForBoth' as PopupOptions
  //   else if((width - x) < POPUP_WIDTH)
  //     options = 'accountForRight' as PopupOptions
  //   else if( (height - y) < POPUP_HEIGHT)
  //     options = 'accountForBottom' as PopupOptions

  //   popupCords = {
  //     x: x,
  //     y: y,
  //     options: options as PopupOptions
  //   }
  //   console.log(e);
  // };
  const handlePaneClick = (e: MouseEvent | null) => {
    if (!e) return;
    if (!firstPoint) {
      firstPoint = { x: e.offsetX, y: e.offsetY };
    } else if (!secondPoint) {
      secondPoint = { x: e.offsetX, y: e.offsetY };

      const offset = Math.atan((secondPoint.y - firstPoint.y)/(secondPoint.x-firstPoint.x)) * (180/Math.PI)
      const normilizedOffset = firstPoint.x > secondPoint.x ? offset+180:offset+0 
      console.log('offset', offset);
      const length = Math.sqrt(Math.pow(secondPoint.x-firstPoint.x, 2) + Math.pow(secondPoint.y-firstPoint.y, 2))
      const lamp = {
        direction: "to-bottom",
        length: length,
        depth: 20,
        x: firstPoint.x,
        y: firstPoint.y,
        angleOffset: normilizedOffset
      };
      handleAddLamp(lamp as NeonLampProps)
      firstPoint = null
      secondPoint = null
    }
    console.log(e);
  };

  const handleAddLamp = (lamp: NeonLampProps) => {
    console.log('lamp in adder', lamp);
    
    const newX = lamp.x;
    const newY = lamp.y;
    const newLamp = {
      direction: lamp.direction,
      length: lamp.length,
      depth: lamp.depth,
      x: newX,
      y: newY,
      angleOffset: lamp.angleOffset
    };
    mockLamps = [...mockLamps, newLamp as NeonLampProps];
  };

  const handleRightClick = (e: MouseEvent | null) => {};

  const handleClosePopup = () => {
    popupCords = null;
  };
  $inspect(projectedLength)
  const handleMouseMove = (e: MouseEvent | null) => {
    if(firstPoint && !secondPoint && e){
      const projectedEndCords = {x: e.offsetX, y: e.offsetY}
      const offset = Math.atan((projectedEndCords.y - firstPoint.y)/(projectedEndCords.x-firstPoint.x)) * (180/Math.PI)
      const normalizedOffset = firstPoint.x > projectedEndCords.x ? offset+180:offset+0 
      projectedLength = Math.sqrt(Math.pow(projectedEndCords.x-firstPoint.x, 2) + Math.pow(projectedEndCords.y-firstPoint.y, 2))
      projectedLamp = {
        direction: "to-bottom",
        length: projectedLength,
        depth: 20,
        x: firstPoint.x,
        y: firstPoint.y,
        angleOffset: normalizedOffset
      };

    }
    
  }
</script>

<div
  id={"neon-provider"}
  role="button"
  tabindex="0"
  onclick={(e) => handlePaneClick(e)}
  onkeydown={(e) => (e.key === "Enter" || e.key === " ") && handlePaneClick(null)}
  onmousemove={(e) => handleMouseMove(e)}
  onfocus={(e) => {}}
  class="relative border-2 rounded-lg border-[#43ba8c] p-2"
  style={`width: ${width}px; height: ${height}px`}
>
  {#if popupCords}
    <PanePopup
      x={popupCords.x}
      y={popupCords.y}
      {handleAddLamp}
      onClose={handleClosePopup}
      options={popupCords.options as PopupOptions}
      providerWidth={width}
      providerHeight={height}
    />
  {/if}
  {#each mockLamps as lamp}
    <NeonLamp direction={lamp.direction} length={lamp.length} depth={lamp.depth} x={lamp.x} y={lamp.y} angleOffset={lamp.angleOffset} />
  {/each}
  <!-- {#if projectedLamp && projectedLength}
        <NeonLamp direction={projectedLamp.direction} length={projectedLength} depth={20} x={projectedLamp.x} y={projectedLamp.y} angleOffset={projectedLamp.angleOffset} />
  {/if} -->
</div>
