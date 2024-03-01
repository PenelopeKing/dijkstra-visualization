<script>
	import ar from "https://unpkg.com/perfect-arrows@0.3.2/dist/perfect-arrows.cjs.development.js"
	let getArrow = ar.getArrow
  const p1 = { x: 360, y: 240 }
  let p2 = { x: 128, y: 96 }
	let sx, sy, cx, cy, ex, ey, ae, as, ec, endAngleAsDegrees;
	
  $:[sx, sy, cx, cy, ex, ey, ae, as, ec] = getArrow(p1.x, p1.y, p2.x, p2.y, {
    padEnd: 20,
  })
</script>

<div>
	<svg
      view-box="0 0 720 480"
      style="width: 720px; height: 480px"
      stroke="#000"
      fill="#000"
      stroke-width="3px"
    >
      <circle cx={sx} cy={sy} r=4 />
      <path d="M{sx},{sy} Q{cx},{cy} {ex},{ey}" fill="none" />
      <polygon
        points="0,-6 12,0, 0,6"
        transform="translate({ex},{ey}) rotate({ae * (180 / Math.PI)})"
      />
    </svg>
</div>	
<style>
	div { width: 720px; height: 480px; background-color:#eaeaea }
</style>