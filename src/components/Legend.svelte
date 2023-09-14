<script>
    export let colorScale;
    //export let hoveredContinent;

    const ticks = colorScale.domain();
  </script>
  
  <div class="legend">
    {#each colorScale.domain() as continent}
    <p>
        <span style="background-color: {colorScale(continent)}">

        </span>
        {continent}
    </p>
    {/each}
  </div>

  <style>
    .legend{
        display: flex;
        flex-direction: row;
        justify-content: center;
        flex-wrap: wrap;
        column-gap: 10px;
        row-gap: 5px;
        margin-bottom: 5px;
    }

    span{
        width: 9px;
        height: 9px;
        display: inline-block;
        border-radius: 50%;
        border: 1px solid rgba(0, 0, 0, 0.5);
    }

    p{
        font-size: 14px;
        color: rgba(0, 0, 0, 0.8);
        text-transform: uppercase;
        display: flex;
        align-items: center;
        column-gap: 3px; 
    }
  </style>