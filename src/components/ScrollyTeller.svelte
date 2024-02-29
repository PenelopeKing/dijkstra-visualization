<script>
    import Scroller from "@sveltejs/svelte-scroller";
    import Nodes from "./Nodes.svelte";
    let count, index, offset, progress;
    let width = 900;
    let height = 100;


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

<div
    class="background"
    slot="background"
    bind:clientWidth={width}
    bind:clientHeight={height}>
    <Nodes {index} {width} {height} />
</div>

<div class="foreground" slot="foreground"
style="pointer-events: none;">
  <section>Pg 1: Interactive Set Up - Premise</section>
  <section>Pg 2: Interactive Map Animation</section>
  <section>Pg 3: Interactive Map - Free Reign </section>
  <section>Pg 4: Start of Dijkstra Explanation... Set Up</section>
  <section>Pg 5: Node a </section>
  <section>Pg 6: Node f</section>
  <section>Pg 7: Node b</section>
  <section>Pg 8: Node c</section>
  <section>Pg 9: Node d</section>
  <section>Pg 10: Node e</section>
  <section>Pg 11: Explanation cont... </section>
  <section>Pg 12: Final Solution </section>
</div>

</Scroller>


<style>
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: green solid 3px;
  }
  .foreground {
    width: 90%;
    margin: 0 auto;
    height: auto;
    position: relative;
    outline: red solid 3px;
  }
  section {
    height: 90vh;
    /* color: white; */
    outline: magenta solid 3px;
    text-align: center;
    max-width: 1500px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }
</style>
