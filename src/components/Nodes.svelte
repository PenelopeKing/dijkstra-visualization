<script>

    // get node data with nodes.nodes
    import { nodes } from "../data/ex_nodes"
    // get edge data with edges.edges
    import { edges } from "../data/ex_edges"
    import { fly, draw , fade} from "svelte/transition";
    import { cubicOut, cubicInOut } from "svelte/easing";
    import { tweened } from "svelte/motion";

    
    import * as d3 from 'd3';

    export let height, width, index;
    const marginLeft = 100;
    const marginRight = 450;
    const marginTop = 200;
    const marginBottom = 200;

    let svg;

    $: x=d3
    .scaleLinear() /* date as input and axis as output */
    .domain(d3.extent(nodes.nodes, (d) => d.x)) /* min and max vals */
    .range([marginLeft, width - marginRight])

    $: y=d3
    .scaleLinear()
    .domain(d3.extent(nodes.nodes, (d) => d.y))
    .range([height - marginBottom, marginTop])

    const tweenOptions = {
    delay: 0,
    duration: 1000,
    easing: cubicOut,
  };
    let strokeWidth = tweened(1, { duration: 5000, easing: cubicOut });
    
    // Function to handle clicking on nodes
    function handleClick(event, node) {
        console.log("clicked")
        // Update position of rectangle to follow edge to clicked node
        updateRectanglePosition(node);
    }
    // Function to update position of rectangle based on selected node
    function updateRectanglePosition(selectedNode) {
        const edge = edges.edges.find(e => (e.source === selectedNode.id || e.target === selectedNode.id));
        if (!edge) return; // No edge found

        const sourceNode = nodes.nodes.find(n => n.id === edge.source);
        const targetNode = nodes.nodes.find(n => n.id === edge.target);

        const rect = svg.querySelector('rect');
        const rectX = x(sourceNode.x) + (x(targetNode.x) - x(sourceNode.x)) / 2 - 10;
        const rectY = y(sourceNode.y) + (y(targetNode.y) - y(sourceNode.y)) / 2 - 10;
        rect.setAttribute('x', rectX);
        rect.setAttribute('y', rectY);
    }

    
</script>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="weighted-graph">
    <svg class="graph"
    bind:this={svg}
    {width}
    {height}
    viewBox="0 0 {width} {height}"
    style="max-width: 100%; height: auto; pointer-events: auto;"
    >
    {#if index > 0}
        {#each edges.edges as e}
                <polyline
                    points={ // String of coordintates x0,y0 x1,y1
                    String(x(nodes.nodes[e.source].x)) + "," + String(y(nodes.nodes[e.source].y))
                            + " " + String(x(nodes.nodes[e.target].x)) + "," + String(y(nodes.nodes[e.target].y))
                            }
                    stroke="#5e5e5e"
                    stroke-width="3"
                    in:draw|global={{intro: true , duration: 1000, delay: 200, easing: cubicInOut }}
                />
            {/each}
            {#each nodes.nodes as n}
                <circle 
                    key={n.id} 
                    cx={x(n.x)} 
                    cy={y(n.y)} 
                    r="20" 
                    fill={n.color}
                    in:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }}
                    style="pointer-events: auto;"
                    on:click={() => handleClick(event, n)}
                />
            {/each}
    
        {/if}
    <rect x="100" y="350" width="50" height="20" fill="orange" />
    </svg>
</div>
