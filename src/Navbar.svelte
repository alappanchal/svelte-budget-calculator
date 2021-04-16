<script>
	// your script goes here

	export let showForm;

	function addItem(){
		showForm();
	}	
</script>

<style>
	/* your styles go here */
	i{
		margin-right: 5px;;
	}
</style>

<nav class="nav">
	<div class="nav-center">
		<h1 class="nav-title">Budget Calculator</h1>
		<button type="button" class="nav-btn" on:click="{addItem}">
			<i class="far fa-plus-square"></i>Add Item
		</button>
	</div>	
</nav>