<script>
  import Example from "$components/Example.svelte";
  import data from "$data/data.js";
  console.log(data);

  import {
    forceSimulation,
    forceX,
    forceY,
    forceCollide
  } from "d3-force";
  //import simulation from "d3-force/src/simulation";

  const RADIUS = 5;
  $: simulation = forceSimulation(data)
  .force("x", 
  forceX()
  .x(d => xScale(d.happiness)).strength(0.8)
  )
  .force('y',
  forceY()
  .y(d => yScale(d.continent)).strength(0.2)
  )
   .force('collide',
   forceCollide().radius(RADIUS)
   )

   $: console.log(simulation.nodes());

   let width = 400,
   height = 400;

   const margin ={
    top: 0,
    right: 0,
    bottom: 20,
    left: 0
   };

   $: innerWidth = width-margin.left-margin.right;
   let innerHeight = height-margin.top-margin.bottom;

   $: console.log({innerWidth});

   import {scaleLinear, scaleBand} from "d3-scale";
   $: xScale = scaleLinear()
   .domain([1, 9]) //Input
   .range([0, innerWidth]) //Output

   import { mean, rollups} from "d3-array";
   const continent = rollups(
    data,
    v => mean(v, d => d.happiness),
    d => d.continent
   ).sort((a,b) => a[1] - b[1])
   .map(d => d[0]);

   let yScale = scaleBand()
   .domain(continent) // return list of continent
   .range([innerHeight, 0])
   .paddingOuter(0.5);

console.log(yScale('Africa'));
$: nodes = simulation.nodes();
</script>

<div class= 'class-container'
 bind:clientWidth={width}>
<svg {width} {height}>
<g class= 'inner-chart' transform="translate({margin.left}, {margin.top})">
  {#each nodes as node}
  <circle
  cx={node.x}
  cy={node.y}
  r={RADIUS}
  fill='steelblue'/>
  {/each}
</g>
</svg>
</div>

<!-- <main>
  <h1>Let's make a chart 😎</h1>
  <h2>
    Get started by deleting all of the contents in <pre>App.svelte</pre>
    🗑
  </h2>
  <Example />
  <footer>
    For help, <a
      href="https://twitter.com/CL_Rothschild"
      target="_blank"
      rel="noopener noreferrer">DM Connor on Twitter ✉️</a
    >
  </footer>
</main>

<style>
  main {
    text-align: center;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: #f0f0f0;
  }

  h1 {
    font-size: 3rem;
    margin-bottom: 1rem;
    font-weight: 700;
  }

  h2 {
    font-size: 1.5rem;
    color: #333;
    margin-bottom: 2rem;
    line-height: 1.5;
  }

  pre {
    padding: 1px 6px;
    display: inline;
    margin: 0;
    background: #ffb7a0;
    border-radius: 3px;
  }

  a {
    color: #ff3e00;
    text-decoration: inherit;
  }

  footer {
    font-size: 1rem;
    color: #333;
  }
</style> -->
