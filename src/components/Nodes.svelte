<script>
    import { nodes } from "../data/ex_nodes"
    import { edges } from "../data/ex_edges"
    import { fly, draw } from "svelte/transition";
    import { tweened } from "svelte/motion";
    import * as d3 from 'd3';

    export let height, width, index;
    let svg

    
    console.log(nodes)
    console.log(edges)

    $: x=d3
    .scaleUtc() /* date as input and axis as output */
    .domain(d3.extent(nodes.nodes, (d) => d.x)) /* min and max vals */
    .range([marginLeft, width - marginRight])

    $: y=d3
    .scaleLinear()
    .domain(d3.extent(nodes.nodes, (d) => d.y))
    .range([height - marginBottom, marginTop])
</script>

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
    <!--{} means fill with JS expression-->
      <circle key={n.id} cx={x(n.x)} cy={y(n.y)} r="2.5"/>
    {/each}
    </g>

</div>