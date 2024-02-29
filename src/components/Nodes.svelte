<script>
    import { nodes } from "../data/ex_nodes"
    import { edges } from "../data/ex_edges"
    import { fly, draw } from "svelte/transition";
    import { tweened } from "svelte/motion";
    import * as d3 from 'd3';

    export let height, width, index;
    const marginLeft = 0;
    const marginRight = 0;
    const marginTop = 0;
    const marginBottom = 0;
    
    let svg
    
    let x,y;
    
    console.log(nodes)
    console.log(edges)

    $: x=d3
    .scaleUtc() /* date as input and axis as output */
    .domain(d3.extent(nodes.nodes, (d) => d.x)) /* min and max vals */
    .range([200, 250])

    $: y=d3
    .scaleLinear()
    .domain(d3.extent(nodes.nodes, (d) => d.y))
    .range([200, 100])

</script>


<svg class = "nodes">
    {#if index >= 1}
        {#each nodes.nodes as node}
        <circle
            cx={x(node.x)}
            cy={y(node.y)}
            r="5"
            fill="blue"
        />
        {/each}
    {/if}
</svg>

<div class="weighted-graph">
    <svg
    bind:this={svg}
    {width}
    {height}
    viewBox="0 0 {width} {height}"
    style="max-width: 100%; height: auto;"
    >
  <g stroke="#000" stroke-opacity="0.2">
    {#each nodes.nodes as n}
        console.log
        <circle key={n.id} cx={x(n.x)} cy={y(n.y)} r="2.5"/>
    {/each}
    </g>

</div>