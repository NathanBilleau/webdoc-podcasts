<script>
	import { end, start } from "../stores/voice";
	import { onMount } from "svelte";

	import floodcast from "../images/floodcast.jpg";
	$start = 60 + 38.7;
	$end = 60 * 2 + 7;

	let episode;
	let paused = true;
	let currentTime = 0;
	let duration = 0;
	let playbackRate = 1;

	onMount(() => {
		episode = document.querySelector("audio#episode");

		episode.addEventListener("canplay", (e) => {
			duration = episode.duration;
		});
	});

	const togglePlayPause = () => {
		if (episode.paused) {
			episode.play();
			paused = false;
		} else {
			episode.pause();
			paused = true;
		}

		paused = paused;
	};

	const formatTime = (seconds) => {
		var date = new Date(0);
		date.setSeconds(seconds); // specify value for SECONDS here
		var timeString = date.toISOString().substr(11, 8);

		return timeString;
	};

	const progress = (seconds) => {
		currentTime += seconds;
	};

	const speed = () => {
		if (episode.playbackRate === 1) {
			playbackRate = 1.5;
		} else if (episode.playbackRate === 1.5) {
			playbackRate = 2;
		} else if (episode.playbackRate === 2) {
			playbackRate = 0.5;
		} else {
			playbackRate = 1;
		}
		episode.playbackRate = playbackRate;
		playbackRate = playbackRate;
	};
</script>

<svelte:head>
	<title>Player Spotify</title>
</svelte:head>

<h1>À vous de tester maintenant</h1>

<div class="iphone off">
	<audio src="/lepuyduflood.mp3" bind:currentTime id="episode" />
	<div class="flexrow">
		<i class="fa fa-angle-down" />
		<span class="name">FloodCast</span>
		<i class="fas fa-ellipsis-h" />
	</div>
	<div class="player">
		<img class="cover" src={floodcast} alt="" />
		<span class="episode"> S05E26 - Le Puy du Flood</span>
		<span class="name">FloodCast</span>
		<input
			type="range"
			class="progress"
			bind:value={currentTime}
			max={duration}
		/>
		<div class="flexrow progresstime">
			<span>{formatTime(currentTime)}</span>
			<span>{formatTime(duration)}</span>
		</div>
		<div class="flexrow buttons">
			<button on:click={speed}>
				<span>{playbackRate}x</span>
			</button>
			<button on:click={() => progress(-15)}>
				<i class="fas fa-undo-alt" />
			</button>
			<button class="playBtn" on:click={togglePlayPause}>
				<i class="fas fa-{paused ? 'play' : 'pause'}-circle" />
			</button>
			<button on:click={() => progress(15)}>
				<i class="fas fa-redo-alt" />
			</button>
			<i class="fas fa-moon" />
		</div>
	</div>
	<div class="flexrow">
		<i class="fas fa-laptop-house" />
		<i class="far fa-share-square" />
	</div>
</div>

<style>
	.iphone {
		width: 28vh;
		height: calc(28vh * 2.164);
		background: linear-gradient(#82949f, #465055);
		border-radius: 25px;
		position: relative;
		display: flex;
		justify-content: space-between;
		align-items: center;
		flex-direction: column;
		padding: 20px 15px;
		box-sizing: border-box;
		border: 2px solid #000;
		font-family: "Nunito Sans";
		color: #fff;
		margin: auto;
		box-shadow: 0 0 0 3px #254c70;
	}

	.iphone::before {
		content: "";
		height: 4%;
		width: 55%;
		border-radius: 10px;
		border-top-left-radius: 0;
		border-top-right-radius: 0;
		background-color: #000;
		display: block;
		top: 0;
		position: absolute;
		left: 22.5%;
	}

	.iphone::after {
		content: "";
		height: 2px;
		width: 40%;
		border-radius: 10px;
		background-color: rgba(255, 255, 255, 0.8);
		display: block;
		bottom: 5px;
		position: absolute;
		left: 30%;
	}

	.iphone .flexrow {
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 100%;
		font-size: 0.8rem;
	}

	.iphone .flexrow .name {
		text-align: center;
		font-weight: 300;
		font-size: 0.5rem;
	}

	.iphone .flexrow i {
		opacity: 0.7;
	}

	.iphone .buttons {
		padding-top: 20px;
	}

	.iphone .player button {
		background: none;
		border: none;
		outline: none;
		cursor: pointer;
		color: #fff;
	}

	.iphone .flexrow.buttons {
		display: grid;
		grid-template-columns: 1fr 1fr 2fr 1fr 1fr;
		justify-content: center;
		align-items: center;
		text-align: center;
	}

	.iphone .player .playBtn {
		font-size: 2.5rem;
	}

	.iphone .player .playBtn i {
		opacity: 1;
	}

	.iphone .player .cover {
		width: 100%;
		border-radius: 7px;
	}

	.iphone .player .episode {
		display: block;
		font-weight: 700;
		font-size: 0.8rem;
	}

	.iphone .player .progresstime {
		font-size: 0.5rem;
		opacity: 0.8;
	}

	.iphone .player .name {
		width: 100%;
		border-radius: 5px;
		font-weight: 300;
		font-size: 0.65rem;
		opacity: 0.6;
	}

	.iphone .player .progress {
		height: 3px;
		width: 100%;
	}
</style>
