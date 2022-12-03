<script>
	import Container from "../components/container.svelte";
</script>

<Container>
	<div class="center-div">
		<span> Escolha o Jogo </span>
		<div class="buttons">
			<a href="/tic-tac-toe">Tic-Tac-Toe</a>
			<a href="/puzzle">Puzzle</a>
		</div>
	</div>
</Container>

<style>
	.center-div {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 2rem;
		flex-direction: column;
		gap: 20px;
	}

	.buttons > a {
		text-decoration: none;
		background-color: crimson;
		color: white;
		padding: 10px;
		border-radius: 10px;
		border: 3px solid transparent;
	}

	.buttons > a:hover {
		background-color: rgb(174, 6, 40);
	}
	.buttons > a:focus {
		border-color: black;
	}
</style>
