<script>
  export let xScale;
  export let height;
  export let width;
//   export let margin;
//   export let groupByContinent;
//   export let yScale;

  $: console.log({ xScale, width, height });

  $: ticks = xScale.ticks(4); // let ticks = [0, 2, 4, 6, 8];

  //import { fade } from "svelte/transition";
</script>

<g class = "axis x">
    {#each ticks as tick}
 <g class="tick" transform="translate({xScale(tick)}, 0)">
    <text x="6" y={height}>{tick}</text>
    <line x1="0" x2="0" y1="0" y2={height} 
    stroke="hsla(215, 15%, 91%, 1)" />
 </g>
 {/each}
 <text class="axis-title" x={width} y={height} text-anchor="end"
 dominant-baseline="hanging" 
 dy = "8">
    Happiness, out of 10 &rarr;
    </text
  >
</g>
