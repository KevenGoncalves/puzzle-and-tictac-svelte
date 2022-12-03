<script lang="ts">
	import Container from "../../components/container.svelte";
	import Square from "../../components/square.svelte";

	type SquaresI = {
		square: string | number;
	};

	let values: SquaresI[] = [
		{ square: 1 },
		{ square: 2 },
		{ square: 3 },
		{ square: 4 },
		{ square: 5 },
		{ square: 6 },
		{ square: 7 },
		{ square: 8 },
		{ square: "" },
	];
	let fields: SquaresI[] = [];
	let gameStatus = true;
	let mode = true;

	const randomizePositions = () => {
		while (values.length != fields.length && fields.length < 9) {
			let i = Math.round(Math.random() * 8);

			if (fields.includes(values[i])) continue;

			fields.push(values[i]);
		}
	};

	const restart = () => {
		fields = [];
		gameStatus = true;
		randomizePositions();
	};

	const changeMode = () => {
		mode = !mode;
	};

	function getAround(index: number) {
		const col = 3;
		const row = 3;
		let around = [];

		const leftEdge = index % col === 0;
		const rightEdge = (index + 1) % col === 0;
		const topEdge = Math.floor(index / col) === 0;
		const bottomEdge = Math.floor(index / col) === row - 1;

		if (!leftEdge) around.push(index - 1);
		if (!rightEdge) around.push(index + 1);
		if (!topEdge) around.push(index - col);
		if (!bottomEdge) around.push(index + col);

		return around;
	}

	$: slideClick = (index: number) => {
		let change = fields[index];
		let empty = fields.findIndex((i) => i.square == "");

		if (mode) {
			if (getAround(empty).includes(index)) {
				fields[index] = fields[empty];
				fields[empty] = change;
			}
		} else {
			fields[index] = fields[empty];
			fields[empty] = change;
		}
	};

	$: verifyWin = () => {
		if (JSON.stringify(fields) == JSON.stringify(values)) {
			gameStatus = false;
		}
	};

	randomizePositions();
</script>

<div class="controllers">
	<button on:click={restart}>Reiniciar</button>
	<button on:click={changeMode}>Modo Facil:{mode ? "Não" : "Sim"}</button>
</div>
{#if !gameStatus}
	<span>Ganhou</span>
{/if}
<Container>
	{#each fields as item, index}
		<div
			on:click={() => {
				if (gameStatus) {
					slideClick(index);
					verifyWin();
				}
			}}
		>
			<Square
				value={item.square}
				adjacent={getAround(fields.findIndex((i) => i.square == "")).includes(index) && gameStatus}
				verify={!gameStatus}
			/>
		</div>
	{/each}
</Container>

<style>
	.controllers {
		display: flex;
		flex-direction: column;
		width: 180px;
		gap: 10px;
	}
	.controllers > button {
		padding: 0.75rem;
		border: 3px solid transparent;
		border-radius: 0.5rem;
		font-size: 1rem;
		color: white;
		background-color: crimson;
	}

	.controllers > button:hover {
		background-color: hsla(348, 83%, 47%, 0.837);
	}

	.controllers > button:focus {
		border: 3px solid black;
	}

	span {
		font-size: 3rem;
	}
</style>
