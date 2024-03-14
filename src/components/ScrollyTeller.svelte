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
        <br/>
        <br/>
        <br/>
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
      <div class="textbox2">
        <p class="text2" align="left">
          Welcome to your first day at your job at OOperEats. It is time to make
          your first delivery. Unfortunately our map system is down, so it is up
          to you to decide the path you want to take to your customer’s
          destination. Keep in mind that we want to be as quick as possible so
          you can be ready for your next order!
          <br/><br/>
          Your Goal: Find the fastest path to your destination from the pick-up order
          restaurant.
        </p>
      </div>
      <br/>
      <div class="textbox2">
        <p class="text2" align="left">
          Food delivery services relies on efficient route planning to deliver meals quickly, but understanding the complexity of optimizing delivery routes can be challenging, especially when considering factors like distance. Furthermore, placing route optimization problems in a familiar and interesting context allows for an easier understanding of route optimization problems.
          <br/><br/>
          Dijkstra's (DYKE-strəz) algorithm is an algorithmn used to find the shortest path between nodes in a weighted graph. It was created by Edsger W. Dijkstra. This website will teach about how to apply Dijkstra's algorithmn in a directed graph using various interactive features.
          <br/><br/>
          Scroll down to continue!
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Try It Yourself!</h3>
      <br/>
      <div class="textbox">
        <p class="text-top" align="left">
          The restaurant you are currently at is the <span style="color: blue"
            >blue dot</span
          > at the left.
        </p>
        <p class="text-center" align="left">
          The <span style="color: pink">pink dots</span> are neighborhoods you
          can pass through on your drive.
          <br/><br/>
          The numbers represent how many minutes it takes to get from one neighborhood
          to another.
          <br/><br/>
          This <span style="color: red">red dot</span> is your final destination.
        </p>
        <br/>
        <p class="text-bottom" align="left">
          <b
            >Try to find the fastest path possible to your destination. Create
            the path you think is best by clicking on the nodes.</b
          > Scroll down when you are done!
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Answer</h3>
      <br/>
      <div class="textbox">
        <p class="text-top" align="left">Was this what you got?</p>
        <p class="text-center" align="left">
          Finding the shortest path is not a simple task… How can we find the
          concrete shortest path to your customer?
        </p>
        <p class="text-bottom" align="left">
        One method to do so in this situation is called <b>Dijkstra Algorithm</b
        >
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Start of Dijkstra Explanation
      </h3>
      <br/>
      <div class="textbox">
        <p class="text-top" align="left">
          The Dijkstra Algorithm is very similar to the Breadth First Search
          algorithm, and lets us find the shortest path between a
          <span style="color: blue">source</span>
          and
          <span style="color: red">target</span>
          node on a positively weighted graph (directed or undirected).
        </p>
        <p class="text-bottom" align="left">
          To start out, we define a value
          <b>
            u.est for each node u, denoting its estimated distance from the
            source node.
          </b>
          <br/><br/>
          For the initial source node <i>a</i>, this equals 0, and is unknown for all
          other nodes, so we set them to positive infinity.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Initialize Visited and Unvisited Sets
      </h3>
      <br/>

      <div class="textbox">
        <p class="text-top" align="left">
          To help us keep track of which nodes to update and which edges to look
          at, we'll categorize the nodes into two sets: Visited and Unvisited.
          <br/><br/>
          Nodes in Visited have a <b>known shortest distance</b> from the source
          node, while nodes in Unvisited do not.
        </p>
        <p class = "text-bottom" align = "left">
          With the setup done, we're ready to start.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited nodes adjacent to the set of Visited nodes.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text-top" align="left">
          The first step is to look at all the nodes in Unvisited that are <b
            >1 edge away from (or adjacent to) the Visited nodes</b
          >. </p>
          <p class = "text-bottom" align = "left">In this case, those nodes are <i>f</i>, <i>b</i>, and <i>c</i>.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Update distance estimates (u.est)
      </h3>
      <br/>

      <div class="textbox">
        <p class="text-top" align="left">
          Then, we calculate new distance values for each of the Unvisited nodes adjacent
          to the Visited nodes.
          <br/><br/>
          We do this by <b>adding the edge weight to the u.est value of the connected Visited node</b>. 
          <br/><br/>
          If the new distance value is less than the respective Unvisited node’s
          u.est, then we update u.est to be the calculated distance value.
          <br/><br/>
          For node <i>f</i>, infinity &gt; 0 + 7, so <i>f.est</i> is now 7.
          <br/><br/>
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited Node closest to the source.
      </h3>

      <div class="textbox">
        <p class="text-top" align="left">
          Now, the next step is to find the node in Unvisited closest to our
          source node <i>a</i>.
          </p>
          <p class = "text-bottom" align = "left">
          In otherwords, <b>find the node in Unvisited with the smallest estimated
          distance</b>, which in this case is node <i>f</i>
          <br/>
          <br/>
          Dijkstra's algorithm is a greedy algorithm... it optimally selects the next unvisited node with the shortest known distance from the source at each step.
        </p>
      </div>

    </section>
    <section>
      <h3 class="page-head" align="left">
        Move the node from Unvisited to Visited
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          Now, we consider the node <i>f</i> to be visited! So we can move it to the Visited set.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Repeat!</h3>
      <br/>
      <div class="textbox">
        <p class = "text" align= "left">
          And now we repeat those steps until there are no more edges to update
          <i>u.est</i> with for any node <i>u</i>.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited nodes adjacent to the set of Visited nodes and update <i>u.est</i>.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          Once again, <b>we find all Unvisited nodes adjacent to
          the Visited nodes</b> and <b>update <i>u.est</i></b> for those Unvisited nodes.
          <br/><br/>
          Although we've already found node <i>b</i>, there is another edge leading to it from node <i>f</i>, so we still need to check <i>b.est</i>. 
          <br/><br/>
          Since the new calculated distance for <i>b</i> is greater than <i>b.est</i>, the estimate stays the same.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited Node closest to the source.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          Now we <b>find the Unvisited node with the smallest <i>u.est</i></b>, which is node <i>b</i>.
          <br/><br/>
          Then we can add <i>b</i> to the Visited set.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited nodes adjacent to the set of Visited nodes and update <i>u.est</i>.
      </h3>
      <br/>

      <div class="textbox">
        <p class="text" align="left">Again, <b>we find all Unvisited nodes adjacent to
          the Visited nodes</b> and <b>update <i>u.est</i></b> for those Unvisited nodes.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited Node closest to the source.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          Then we <b>find the Unvisited node with the smallest <i>u.est</i></b>, which is node <i>c</i>, and <b>move it to the Visited set</b>.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited nodes adjacent to the set of Visited nodes and update <i>u.est</i>.
      </h3>
      <br/>

      <div class="textbox">
        <p class="text" align="left">We <b>find the Unvisited nodes adjacent to the Visited nodes</b> again, and <b>update their <i>u.est</i> values</b>.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited Node closest to the source.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left"><b>Find the adjacent Unvisited node with the smallest <i>u.est</i></b> and <b>move it to Visited</b>, which for this step is node <i>e</i>.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        Find the Unvisited nodes adjacent to the set of Visited nodes and update <i>u.est</i>.
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">Again, we <b>find the Unvisited nodes adjacent to the Visited nodes</b>, and <b>update their <i>u.est</i> values</b>.</p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">Find the Unvisited Node closest to the source.</h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          And now, for the last time, we find the adjacent Unvisited node with the smallest <i>u.est</i>.
          Now there are no more edges to make updates with nor nodes to visit!
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        All done!
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">We have completed Dijsktra's Algorithm for this positively weighted and directed graph. 
          <br/><br/>
          But wait a minute...
          <br/><br/>
          This doesn't look exactly like our solution from before!
          <br/><br/>
          As it turns out, Dijsktra's Algorithm finds the shortest path between a source node and <b>ALL</b> the nodes in a positively weighted graph.
        </p>
      </div>
    </section>
    <section>
      <h3 class="page-head" align="left">
        So what is the fastest route from the restaurant to the final destination?
      </h3>
      <br/>
      <div class="textbox">
        <p class="text" align="left">
          To get our solution from before, we simply only consider the path that connects the source and target nodes.
        </p>
      </div>
    </section>

    <section>
      <h3 class="page-head-entire" align="left">
        Summary
      </h3>
      <div class="textbox-entire">
        <p class="text-top" align="left">
          By visualizing Dijkstra's algorithm in action with a food delivery destination and an interactive setting, grasping Dijsktra's Algorithmn and understanding route optimization becomes much more simple. Furthermore, through digestable steps, the algorithmn becomes much more simple.
          The one thing that everyone should be able to walk away from our visualization with is a better understanding of Dijkstra's Algorithm and the applications in which it can be used. 
          <br/>
          <br/>
          In our interactive exploration of Dijkstra's algorithm within the context of a food delivery service, we've uncovered a fundamental principle: efficiency through optimization. While our focus has been on finding the shortest path for delivering food, the underlying lesson extends far beyond this scenario. In other contexts other than this, optimzing routes through greedy algorithms such as Dijkstra's Algorithmn saves time and resources, leading to more efficient processes in general.
        </p>
        <p class="text-bottom" align="left">
          Scroll down to try a different example now that you know Dijkstra's Algorithmn!
        </p>
      </div>

    </section>

    <section>
    <h3 class="page-head" align="left">
      Your Turn!
    </h3>
    <br/>
    <div class="textbox">
      <p class="text-top" align="left">
        Now that you are more familiar with Dijkstra's Algorithm, try finding the shortest path to your customer in this setting.
      </p>
      <p class="text-bottom" align="left">
        Click the nodes to move your car. Scroll down to see the answer.
      </p>
    </div>
  </section>
    <section>
    <h3 class="page-head" align="left">
      Your Turn! – Solution
    </h3>
    <br/>
    <div class="textbox">
      <p class="text" align="left">
        Was this what you got?
        </p>
        <p class="text" align="left">
        The <span style="color: red">red</span> lines show the right answer.
        The <span style="color: blue">blue</span> lines are your previous answer.
      </p>
    </div>
  </section>

  </div>
</Scroller>

<style>
  .names {
    font-size: 0.7em;
  }
  .page-head {
    margin-left: 3%;
    margin-right: 65%;
  }
  .page-head-entire {
    margin-left: 20%;
    margin-right: 20%;
  }
  .text {
    margin: 2%;
    padding: 15px;
    padding-left: 10px;
    margin-bottom: 20px;
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
  .text-bottom {
    padding-bottom: 15px;
    padding-left: 15px;
    padding-right: 15px;
  }

  .text2 {
    margin: 2%;
    padding: 15px;
    display: inline-block;
  }
  * {
    font-family: "Nunito", sans-serif;
    box-sizing: border-box;
  }
  .textbox2 {
    background: white;
    margin-left: 55%;
    margin-right: 7%;
    border-radius: 5px;
  }
  .textbox-entire {
    background: white;
    margin-left: 20%;
    margin-right: 20%;
    border-radius: 5px;
  }

  .textbox {
    background: white;
    margin-left: 3%;
    margin-right: 65%;
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
    padding-right:400px;
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
