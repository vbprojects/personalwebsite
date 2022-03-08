<main>
	<!-- <canvas id="background"></canvas> -->
	<div id="top">
		<div id="left">
			<div id="intro">
				<p>Hi,<br>I'm Varun Bhatnagar,</p>
			</div>
			<div id="spacer"></div>
			<div id="desc">
				<p>An undergraduate student at Penn State for computational data science. Check out some of my work.</p>
			</div>
		</div>
		<div id="right">
			<!-- <div id="spacer2"></div> -->
			<div id="buttons">
                <div><a href="www.Linkdin.com"><p>Linkdin</p></a></div>
                <div><a href="www.Linkdin.com"><p>Github</p></a></div>
                <div><a href="www.Linkdin.com"><p>Kagggle</p></a></div>
                <div><a href="www.Linkdin.com"><p>Gallery</p></a></div>
            </div>
		</div>
	</div>
</main>



<style>
	@import url('https://fonts.googleapis.com/css2?family=Lato&display=swap');
	@import url('https://fonts.googleapis.com/css2?family=M+PLUS+1+Code:wght@200;400;500&display=swap');
	@import url('https://fonts.googleapis.com/css2?family=Courier+Prime:ital@1&family=M+PLUS+1+Code:wght@200;400;500&display=swap');
	@import url('https://fonts.googleapis.com/css2?family=Courier+Prime:ital@1&family=Cutive+Mono&family=M+PLUS+1+Code:wght@200;400;500&display=swap');
	@import url('https://fonts.googleapis.com/css2?family=Courier+Prime:ital@1&family=Cutive+Mono&family=M+PLUS+1+Code:wght@200;400;500&family=Major+Mono+Display&display=swap');
	:global(body){
		background-color: #6320EE;
		display: flex;
		justify-content: center;
		text-align: center;
		padding: 0px;

		/* padding-top: 5%; */
	}
    #buttons {
        /* margin: 100px; */
        margin-top: 0;
        padding-top: 0;
        display: flex;
        justify-content: space-between;
        /* border: 1px solid green; */
        align-items: center;
        flex-direction: column;
        gap: 10px;
        width: 100%;
        height: 100%;
    }
    #buttons div{ /* Some padding */
        box-shadow: inset 0 0 0 1px rgba(255,255,255,0.08);
        height: 95%;
        width: 95%;
        vertical-align: middle;
    }
    #buttons div{ /* Some padding */
        box-shadow: inset 0 0 0 1px rgba(255,255,255,0.08);
        height: 95%;
        width: 95%;
        vertical-align: middle;
    }
	main {
		/* padding: 0px; */
		/* margin: 0px; */
		width: 100%;
		height: 100%;
	}
	#top{
		/* width: 100%;
		height: 95%;
		margin: 0px;
		padding: 0px;
		/* background-color: white; */
		/* display: inline-block; */
        width: 1200px;
		height: 800px;
		display: grid;
		margin: auto;
		margin-top: 30px;
		grid-template-columns: repeat(2, 1fr);
		gap: 10px;
		grid-auto-rows: minmax(100px, auto);
		/* background-color: red; */
		
	}
	#right{
		width: 100%;
		height: 100%;
		display: inline-block;
		vertical-align: top;
        -webkit-backdrop-filter: blur(8px);  
		backdrop-filter: blur(8px); 
		box-shadow: inset 0 0 0 1px rgba(255,255,255,0.08);
        margin-top: 0;
        padding-top: 0;
	}
	#left{
		width: 100%;
		height: 100%;
		display: inline-block;
		vertical-align: top;
        -webkit-backdrop-filter: blur(8px);  /* Safari 9+ */
		backdrop-filter: blur(8px); /* Chrome and Opera */
		box-shadow: inset 0 0 0 1px rgba(255,255,255,0.08);
		/* background-color: purple; */
	}
	#intro{
		padding-top: 5vh;
	}
	p{
		font-size: 2em;
		text-align: left;
		padding-left: 10%;
		font-family: 'M PLUS 1 Code', sans-serif;
		/* font-family: Lato, sans-serif; */
		/* color: #98B6B1; */
		color: #00BA97;
		line-height: 1.6;
	}
	#spacer{
		height: 2.5vh;
	}
	#spacer2{
		height: 20vh;
	}
	#background{
		position: absolute;
		width: 100%;
		height: 100%;
	}
	:global(.tri){
		fill: #28273F;
		z-index: -1;
	}
	:global(#csvg){
		z-index: -2;
		width: 100%;
		height: 100%;
		position: absolute;
		/* visibility: hidden; */
	}
	:global(#tsvg){
		z-index: -1;
		width: 100%;
		height: 100%;
		position: absolute;
	}
