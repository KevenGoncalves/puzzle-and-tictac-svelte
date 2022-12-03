<script lang="ts">
	import Square from "../../components/square.svelte";
	import Container from "../../components/container.svelte";

	type SquaresI = {
		square: string;
	};

	//Caso Inicial
	let values: SquaresI[] = [
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
		{ square: "" },
	];
	let path: number[] = [];
	let winPath: number[] = [];
	let adjacents: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8];
	let gameStatus = true;
	let playWithAi = true;
	let playerTime = true;
	let whoWin = "";

	const restart = () => {
		values = [
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
			{ square: "" },
		];
		path = [];
		adjacents = [0, 1, 2, 3, 4, 5, 6, 7, 8];
		gameStatus = true;
		whoWin = "";
	};

	const changeMode = () => {
		playWithAi = !playWithAi;
		restart();
	};

	const changeValue = (index: number, peace: string) => {
		if (path.includes(index)) return null;
		values[index].square = peace;
		path.push(index);
		playerTime = false;
	};

	$: verifyWins = (peace: string) => {
		let wins = false;

		const attribuateWins = () => {
			wins = true;
			gameStatus = false;
			adjacents = [];
			whoWin = peace;
		};

		const winCase = (state1: number, state2: number, state3: number) => {
			if (values[state1].square == peace && values[state2].square == peace && values[state3].square == peace) {
				winPath.push(state1, state2, state3);
				attribuateWins();
			}
		};

		const possibleCases = [
			[0, 1, 2],
			[3, 4, 5],
			[6, 7, 8],
			[0, 3, 6],
			[1, 4, 7],
			[2, 5, 8],
			[0, 4, 8],
			[2, 4, 6],
		];

		possibleCases.forEach((cases) => winCase(cases[0], cases[1], cases[2]));

		return wins;
	};

	$: possibleMovies = (index: number) => {
		adjacents = adjacents.filter((numb) => numb != index);
	};

	$: AIPlay = () => {
		let val = Math.round(Math.random() * 8);

		while (path.length < 8) {
			if (!path.includes(val)) break;
			val = Math.round(Math.random() * 8);
		}

		changeValue(val, "O");
		possibleMovies(val);
		playerTime = true;
	};
</script>

<div class="controllers">
	<button on:click={restart}>Reiniciar</button>
	<button on:click={changeMode}>Jogar Com AI:{playWithAi ? "Sim" : "Não"}</button>
</div>
{#if !gameStatus}
	<span>{whoWin == "X" ? "Ganhou!" : whoWin == "O" ? "Perdeu!" : ""}</span>
{/if}
<Container>
	{#each values as item, index}
		<div
			on:click={() => {
				if (gameStatus) {
					changeValue(index, "X");
					possibleMovies(index);
					if (playWithAi && !playerTime) AIPlay();
				}
			}}
		>
			<Square
				value={item.square}
				adjacent={adjacents.includes(index)}
				verify={verifyWins("X") && winPath.slice(-4, -1).includes(index)}
				iaWin={verifyWins("O") && winPath.slice(-4, -1).includes(index) && playWithAi}
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
