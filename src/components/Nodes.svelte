<script>
    import { nodes } from "../data/ex_nodes"
    import { edges } from "../data/ex_edges"
    import { fly, draw } from "svelte/transition";
    import { cubicOut, cubicInOut } from "svelte/easing";
    import { tweened } from "svelte/motion";
    import * as d3 from 'd3';

    export let height, width, index;
    const marginLeft = 300;
    const marginRight = 300;
    const marginTop = 300;
    const marginBottom = 300;
    
    let svg;

    $: x=d3
    .scaleLinear() /* date as input and axis as output */
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
    {#if index > 0}
    <g stroke="#000" stroke-opacity="0.2">
        {#each nodes.nodes as n}
            <circle key={n.id} cx={x(n.x)} cy={y(n.y)} r="10"
            transition:draw={{ duration: 5000, easing: cubicInOut }}/>
        {/each}
    </g>
    

    {/if}

</div>