</style>

<svelte:head>
	<!-- elements go here -->

</svelte:head>


<script>
    import { metatags } from '@roxi/routify'
    
    metatags.title = 'My Routify app'
    metatags.description = 'Description coming soon...'
	import * as d3 from "d3"
	import * as R from "ramda"
	import {Delaunay} from "d3-delaunay"
	const n = 200;
	const M = 4_000;
	const arr = () => R.map((x) => (Math.random()*1.1 - .2 * 1.1) * M, R.range(0, n))
	const pnts = R.zip(arr(), arr())
	const del = Delaunay.from(pnts)
	const vor = Array.from(del.voronoi([-.2 * 1.1 * M, -.2 * 1.1 * M, M, M]).cellPolygons()).sort((x, y) => x[0][1] < y[0][1])
	const triangles = Array.from(del.trianglePolygons()) //R.map((i) => del.trianglePolygon(i), R.range(0, del.triangles.length / 3))
	var svg = d3.select("body")
					.append("svg")
					.attr("id", "tsvg")
	var csvg = d3.select("body").append("svg").attr("id", "csvg")
	const dist = (x1, x2, y1, y2) => Math.sqrt(Math.pow(x1 - x2, 2) + Math.pow(y1 - y2, 2))
	const mix = (p) => (x1, x2, y1, y2) => {
		const dx = x2 - x1
		const dy = y2 - y1
		return [x1 + dx * p, y1 + dy * p]
	}
	const centers = R.map((t) => {
		const x1 = t[0][0]
		const x2 = t[1][0]
		const x3 = t[2][0]
		const y1 = t[0][1]
		const y2 = t[1][1]
		const y3 = t[2][1]
		const a = dist(x1, x2, y1, y2)
		const b = dist(x1, x3, y1, y3)
		const c = dist(x2, x3, y2, y3)
		return [(a*x1 + b*x2 + c*x3) / (a + b + c), (a*y1 + b*y2 + c*y3) / (a + b + c)]
	}, triangles)
	var ptris = R.zip(centers, triangles)
	ptris.sort((x, y) => x[0][0] < y[0][0])
	const npnts = (p) => R.map((t) => {
		const c = t[0]
		const ps = t[1]
		const nps = R.map((P) => {
			return mix(p)(P[0], c[0], P[1], c[1])
		}, ps)
		return R.join(' ', R.map((tt) => R.join(',', tt), nps))	
	}, ptris)
	const tris = R.zip(npnts(.02), npnts(.15))
	R.map((t) => {	
		svg.append("polygon")
			.attr("points", t[0])//pntss)
			.style("position", "fixed")
			.attr("class", "tri")
	}, tris)
	R.map((t) => {
		const p = R.join(' ', R.map((tt) => R.join(',', tt), t[0]))	
		csvg.append("polygon")
			.attr("points", p)
			// .style("fill", "blue")
			.style("position", "fixed")	
			.attr("class", "vor")
			.style("fill", "rgba(10, 10, 10," + t[1] / vor.length + ')')
	}, R.zip(vor, R.range(0, vor.length)))
	const easel1 = d3.easeElasticIn//d3.easeSinIn
	const easel2 = d3.easeSinOut
	const polygons = d3.select("svg").selectAll(".tri")
	const breathein = async () => polygons.each(function(d, i){
		d3.select(this).transition().ease(easel1).duration(800).attr("points", tris[i][1])
	})
	const breatheout = async () => polygons.each(function(d, i){
		d3.select(this).transition().ease(easel2).duration(1200).attr("points", tris[i][0])	
	})
	function sleep(ms) {
  		return new Promise(resolve => setTimeout(resolve, ms));
	}
	const breathe = async () => {
		breathein()
		sleep(800).then(breatheout)
	}
	breathe()
	setInterval(breathe, 2000);
</script>
