<script>
    // get node and edge data 
    import { nodes } from "../data/ex_nodes"
    import { edges } from "../data/ex_edges"
    import { sol_nodes } from "../data/sol_nodes"
    import { sol_edges } from "../data/sol_edges"
    import { c_outside } from "../data/sets"

    import { fly, draw , fade} from "svelte/transition";
    import { cubicOut, cubicInOut } from "svelte/easing";
    import { tweened } from "svelte/motion";
    import * as d3 from 'd3';
    export let height, width, index;
    const marginLeft = 100;
    const marginRight = 100;
    const marginTop = 100;
    const marginBottom = 100;
    const C = c_outside.C
    const Outside = c_outside.Outside

    let currNode;
    let svg;
    let tempx;
    let tempy;
    const blue = "#499e97"
    
    let blueLines = [];
    $: x=d3
    .scaleLinear() /* date as input and axis as output */
    .domain(d3.extent(nodes.nodes, (d) => d.x)) /* min and max vals */
    .range([marginLeft * 1.25, width / 1.7])
    $: y=d3
    .scaleLinear()
    .domain(d3.extent(nodes.nodes, (d) => d.y))
    .range([height * 0.75, marginTop * 2])
    const tweenOptions = {
    delay: 0,
    duration: 1000,
    easing: cubicOut,
  };
    let rectX = tweened(130, { duration: 1000, easing: cubicOut });
    let rectY = tweened(380, { duration: 1000, easing: cubicOut });
  function setRect() {
    rectX = tweened(130, { duration: 1000, easing: cubicOut });
    rectY = tweened(380, { duration: 1000, easing: cubicOut });
  }
  function resetUserInteraction() {

    d3.select(svg).selectAll("*").remove();

    blueLines = [];
  }
    function setCurrNode(node){
        currNode = node;
        console.log('set currNode')
    }
    // Function to handle clicking on nodes
    function handleClick(event, node) {
        console.log("clicked on node")
        // Update position of rectangle to follow edge to clicked node
        updateRectanglePosition(node);
    }

    // Function to update position of rectangle based on selected node
    function updateRectanglePosition(selectedNode) {
        //console.log(selectedNode)
        const edges_lst = edges.edges.filter(e => (e.source === selectedNode.id || e.target === selectedNode.id));
        if (!edges_lst) return; // No edge found

        const targetNode = selectedNode; //nodes.nodes.find(n => n.id === edge.target);
        const exists = edges_lst.some(edge => edge.source === currNode.id); 
        if (!exists) {
            console.log("not connected")
            return;
        }
        const sourceNode = currNode;
        const connected_edge = edges.edges.find(e => (e.source == sourceNode.ig && e.target == selectedNode.id))
        
        // animate movement of car
        rectX.set(x(sourceNode.x) + (x(targetNode.x) - x(sourceNode.x)) + 25);
        rectY.set(y(sourceNode.y) + (y(targetNode.y) - y(sourceNode.y)) - 7);
        
        // draw new lines
        tempx = x(sourceNode.x) + (x(targetNode.x) - x(sourceNode.x)) + 20;
        tempy = y(sourceNode.y) + (y(targetNode.y) - y(sourceNode.y)) - 7;
        // Calculate the new points for the polyline


        const points = `${x(currNode.x)},${y(currNode.y)} ${tempx - 20 },${tempy +7} ${x(selectedNode.x)},${y(selectedNode.y)}`;

        // Update the polyline's points

        // update edge and node colors
        // Create a new polyline element for the blue line
        const newLine = document.createElementNS("http://www.w3.org/2000/svg", "polyline");
        newLine.setAttribute("points", points);
        newLine.setAttribute("stroke", blue);
        newLine.setAttribute("stroke-width", "7");
        newLine.setAttribute("fill", "none");
        newLine.style.opacity = 0;
        // Add the new line to the SVG
        svg.appendChild(newLine);
        // transition
        setTimeout(() => {
            newLine.style.opacity = 1;
        }, 500);
        
        blueLines.push(newLine);

        // add new circle
        const newCircle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
        newCircle.setAttribute("fill" , blue);
        newCircle.setAttribute("r", '20');
        newCircle.setAttribute('cx' , `${x(targetNode.x)}`);
        newCircle.setAttribute('cy' , `${y(targetNode.y)}`);
        newCircle.style.opacity = 0;
        svg.appendChild(newCircle);
        blueLines.push(newCircle)
        setTimeout(() => {
            newCircle.style.opacity = 1;
        }, 500);
        // cx={x(n.x)} 
        // cy={y(n.y)} 

        // create new circle
        setCurrNode(targetNode); // change what currNode is 
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
        {#if index === 0}
            {resetUserInteraction()}
        {/if}

        {#if index === 1}
            {setRect()}
            {resetUserInteraction()}
            {#each edges.edges as e}
                    <polyline
                        points={ // String of coordintates x0,y0 x1,y1
                        String(x(nodes.nodes[e.source].x)) + "," + String(y(nodes.nodes[e.source].y))
                                + " " + String(x(nodes.nodes[e.target].x)) + "," + String(y(nodes.nodes[e.target].y))
                                }
                        stroke={e.color}
                        stroke-width="3"
                        in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                    />
                {/each}


                {#each nodes.nodes as n}
                    {#if n.id == 0}
                            {setCurrNode(n)}
                        {/if}
                    <circle 
                        key={n.id} 
                        cx={x(n.x)} 
                        cy={y(n.y)} 
                        r="20" 
                        fill={n.color}
                        in:fade|global={{intro: true , duration: 500, delay: 50, easing: cubicInOut }}
                        style="pointer-events: auto;"
                        on:click={() => handleClick(event, n)}
                    />
                {/each}
                <rect x={$rectX} y={$rectY} width="50" height="20" fill="green" 
                in:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }} />
        {/if}


        {#if index > 1}
            {setRect()}
            {resetUserInteraction()}
            {#each edges.edges as e}
                    <polyline
                        points={ // String of coordintates x0,y0 x1,y1
                        String(x(nodes.nodes[e.source].x)) + "," + String(y(nodes.nodes[e.source].y))
                                + " " + String(x(nodes.nodes[e.target].x)) + "," + String(y(nodes.nodes[e.target].y))
                                }
                        stroke={e.color}
                        stroke-width="3"
                    />
                {/each}
                {#each nodes.nodes as n}
                    <circle 
                        key={n.id} 
                        cx={x(n.x)} 
                        cy={y(n.y)} 
                        r="20" 
                        fill={n.color}
                    />
                {/each}
        {/if}
        {#if index === 2}
            console.log('here')
                {#each sol_edges.edges as e}
                    <polyline
                        points={ // String of coordintates x0,y0 x1,y1
                        String(x(sol_nodes.nodes[e.source].x)) + "," + String(y(sol_nodes.nodes[e.source].y))
                                + " " + String(x(sol_nodes.nodes[e.target].x)) + "," + String(y(sol_nodes.nodes[e.target].y))
                                }
                        stroke={blue}
                        stroke-width="5"
                        in:draw|global={{intro: true , duration: 1000, delay: (100 + (e.source * 200)), easing: cubicInOut }}
                        out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                        />
                    {/each}
        
                {#each sol_nodes.nodes as n}
                    <circle 
                        key={n.id} 
                        cx={x(n.x)} 
                        cy={y(n.y)} 
                        r="20" 
                        fill={n.color}
                        in:fade|global={{intro: true , duration: 1000, delay: 10 + (n.id * 200), easing: cubicInOut }}
                        out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    />
                {/each}   
            {/if}
            {#if index > 3}
                {console.log("table should be here")}
                <foreignObject x="700" y="200" width="300" height="500">
                    <div>
                        <table style="width:100%">
                            <thead>
                                <tr>
                                    <th>C</th>
                                    <th>Outside</th>
                                </tr>
                            </thead>
                            <tbody>
                                {#each Array(5) as _, i (i)}
                                {#if index - 3 > i}
                                    <tr>
                                        <td>{C[i]}</td>
                                        <td>{Outside[i]}</td>
                                    </tr>
                                    {/if}
                                {/each}
                            </tbody>
                        </table>
                    </div>
                </foreignObject>

                
            {/if}
        
            
        
        
    </svg>

  
</div>

<style>
    table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  text-align: left;
}
    th, td {
  border-color: #96D4D4;
  width:50%;
}
tr:nth-child(even) {
  background-color: #D6EEEE;
}
</style>
