<script>
  import Example from "$components/Example.svelte";
  import data from "$data/data.js";
  //console.log(data);

  import { forceSimulation, forceX, forceY, forceCollide } from "d3-force";
  //import simulation from "d3-force/src/simulation";

  //const RADIUS = 5;
  const simulation = forceSimulation(data);

  $: {
    simulation
      .force(
        "x",
        forceX()
          .x((d) => xScale(d.happiness))
          .strength(0.8)
      )
      .force(
        "y",
        forceY()
          .y((d) => yScale(d.continent))
          .strength(0.2)
      )
      .force("collide", forceCollide().radius(d => radiusScale(d.happiness)))
      .alpha(0.3)
      .alphaDecay(0.0005)
      .restart();
  }

  // $: nodes = simulation.nodes();
  let nodes = [];
  simulation.on("tick", () => {
    nodes = simulation.nodes();
  });

  $: console.log(simulation.nodes());

  let width = 400,
    height = 400;

  const margin = {
    top: 0,
    right: 0,
    bottom: 25,
    left: 0,
  };

  $: innerWidth = width - margin.left - margin.right;
  let innerHeight = height - margin.top - margin.bottom;

  $: console.log({ innerWidth });

  import { scaleLinear, scaleBand, scaleOrdinal, scaleSqrt } from "d3-scale";
  $: xScale = scaleLinear()
    .domain([1, 9]) //Input
    .range([0, innerWidth]); //Output

  //Generate the average for each continent, so thart we can sort according to that
  import { mean, range, rollups } from "d3-array";
  const continents = rollups(
    data,
    (v) => mean(v, (d) => d.happiness),
    (d) => d.continent
  ) // Group Data by continent and return the group-wide-mean
    .sort((a, b) => a[1] - b[1]) //Sort according to value
    .map((d) => d[0]); //Grab the continent name

  console.log({ continents });

  //Add a color to each circle according to its continent
  //import { scaleOrdinal } from "d3-scale"; //imported above
  const colorRange = ["#dda0dd", "#fe7f2d", "#fcca46", "#619b8a", "#eae2b7"];
  const colorScale = scaleOrdinal().domain(continents).range(colorRange);

  let yScale = scaleBand()
    .domain(continents) // return list of continent
    .range([innerHeight, 0])
    .paddingOuter(0.5);

  //Size of circles
  $: radiusScale = scaleSqrt()
    .domain([1, 9]) //Input
    .range(width < 568 ? [2, 6] : [3, 8]); //Output

  //console.log(yScale("Africa"));

  import AxisX from "$components/AxisX.svelte";
  import AxisY from "$components/AxisY.svelte";

  import Legend from "$components/Legend.svelte";
</script>

<h1>The Happiest Countries Clients Of Weroute System</h1>
<Legend {colorScale} />
<div class="class-container" bind:clientWidth={width}>
  <svg {width} {height}>
    <g class="inner-chart" transform="translate({margin.left}, {margin.top})">
      <AxisX xScale={xScale} height={innerHeight} width = {innerWidth} />
      <AxisY {yScale} />
      {#each nodes as node}
        <circle
          cx={node.x}
          cy={node.y}
          r={radiusScale(node.happiness)}
          fill={colorScale(node.continent)}
          stroke="black"
        />
      {/each}   
    </g>
  </svg>
</div>

<style >
  :global(.tick text) {
    font-size: 12px;
    font-weight: 400;
    fill: hsla(212, 10%, 53%, 1);
    user-select: none;
  }

  :global(.axis-title){
      font-size: 14px;
    }

    h1 {
    font-size: 22px;
    margin-bottom: 6px;
    font-weight: 600;
    text-align: center;
  }
</style>

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
    flex-direction
    : column;
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
