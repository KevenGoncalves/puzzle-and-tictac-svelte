<script lang="ts">
	export let value: string | number = "";
	export let verify = false;
	export let adjacent = false;
	export let iaWin = false;
</script>

<div class={"square " + (verify ? "colored" : "") + " " + (adjacent ? "adjacent" : "") + " " + (iaWin ? "aiWin" : "")}>
	{value}
</div>

<style>
	.square {
		width: 200px;
		height: 200px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 1px solid black;
		font-size: 5rem;
	}

	.colored {
		background-color: indigo;
		color: white;
	}

	.adjacent {
		background-color: crimson;
		color: white;
	}

	.aiWin {
		background-color: royalblue;
		color: white;
	}

	@media screen and (min-width: 1370px) {
		.square {
			width: 199px;
			height: 199px;
		}
	}

	@media screen and (min-width: 1500px) {
		.square {
			width: 198px;
			height: 198px;
		}
	}
</style>
