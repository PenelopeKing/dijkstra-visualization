<script>
    // get node and edge data 
    import { nodes } from "../data/ex_nodes"
    import { edges } from "../data/ex_edges"
    import { sol_nodes } from "../data/sol_nodes"
    import { sol_edges } from "../data/sol_edges"
    import { c_outside } from "../data/sets"
    import { uest_vals } from "../data/uest"

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
    const set_index = Object.keys(c_outside.C)

    let currNode;
    let svg;
    let tempx;
    let tempy;
    const blue = "#3d87ff";
    const red = '#FF0000';
    const pink = '#FFB6C1'; 
    const green = '#008f180';
    
    let blueLines = [];
    $: x=d3
    .scaleLinear() 
    .domain(d3.extent(nodes.nodes, (d) => d.x)) /* min and max vals */
    .range([marginLeft * 4, width / 1.6])
    $: y=d3
    .scaleLinear()
    .domain(d3.extent(nodes.nodes, (d) => d.y))
    .range([height * 0.75, marginTop * 2])
    const tweenOptions = {
    delay: 0,
    duration: 1000,
    easing: cubicOut,
  };
    let rectX = tweened(400, { duration: 1000, easing: cubicOut });
    let rectY = tweened(385, { duration: 1000, easing: cubicOut });
  function setRect() {
    rectX = tweened(400, { duration: 1000, easing: cubicOut });
    rectY = tweened(385, { duration: 1000, easing: cubicOut });
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
            <img src = "../imgs/car.png" alt = "car" x=20 y=100 width="50" height="20"  />

        {/if}

        {#if index === 1}
            {setRect()}
            {resetUserInteraction()}
            {#each edges.edges as e}
                <defs>
                    <!-- A marker to be used as an arrowhead. -->
                    <!-- refX determines how far down the line the marker is. -->
                    <marker
                    id="arrow"
                    viewBox="0 0 10 10"
                    refX="23"
                    refY="5"
                    markerWidth="5"
                    markerHeight="5"
                    orient="auto-start-reverse">
                    <path d="M 0 0 L 10 5 L 0 10 z" />
                    </marker>
                </defs>
                <polyline
                    points={ // String of coordinates x0,y0 x1,y1
                    String(x(nodes.nodes[e.source].x)) + "," + String(y(nodes.nodes[e.source].y))
                            + " " + String(x(nodes.nodes[e.target].x)) + "," + String(y(nodes.nodes[e.target].y))
                            }
                    stroke={e.color}
                    stroke-width="3"
                    in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                    marker-end="url(#arrow)"
                />
                <text
                    x="{x((nodes.nodes[e.source].x + nodes.nodes[e.target].x) / 2) + e.x_shift}"
                    y="{y((nodes.nodes[e.source].y + nodes.nodes[e.target].y) / 2) + e.y_shift}"
                    >
                    {e.weight}
                </text>
                
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
                <img src = "imgs/car.png" alt = "car" x={$rectX} y={$rectY} width="50" height="20" 
                in:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }} />

                <rect x={$rectX} y={$rectY} width="50" height="20" fill="green" 
                in:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 10, easing: cubicInOut }} />
        {/if}


        {#if index > 1}
            {setRect()}
            {resetUserInteraction()}
            {#each edges.edges as e}
                <defs>
                    <!-- A marker to be used as an arrowhead. -->
                    <!-- refX determines how far down the line the marker is. -->
                    <marker
                    id="arrow"
                    viewBox="0 0 10 10"
                    refX="23"
                    refY="5"
                    markerWidth="5"
                    markerHeight="5"
                    orient="auto-start-reverse">
                    <path d="M 0 0 L 10 5 L 0 10 z" />
                    </marker>
                </defs>
                <polyline
                    points={ // String of coordintates x0,y0 x1,y1
                    String(x(nodes.nodes[e.source].x)) + "," + String(y(nodes.nodes[e.source].y))
                            + " " + String(x(nodes.nodes[e.target].x)) + "," + String(y(nodes.nodes[e.target].y))
                            }
                    stroke={e.color}
                    stroke-width="3"
                    marker-end="url(#arrow)"
                />
                <text
                    x="{x((nodes.nodes[e.source].x + nodes.nodes[e.target].x) / 2) + e.x_shift}"
                    y="{y((nodes.nodes[e.source].y + nodes.nodes[e.target].y) / 2) + e.y_shift}"
                    >
                    {e.weight}
                </text>
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
        <!--Draw in solution edges-->
        {#if index === 2}
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
                        in:fade|global={{intro: true, duration: 1000, delay: 10 + (n.id * 200), easing: cubicInOut }}
                        out:fade|global={{intro: true, duration: 500, delay: 0, easing: cubicInOut }}
                    />
                    
                {/each}  
                <text x={x(nodes.nodes[3].x) + 30} y={y(nodes.nodes[3].y) + 5}> 9 + 2 + 4 + 5 </text>
                <text x={x(nodes.nodes[3].x) + 30} y={y(nodes.nodes[3].y) + 30}> = 20 minutes </text>
            {/if}
            <!--Draw in first set of u.est values-->
            {#if index >= 3}
                {#if index ===3}
                    <text x={x(nodes.nodes[2].x) - 20} y={y(nodes.nodes[2].y) - 60}>u.est is the estimated distance between node u and the source node</text>
                {/if}
                {#if index < 7}

                {#each nodes.nodes as n}
                <text x={x(n.x) - 4} y={y(n.y) + 5}>{n.name}</text>

                    <text
                    x="{x(n.x) + uest_vals[n.id]['x_shift']}"
                    y="{y(n.y) + uest_vals[n.id]['y_shift']}"
                    >
                        {n.name}.est = {uest_vals[n.id][0]}
                    </text>
                {/each}
                {/if}
            {/if}
            <!--Show Set table-->
            {#if index > 3}
                {console.log("table should be here")}
                
                <foreignObject x="50" y="200" width="300" height="500">
                    <div>
                        <table style="width:100%">
                            <thead>
                                <tr>
                                    <th>Visited</th>
                                    <th>Unvisited</th>
                                </tr>
                            </thead>
                            <tbody>
                                
                                {#each set_index as i}
                                    {#if index > i}
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
            <!--First set of edge updates-->
            {#if index >= 5}
                {#if index < 7}
                    <polyline
                    points={ // String of coordintates x0,y0 x1,y1 for edge a, f
                    String(x(nodes.nodes[0].x)) + "," + String(y(nodes.nodes[0].y))
                            + " " + String(x(nodes.nodes[5].x)) + "," + String(y(nodes.nodes[5].y))
                            }
                    stroke={pink}
                    stroke-width="5"
                    in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    />
                {/if}
                {#if index < 11}
                <polyline
                points={ // String of coordintates x0,y0 x1,y1 for edge a, b
                String(x(nodes.nodes[0].x)) + "," + String(y(nodes.nodes[0].y))
                        + " " + String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                        }
                stroke={pink}
                stroke-width="5"
                in:draw|global={{intro: true , duration: 1000, delay: 300, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                {/if}
                <polyline
                points={ // String of coordintates x0,y0 x1,y1 for edge a, c
                String(x(nodes.nodes[0].x)) + "," + String(y(nodes.nodes[0].y))
                        + " " + String(x(nodes.nodes[2].x)) + "," + String(y(nodes.nodes[2].y))
                        }
                stroke={pink}
                stroke-width="5"
                in:draw|global={{intro: true , duration: 1000, delay: 600, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[0].id} 
                    cx={x(nodes.nodes[0].x)} 
                    cy={y(nodes.nodes[0].y)} 
                    r="20" 
                    fill={nodes.nodes[0].color}
                />
                <text x={x(nodes.nodes[0].x) - 4} y={y(nodes.nodes[0].y) + 5}>a</text>
                <text x={x(nodes.nodes[5].x) - 4} y={y(nodes.nodes[5].y) + 5}>f</text>
                <text x={x(nodes.nodes[1].x) - 4} y={y(nodes.nodes[1].y) + 5}>b</text>
                <text x={x(nodes.nodes[2].x) - 4} y={y(nodes.nodes[2].y) + 5}>c</text>
                
            {/if}
            <!--First set of u.est updates-->
            {#if index >= 6 && index < 7}
                <text
                    x="{x(nodes.nodes[1].x) + uest_vals[nodes.nodes[1].id]['x_shift']}"
                    y="{y(nodes.nodes[1].y) + uest_vals[nodes.nodes[1].id]['y_shift']}"
                    >
                        b.est =&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt; 0 + 9 = {uest_vals[nodes.nodes[1].id][1]}
                </text>
                <text
                    x="{x(nodes.nodes[2].x) + uest_vals[nodes.nodes[2].id]['x_shift']}"
                    y="{y(nodes.nodes[2].y) + uest_vals[nodes.nodes[2].id]['y_shift']}"
                    >
                        c.est =&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt; 0 + 14 = {uest_vals[nodes.nodes[2].id][1]}
                </text>

                <text
                    x="{x(nodes.nodes[5].x) + uest_vals[nodes.nodes[5].id]['x_shift']}"
                    y="{y(nodes.nodes[5].y) + uest_vals[nodes.nodes[5].id]['y_shift']}"
                    >
                        f.est =&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt; 0 + 7 = {uest_vals[nodes.nodes[5].id][1]}
                </text>
            {/if}

            <!--Set node f to visited-->
            {#if index >= 7}
                <polyline
                points={ // String of coordintates x0,y0 x1,y1 for edge a, f
                String(x(nodes.nodes[0].x)) + "," + String(y(nodes.nodes[0].y))
                        + " " + String(x(nodes.nodes[5].x)) + "," + String(y(nodes.nodes[5].y))
                        }
                stroke={blue}
                stroke-width="5"
                in:draw|global={{intro: true , duration: 1000, delay: 0, easing: cubicInOut }}
                out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                        key={nodes.nodes[5].id} 
                        cx={x(nodes.nodes[5].x)} 
                        cy={y(nodes.nodes[5].y)} 
                        r="20" 
                        fill={blue}
                        in:fade|global={{intro: true , duration: 500, delay: 500, easing: cubicInOut }}
                        out:fade|global={{intro: true , duration: 500, delay: 100, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[0].id} 
                    cx={x(nodes.nodes[0].x)} 
                    cy={y(nodes.nodes[0].y)} 
                    r="20" 
                    fill={blue}
                />
                <text x={x(nodes.nodes[0].x) - 4} y={y(nodes.nodes[0].y) + 5}>a</text>
                <text x={x(nodes.nodes[5].x) - 4} y={y(nodes.nodes[5].y) + 5}>f</text>
                {#if index < 11}
                    {#each nodes.nodes as n}
                    <text x={x(n.x) - 4} y={y(n.y) + 5}>{n.name}</text>

                        <text
                        x="{x(n.x) + uest_vals[n.id]['x_shift']}"
                        y="{y(n.y) + uest_vals[n.id]['y_shift']}"
                        >
                            {n.name}.est = {uest_vals[n.id][1]}
                        </text>
                    {/each}
                {/if}
            {/if}
            {#if index >= 10}
            <polyline
                    points={ // String of coordintates x0,y0 x1,y1 for edge f, d
                    String(x(nodes.nodes[5].x)) + "," + String(y(nodes.nodes[5].y))
                            + " " + String(x(nodes.nodes[3].x)) + "," + String(y(nodes.nodes[3].y))
                            }
                    stroke={pink}
                    stroke-width="5"
                    in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    />
                    <polyline
                    points={ // String of coordintates x0,y0 x1,y1 for edge f, b
                    String(x(nodes.nodes[5].x)) + "," + String(y(nodes.nodes[5].y))
                            + " " + String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                            }
                    stroke={pink}
                    stroke-width="5"
                    in:draw|global={{intro: true , duration: 1000, delay: 300, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    />
                <circle 
                    key={nodes.nodes[5].id} 
                    cx={x(nodes.nodes[5].x)} 
                    cy={y(nodes.nodes[5].y)} 
                    r="20" 
                    fill={blue}
                />
                <circle 
                    key={nodes.nodes[3].id} 
                    cx={x(nodes.nodes[3].x)} 
                    cy={y(nodes.nodes[3].y)} 
                    r="20" 
                    fill={red}
                />
                <circle 
                    key={nodes.nodes[1].id} 
                    cx={x(nodes.nodes[1].x)} 
                    cy={y(nodes.nodes[1].y)} 
                    r="20" 
                    fill={pink}
                />
                <text x={x(nodes.nodes[5].x) - 4} y={y(nodes.nodes[5].y) + 5}>f</text>
                <text x={x(nodes.nodes[3].x) - 4} y={y(nodes.nodes[3].y) + 5}>d</text>
                <text x={x(nodes.nodes[1].x) - 4} y={y(nodes.nodes[1].y) + 5}>b</text>

                <!-- Update b.est and d.est -->
                {#if index < 11}
                <text
                    x="{x(nodes.nodes[1].x) + uest_vals[nodes.nodes[1].id]['x_shift']}"
                    y="{y(nodes.nodes[1].y) + uest_vals[nodes.nodes[1].id]['y_shift']}"
                    >
                        b.est = &nbsp;&nbsp; &lt; 7 + 10 = {uest_vals[nodes.nodes[1].id][2]}
                </text>
                <text
                    x="{x(nodes.nodes[3].x) + uest_vals[nodes.nodes[3].id]['x_shift']}"
                    y="{y(nodes.nodes[3].y) + uest_vals[nodes.nodes[3].id]['y_shift']}"
                    >
                        d.est = &nbsp;&nbsp;&nbsp;&nbsp; &gt; 7 + 15 = {uest_vals[nodes.nodes[3].id][2]}
                </text>
                {/if}
            {/if}
            <!--Add node b to visited-->
            {#if index >= 11}
                {#if index < 13}
                {#each nodes.nodes as n}
                <text x={x(n.x) - 4} y={y(n.y) + 5}>{n.name}</text>

                    <text
                    x="{x(n.x) + uest_vals[n.id]['x_shift']}"
                    y="{y(n.y) + uest_vals[n.id]['y_shift']}"
                    >
                        {n.name}.est = {uest_vals[n.id][2]}
                    </text>
                {/each}
                {/if}
                <polyline
                    points={ // String of coordintates x0,y0 x1,y1 for edge a, b
                    String(x(nodes.nodes[0].x)) + "," + String(y(nodes.nodes[0].y))
                            + " " + String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                            }
                    stroke={blue}
                    stroke-width="5"
                    in:draw|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[1].id} 
                    cx={x(nodes.nodes[1].x)} 
                    cy={y(nodes.nodes[1].y)} 
                    r="20" 
                    fill={blue}
                    in:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[0].id} 
                    cx={x(nodes.nodes[0].x)} 
                    cy={y(nodes.nodes[0].y)} 
                    r="20" 
                    fill={blue}
         
                />
                <text x={x(nodes.nodes[0].x) - 4} y={y(nodes.nodes[0].y) + 5}>a</text>
                <text x={x(nodes.nodes[1].x) - 4} y={y(nodes.nodes[1].y) + 5}>b</text>

            {/if}
            {#if index >= 12}
                <polyline
                        points={ // String of coordintates x0,y0 x1,y1 for edge f, b
                        String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                                + " " + String(x(nodes.nodes[2].x)) + "," + String(y(nodes.nodes[2].y))
                                }
                        stroke={pink}
                        stroke-width="5"
                        in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                        out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <polyline
                        points={ // String of coordintates x0,y0 x1,y1 for edge f, b
                        String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                                + " " + String(x(nodes.nodes[3].x)) + "," + String(y(nodes.nodes[3].y))
                                }
                        stroke={pink}
                        stroke-width="5"
                        in:draw|global={{intro: true , duration: 1000, delay: 300, easing: cubicInOut }}
                        out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                        />
                <circle 
                        key={nodes.nodes[1].id} 
                        cx={x(nodes.nodes[1].x)} 
                        cy={y(nodes.nodes[1].y)} 
                        r="20" 
                        fill={blue}
            
                    />
                <circle 
                    key={nodes.nodes[3].id} 
                    cx={x(nodes.nodes[3].x)} 
                    cy={y(nodes.nodes[3].y)} 
                    r="20" 
                    fill={red}
            
                />
                <text x={x(nodes.nodes[1].x) - 4} y={y(nodes.nodes[1].y) + 5}>b</text>
                <text x={x(nodes.nodes[2].x) - 4} y={y(nodes.nodes[2].y) + 5}>c</text>
                <text x={x(nodes.nodes[3].x) - 4} y={y(nodes.nodes[3].y) + 5}>d</text>
                {#if index < 13}
                    <text
                            x="{x(nodes.nodes[3].x) + uest_vals[nodes.nodes[3].id]['x_shift']}"
                            y="{y(nodes.nodes[3].y) + uest_vals[nodes.nodes[3].id]['y_shift']}"
                            >
                                d.est = &nbsp;&nbsp;&nbsp;&nbsp; &gt; 9 + 12 = {uest_vals[nodes.nodes[3].id][3]}
                    </text>
                    <text
                            x="{x(nodes.nodes[2].x) + uest_vals[nodes.nodes[2].id]['x_shift']}"
                            y="{y(nodes.nodes[2].y) + uest_vals[nodes.nodes[2].id]['y_shift']}"
                            >
                                c.est = &nbsp;&nbsp;&nbsp;&nbsp; &gt; 9 + 2 = {uest_vals[nodes.nodes[2].id][3]}
                    </text>
                {/if}
            {/if}
            <!--Add node c to Visited-->
            {#if index >= 13}
                {#each nodes.nodes as n}
                    <text x={x(n.x) - 4} y={y(n.y) + 5}>{n.name}</text>

                        <text
                        x="{x(n.x) + uest_vals[n.id]['x_shift']}"
                        y="{y(n.y) + uest_vals[n.id]['y_shift']}"
                        >
                            {n.name}.est = {uest_vals[n.id][3]}
                        </text>
                {/each}
                <polyline
                    points={ // String of coordintates x0,y0 x1,y1 for edge f, b
                    String(x(nodes.nodes[1].x)) + "," + String(y(nodes.nodes[1].y))
                            + " " + String(x(nodes.nodes[2].x)) + "," + String(y(nodes.nodes[2].y))
                            }
                    stroke={blue}
                    stroke-width="5"
                    in:draw|global={{intro: true , duration: 1000, delay: 100, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[2].id} 
                    cx={x(nodes.nodes[2].x)} 
                    cy={y(nodes.nodes[2].y)} 
                    r="20" 
                    fill={blue}
                    in:fade|global={{intro: true , duration: 500, delay: 500, easing: cubicInOut }}
                    out:fade|global={{intro: true , duration: 500, delay: 0, easing: cubicInOut }}
                />
                <circle 
                    key={nodes.nodes[1].id} 
                    cx={x(nodes.nodes[1].x)} 
                    cy={y(nodes.nodes[1].y)} 
                    r="20" 
                    fill={blue}
                />
                <text x={x(nodes.nodes[1].x) - 4} y={y(nodes.nodes[1].y) + 5}>b</text>
                <text x={x(nodes.nodes[2].x) - 4} y={y(nodes.nodes[2].y) + 5}>c</text>
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
