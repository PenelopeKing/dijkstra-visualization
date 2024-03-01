<script>
    import Scroller from "./Scroller.svelte";
    import Nodes from "./Nodes.svelte";
    let count, index, offset, progress;
    let width = 800;
    let height = 100;

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
    <p>Pg 2: Interactive Map</p>
    <br/>
    <p class = "text" align= left>The restaurant you are currently at is the blue dot at the left.</p>
    <p class = "text" align= left>These pink dots are neighborhoods you can pass through on your drive.</p>
    <p class = "text" align= left>The numbers represent how long it takes to get from one neighborhood to another.</p>
    <p class = "text" align= left>This red dot is your final destination.</p>
    <br />
    <p class = "text" align= left>Try to find the fastest path possible to your destination. Create the path you think is best by clicking on the nodes. Scroll down when you are done!</p>
  
  </section>
  <section>
    <p>Pg 3: Answer</p>
    <br/>
    <p class = "text" align= left>Was this what you got?</p>
    <p class = "text" align = left>Finding the shortest path is not a simple task…
      How can we find the concrete shortest path to your customer?
      </p>
    <p class = "text" align = left>One method to do so in this situation is called Dijkstra Algorithm.</p>

  </section>
  <section>
    <p>Pg 4: Start of Dijkstra Explanation... Set Up 2</p>
    <br/>
    <p class = "text" align = left>Animation to show the right answer to the user.</p>
  </section>
  <section>
    <p>Pg 5: Node a </p>
    <br/>
    <p class = "text" align = left>Make u.est appear for all nodes</p>
  </section>
  <section>
    <p>Pg 6</p>
    <br/>
    <p class = "text" align = left>Make table for C and Outside Appear</p>
  </section>
  <section>
    <p>Pg 7</p>
    <br/>
    <p class = "text" align = left>Highlight Nodes F, B, C</p>
  </section>
  <section>
    <p>Pg 8</p>
    <br/>
    <p class = "text" align = left>Calculate distance from A to F, show it</p>
  </section>
  <section>
    <p>Pg 9</p>
    <br/>
    <p class = "text" align = left>Compare distance values to u.est, replace with new value</p>
  </section>
  <section>
    <p>Pg 10</p>
    <br/>
    <p class = "text" align = left>Move f from Outside to C</p>
  </section>
  <section>
    <p>Pg 11: node b </p>
    <br/>
    <p class = "text" align = left>Animate calculations for node b</p>
  </section>
  <section>
    <p>Pg 12: node c </p>
    <br/>
    <p class = "text" align = left>Animate calculations for node c</p>
  </section>
  <section>
    <p>Pg 13: node e </p>
    <br/>
    <p class = "text" align = left>Animate calculations for node e</p>
  </section>
  <section>
    <p>Pg 14: node d </p>
    <br/>
    <p class = "text" align = left>Animate calculations for node d</p>
  </section>
  <section>
    <p>Pg 15: Final Solution</p>
    <br/>
    <p class = "text" align = left>Show final solution with all updated edge weights</p>
  </section>
  <section>
    <p>Pg 16: End</p>
    <br/>
    <p class = "text" align = left>End notes on dijkstra alg.</p>
  </section>
</div>

</Scroller>


<style>
 .text {
  margin-left:750px;
  margin-right: 50px;
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
    width: 70%;
    height: 100vh;
    margin:0;
    padding:0;
    position: relative;
    outline: green solid 3px;
    z-index: 10;
  }
  .foreground {
    width: 100%;
    padding: 0;
    margin: 0;
    height: auto;
    position: relative;
    outline: red solid 3px;
    z-index: 0;
  }
  section {
    height: 95vh;
    /* color: white; */
    outline: magenta solid 3px;
    text-align: center;
    max-width: 1500px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }
  span{
    outline: orange solid 3px;
  }
  h1 {
    background-color: #499e97;
    padding: 60px;
    margin: 0px;
    outline: orange 3px;
  }


</style>
