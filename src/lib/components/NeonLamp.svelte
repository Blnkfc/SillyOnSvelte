<script module>
  export type Direction = "to-top" | "to-bottom" | "to-left" | "to-right";
  export interface NeonLampProps {
    direction: Direction;
    length: number;
    depth: number;
    x?: number;
    y?: number;
    angleOffset?: number;
  }
</script>

<script lang="ts">

  import { cn } from "../../utils/mergeTailwind";

  let { direction, length, depth, x, y, angleOffset}: NeonLampProps = $props();
  console.log('lamp', direction, length, depth, x, y, angleOffset);
  
  $effect(() => {
    console.log("x", x);
    console.log("y", y);
  });

  const layout: Record<Direction, Record<string, string>> = {
    "to-top": {
      width: `${length}`,
      height: `${depth}`,
      position: "translate-y-[-15%]",
      misc: "border-t-0 justify-center",
      glow: "drop-shadow-[0_-3px_3px_#fff,0_-3px_5px_#00ffa2]",
    },
    "to-bottom": {
      width: `${length}`,
      height: `${depth}`,
      position: "translate-y-[15%]",
      misc: "border-b-0 justify-center",
      glow: "drop-shadow-[0_3px_3px_#fff,0_3px_5px_#00ffa2]",
    },
    "to-left": {
      width: `${depth}`,
      height: `${length}`,
      position: "translate-x-[-15%]",
      misc: "border-l-0 items-center",
      glow: "drop-shadow-[-3px_0_3px_#fff,-3px_0_5px_#00ffa2]",
    },
    "to-right": {
      width: `${depth}`,
      height: `${length}`,
      position: "translate-x-[20%]",
      misc: "border-r-0 items-center",
      glow: "drop-shadow-[3px_0_3px_#fff,3px_0_5px_#00ffa2]",
    },
  };

</script>

<div
  class={cn(
    `flex absolute bg-[#453d36] border-3 border-[#785a42]  rounded-md inset-shadow-[0px_0px_3px_#fff]`,
    layout[direction]["misc"]
  )}
  style={`
  width: ${layout[direction]["width"]}px; 
  height: ${layout[direction]["height"]}px; 
  left: ${x}px; top: ${y}px;
  transform-origin: left;
  transform: rotate(${angleOffset}deg)`}
>
  <div
    class={cn(
      `absolute  w-[calc(100%-2px)] h-[calc(100%-4px)] inset-shadow-[0_0_3px_#2effc7] bg-[#d6fff4] rounded-sm`,
      layout[direction]["position"],
      layout[direction]["glow"]
    )}
  ></div>
</div>
