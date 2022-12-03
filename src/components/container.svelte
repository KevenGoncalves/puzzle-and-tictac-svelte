<div class="container">
	<slot />
</div>

<style>
	.container {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		border: 2px solid transparent;
		border-radius: 0.5rem;
		width: 606px;
		height: 606px;
		display: flex;
		flex-wrap: wrap;
	}
</style>
