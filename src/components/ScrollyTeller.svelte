<script>
  import Scroller from "./Scroller.svelte";
  import Nodes from "./Nodes.svelte";
  let count, index, offset, progress;
  let width = 800;
  let height = 120;

  function handleClick(event) {
    console.log("clicked on whole background");
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
    on:click={() => handleClick(event)}
  >
    <div div style="display: flex; flex-direction: row;">
      <span id="nodes-graph" style="margin-left: 20px;">
        <Nodes {index} {width} {height} />
      </span>
    </div>
  </div>

  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div class="foreground" slot="foreground" style="pointer-events: none;">
    <section class="first-page">
      <div class="header-background">
        <br />
        <br />
        <br />
        <div class="block">
          <h1 align="center">
            Food for Thought: An Introduction to Dijkstra Algorithm Using Food
            Delivery
          </h1>
          <p class="names" align="center" style="color:#fcfcfc">
            Penny King & Garvey Li
          </p>
        </div>
      </div>
      <br />
      <br />
      <br />
      <div class="textbox2">
        <p class="text2" align="left">
          Welcome to your first day at your job at OOperEats. It is time to make
          your first delivery. Unfortunately our map system is down, so it is up
          to you to decide the path you want to take to your customer’s
          destination. Keep in mind that we want to be as quick as possible so
          you can be ready for your next order!
        </p>
        <p class="text2" align="left">
          Your Goal: Find the fastest path to your destination from the pick-up
          order restaurant.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Part 1: Interactive Map</h3>
      <br />
      <div class="textbox">
        <p class="text-top" align="left">
          The restaurant you are currently at is the <span style="color: blue"
            >blue dot</span
          > at the left.
        </p>
        <p class="text-center" align="left">
          The <span style="color: pink">pink dots</span> are neighborhoods you can
          pass through on your drive.
        </p>
        <p class="text-center" align="left">
          The numbers represent how long it takes to get from one neighborhood
          to another.
        </p>
        <p class="text-center" align="left">
          This <span style="color: red">red dot</span> is your final destination.
        </p>
        <br />
        <p class="text" align="left">
          <b
            >Try to find the fastest path possible to your destination. Create
            the path you think is best by clicking on the nodes.</b
          > Scroll down when you are done!
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Pg 3: Answer</h3>
      <br />
      <div class="textbox">
        <p class="text-top" align="left">Was this what you got?</p>
        <p class="text-center" align="left">
          Finding the shortest path is not a simple task… How can we find the
          concrete shortest path to your customer?
        </p>
        <p class="text" align="left">
          One method to do so in this situation is called <b
            >Dijkstra Algorithm</b
          >.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 4: Start of Dijkstra Explanation... Set Up 2
      </h3>
      <br />
      <div class="textbox">
        <p class="text-top" align="left">
          The Dijkstra Algorithm is very similar to the Breadth First Search
          algorithm, and lets us find the shortest path between a source and
          target node on a positively weighted graph (directed or undirected).
        </p>
        <p class="text" align="left">
          To start out, we define a value u.est for each node u, denoting its
          estimated distance from the source node.
        </p>
        <p class="text" align="left">
          For the initial source node, this equals 0, and is unknown for all
          other nodes, so we can set them to positive infinity.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 5: Initialize Visited and Unvisited Sets
      </h3>
      <br />

      <div class="textbox">
        <p class="text-top" align="left">
          To help us keep track of which edges we need to update, we'll
          categorize the nodes into two sets: Visited and Unvisited. Nodes in
          Visited have been 'visited', while Nodes in Unvisited have not.
        </p>
        <p class="text" align="left">
          With the setup done, we're ready to start.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 6: Find the nodes closest to the set of Visited nodes.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">
          The first step is to look at all the nodes in Unvisited that are 1
          edge away from (or adjacent) the nodes in C. In this case, those nodes
          are f, b, and c.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 7: Calculate the distance between the source node and the closest
        Unvisited nodes. Update u.est.
      </h3>
      <br />

      <div class="textbox">
        <p class="text-top" align="left">
          Then, we calculate new distance values for each of the nodes adjacent
          to C.
        </p>
        <p class="text" align="left">
          We do this by adding the edge weight to the u.est value of the
          respective source node. If the new distance value is less than the
          respective node’s u.est, then we update the target node’s u.est to be
          the distance value. For node f, 7 &lt; inf, so f’s new u.est is 7.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 8: Find the unvisited Node that is closest to the source.
      </h3>

      <div class="textbox">
        <p class="text" align="left">
          Now, the next step is to find the node in Unvisited closest to our
          source node 'a'.
        </p>
        <p class="text" align="left">In otherwords, find the univisited node with the smallest estimated distance, which in this case is node 'f'.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 9: Move the node from Unvisited to Visited
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">Now, we consider the node 'f' to be visited! So we can move it from the Unvisited set to Visited.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Pg 10: Repeat!</h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">
          And now we repeat those steps until there are no more edges to update
          u.est with for any node 'u'.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 11: Find edges connecting adjacent nodes and update u.est.
      </h3>
      <br />

      <div class="textbox">
        <p class="text" align="left">
          Once again, we find all edges connected to Unvisited nodes adjacent to the Visited nodes.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 12: Find the closest Unvisited node.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">
          Then we find the closest Unvisited node, which is node 'b'.
        </p>
        <p class="text" align="left">Now we can add b to the Visited set.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 13: Find edges connecting adjacent nodes and update u.est.
      </h3>
      <br />

      <div class="textbox">
        <p class="text" align="left">Filler Text</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 14: Find the closest Unvisited node.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">
          Node c has the smallest u.est value of the Unvisited nodes.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 15: Find edges connecting adjacent nodes and update u.est.
      </h3>
      <br />

      <div class="textbox">
        <p class="text" align="left">Filler Text</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 16: Find the closest Unvisited node.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">Filler Text</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 17: Find edges connecting adjacent nodes and update u.est.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">Filler Text</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Pg 18: Finished</h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">
          Now there are no more edges to make updates with nor nodes to visit.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Pg 19: Find edges connecting adjacent nodes and update u.est.
      </h3>
      <br />
      <div class="textbox">
        <p class="text" align="left">Filler Text</p>
      </div>
    </section>
  </div>
</Scroller>

<style>
  .names {
    font-size: 0.7em;
  }
  .page-head {
    margin-left: 70%;
    margin-right: 2%;
  }
  .text {
    padding: 15px;
  }
  .text-center {
    padding-left: 15px;
    padding-right: 15px;
  }
  .text-top {
    padding-top: 15px;
    padding-left: 15px;
    padding-right: 15px;
  }

  .text2 {
    margin: 2%;
    padding: 10px;
    padding-top: -2px;
    padding-bottom: -2px;
    display: inline-block;
  }
  * {
    font-family: "Nunito", sans-serif;
    box-sizing: border-box;
  }
  .textbox2 {
    background: white;
    margin-left: 55%;
    margin-right: 10%;
    border-radius: 5px;
  }
  .textbox {
    background: white;
    margin-left: 70%;
    margin-right: 2%;
    border-radius: 5px;
  }

  .first-page {
    margin: 0;
    padding: 0;
  }

  .header-background {
    margin: 0;
    padding: 0;
    top: 0;
    align-items: center;
    font-family: "Nunito", sans-serif;
    font-size: 25px;
    width: 100%;
    height: 120px;
    color: #fcfcfc;
  }

  .background {
    width: 100%;
    height: 100vh;
    margin: 0;
    padding: 0;
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
    background-color: #ede9dd;
  }
  section {
    height: 95vh;
    /* color: white; */
    text-align: center;
    max-width: 1500px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
    margin-top: 10px;
  }

  .block {
    background-color: #499e97;
    height: 31vh;
    padding: 40px;
    margin: 10px;
    margin-left: 10%;
    margin-top: 6.3%;
    margin-bottom: -10%;
    width: 85vh;
    border-radius: 5px;
  }
  h1 {
    margin-top: -1.5%;
    font-size: 30px;
  }

  h3 {
    background-color: #499e97;
    padding: 15px;
    margin: 0;
    font-family: "Nunito", sans-serif;
    font-size: 20px;
    color: white;
    border-radius: 5px;
  }
</style>
