<script>
    import Scroller from "./Scroller.svelte";
    import Nodes from "./Nodes.svelte";
    let count, index, offset, progress;
    let width = 800;
    let height = 120;

    function handleClick(event) {
        console.log("clicked on whole background")
        // Update position of rectangle to follow edge to clicked node
    }


</script>
<Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress

>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div
    class="background"
    slot="background"
    bind:clientWidth={width}
    bind:clientHeight={height}
    style="pointer-events: auto"
    on:click={() => handleClick(event)}>
    <div div style="display: flex; flex-direction: row;">
      <span id = "nodes-graph" style="margin-left: 20px;"> <Nodes {index} {width} {height} /> </span>
  </div>
</div>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="foreground" 
slot="foreground"
style="pointer-events: none;">
  <section class = "first-page">
    <div class = "header-background">
        <h1 align="center">An Introduction to Dijkstra Algorithm</h1>
    </div>
      <p style="color:#fcfcfc" >Penny King & Garvey Li</p>
      <br/>
      <br/>
      <p class = "text" align= left >Welcome to your first day at your job at OOperEats. It is time to make your first delivery. Unfortunately our map system is down so it is up to you to decide the path you want to take to your customer’s destination. Keep in mind that we want to be as quick as possible so you can be ready for your next order pick up!
        </p>
      <p class = "text" align= left> Your Goal: Find the fastest path to your destination from the restaurant pick up.</p>
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 2: Interactive Map</h3>
    <br/>
    <p class = "text" align= left>The restaurant you are currently at is the blue dot at the left.</p>
    <p class = "text" align= left>These pink dots are neighborhoods you can pass through on your drive.</p>
    <p class = "text" align= left>The numbers represent how many minutes takes to get from one neighborhood to another.</p>
    <p class = "text" align= left>This red dot is your final destination.</p>
    <br />
    <p class = "text" align= left>Try to find the fastest path possible to your destination. Create the path you think is best by clicking on the nodes. Scroll down when you are done!</p>
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 3: Answer</h3>
    <br/>
    <p class = "text" align= left>Was this what you got?</p>
    <p class = "text" align = left>Finding the shortest path is not a simple task…
      How can we find the concrete shortest path to your customer?
      </p>
    <p class = "text" align = left>One method to do so in this situation is called Dijkstra Algorithm.</p>

  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 4: Start of Dijkstra Explanation... Set Up 2</h3>
    <br/>
    <p class = "text" align = left>The Dijkstra Algorithm is very similar to the Breadth First Search algorithm, and lets us find the shortest path between a source and target node on a positively weighted graph (directed or undirected).</p>
      <p class = "text" align = left>To start out, we define a value u.est for each node denoting the distance from the source node, where u is the name of the node. For the initial source node, this equals 0, and is unknown for all other nodes, so we can set them to positive infinity.</p>

  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 5: Initialize C and Outside Sets </h3>
    <br/>
    <p class = "text" align = left>
      To help us keep track of which edges we need to update, we’ll categorize the nodes into two sets: C and Outside. Nodes in C have been 'visited', while Nodes in Outside have not.
    </p>
    <p class = "text" align = left>
      With the setup done, we’re ready to start.
    </p>
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 6: Find the closest nodes to C.</h3>
    <br/>
    <p class = "text" align = left>The first step is to look at all the nodes in Outside that are 1 edge away from (or adjacent) the nodes in C. In this case, those nodes are f, b, and c.</p>
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 7: Calculate the distance between the source node of C and the closest Outside nodes. Update u.est.</h3>
    <br/>
    <p class = "text" align = left>Then, we calculate new distance values for each of the nodes adjacent to C.</p>
    <p class = "text" align = left>We do this by adding the edge weight to the u.est value of the respective source node. 
      If the new distance value is less than the respective node’s u.est, then we update the target node’s u.est to be the distance value. For node f, 7 &lt; inf, so f’s new u.est is 7.
    </p>
  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 8: Find the unvisited Node that is closest to the source.</h3>
    <p class = "text" align = left>Now, the next step is to find the node in Outside closest to our source node 'a', which in this case is node 'f'.</p> 
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 9: Move the node from Outside to C</h3>
    <p class = "text" align = left>Node f is now considered to be visited! So, we can move it from the Unvisited set to the Visited set.</p> 

    <p class = "text" align = left></p>
  </section>
  <section>
    <h3 class = "page-head" align = left>Pg 10: Repeat!</h3>
    <br/>
    <p class = "text" align = left>And now we repeat those steps until there are no more edges to update u.est with for any node.</p>

  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 11: Find edges connecting adjacent nodes and update u.est. </h3>
    <br/>
    <p class = "text" align = left>So once again, we find the edges connected to the Visited set and update their estimated distances.</p>
  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 12: Find the shortest edge connected to the Visited set.</h3>
    <br/>
    <p class = "text" align = left>Filler Text</p>
  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 13: Final Solution</h3>
    <br/>
    <p class = "text" align = left>Filler Text</p>
  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 14: Final Solution</h3>
    <br/>
    <p class = "text" align = left>Filler Text</p>
  </section>
  <section>
    <h3 class = "page-head" align = left >Pg 15: Final Solution</h3>
    <br/>
    <p class = "text" align = left>Filler Text</p>
  </section>

</div>

</Scroller>


<style>
  .page-head {
    margin-left:71%;
    margin-right: 2%;
  }
 .text {
  margin-left:72%;
  margin-right: 2%;
 }
  * {
    font-family: 'Nunito', sans-serif;
  }

  .first-page{
    margin:0;
    padding:0;
  }

  .header-background {
    margin:0;
    padding:0;
    top: 0;
    align-items: center;
    font-family: 'Nunito', sans-serif;
    font-size: 25px;
    width: 100%;
    height: 120px;
    color: #fcfcfc;
  }

  .background {
    width: 100%;
    height: 100vh;
    margin:0;
    padding:0;
    position: relative;
    z-index: 10;
  }
  .foreground {
    width: 100%;
    padding: 0;
    margin: 0;
    height: auto;
    position: relative;
    z-index: 0;
  }
  section {
    height: 95vh;
    /* color: white; */
    text-align: center;
    max-width: 1500px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
    margin-top:10px;
  }

  h1 {
    background-color: #499e97;
    height: 80vh;
    padding: 60px;
    margin: 0px;
  }

  h3 {
    background-color: #499e97;
    padding: 15px;
    margin: 0;
    font-family: 'Nunito', sans-serif;
    font-size:20px;
    color:white;
    border-radius: 5px;
  }


</style>
