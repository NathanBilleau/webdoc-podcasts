<script>
	import Nav from "../components/Nav.svelte";
	import Progress from "../components/Progress.svelte";
	import { afterUpdate } from "svelte";
	import { currentTime, start } from "../stores/voice";

	let prev = "/";
	let next = "/";

	let routes = [
		"/",
		"/introduction",
		"/histoire",
		"/chiffres",
		"/creation",
		"/diffusion",
		"/cible",
		"/modele-economique",
		"/maisons-de-production",
		"/les-podcasts-independants",
		"/les-podcasts-produits",
		"/avis-personnel",
		"/credits",
	];
	let i;

	// set the currentime to the starting time for the page
	start.subscribe((s) => {
		$currentTime = s;
	});

	const toggleFullscreen = (e) => {
		var element = document.body;
		if (e instanceof HTMLElement) {
			element = e;
		}

		var isFullscreen =
			document.webkitIsFullScreen || document.mozFullScreen || false;

		element.requestFullScreen =
			element.requestFullScreen ||
			element.webkitRequestFullScreen ||
			element.mozRequestFullScreen ||
			function () {
				return false;
			};
		document.cancelFullScreen =
			document.cancelFullScreen ||
			document.webkitCancelFullScreen ||
			document.mozCancelFullScreen ||
			function () {
				return false;
			};

		isFullscreen
			? document.cancelFullScreen()
			: element.requestFullScreen();
	};

	afterUpdate(() => {
		let currentRoute = window.location.pathname;
		i = routes.indexOf(currentRoute);

		prev = routes[i - 1];
		next = routes[i + 1];

		// animation display
		const timingEls = document.querySelectorAll("[data-time]");

		// on/off
		if (true) {
			for (const el of timingEls) {
				el.style.opacity = 0;
				el.style.transform = `translateY(10px)`;
				el.style.transition = ".8s cubic-bezier(.5, 0, .5, 1)";
			}

			currentTime.subscribe((ct) => {
				for (const el of timingEls) {
					const attrTime = el.getAttribute("data-time");
					if (ct >= attrTime) {
						el.classList.add("display");
					}
				}
			});
		}
	});
</script>

<main>
	<div class="container">
		<button
			class="toggleFullscreen"
			on:click={toggleFullscreen}
			title="Passer en plein écran"
		>
			<i class="fa fa-expand" />
		</button>
		<Progress totalRoutes={routes.length} {i} />
		<div class="content">
			<slot />
		</div>
		<Nav {prev} {next} />
	</div>
</main>

<style>
	main {
		background-image: url("/convertkit--CbLJAUI_js-unsplash.jpg");
		background-size: cover;
		background-position: center;
		background-blend-mode: multiply;
		height: 100vh;
		background-color: #333;
	}

	.container {
		padding: 0 clamp(100px, 9vw, 200px);
		display: grid;
		/* grid-template-rows: auto clamp(120px, 12vh, 200px); */
		grid-template-rows: 10px auto clamp(120px, 12vh, 200px);
		height: 100%;
	}

	.content {
		position: relative;
		overflow-y: auto;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.toggleFullscreen {
		position: absolute;
		font-size: 1.2em;
		color: #fff;
		background-color: transparent;
		border: none;
		z-index: 999;
		right: clamp(100px, 9vw, 200px);
		top: 20px;
		cursor: pointer;
	}
</style>
