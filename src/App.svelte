<script>
	import { Button } from "sveltestrap";
	import { Spinner } from "sveltestrap";
	import { Card } from "sveltestrap";
	import { Modal } from "sveltestrap";
	import { Col, Container, Row } from "sveltestrap";
	import {
		Dropdown,
		DropdownItem,
		DropdownMenu,
		DropdownToggle,
	} from "sveltestrap";

	// let menu = "Menu";

	// const changeMenu = () => {
	// 	menu = "Menu";
	// };

	// const changeIngridient = () => {
	// 	menu = "Ingredients";
	// };

	let open = false;
	let menu = [];
	let menuLoading = false;
	let menuName = "";
	async function openRecipe(recipe) {
		menuName = "";
		menuLoading = true;
		open = true;
		const id = recipe.idMeal;
		const response = await fetch(
			`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${id}`
		);
		const hasil = await response.json();
		menu = hasil.meals;
		menuLoading = false;
		menuName = menu[0].strMeal;
	}

	const toggle = () => (open = !open);

	let search = "";
	let searchName = "";
	let notFound = "";
	let loading = false;
	let recipes = [];

	//Fetching Data
	async function searchRecipe() {
		if (search === "") {
			alert("Please input the ingredients of the food that you want to find.");
		} else {
			loading = true;
			const response = await fetch(
				`https://www.themealdb.com/api/json/v1/1/filter.php?i=${search}`
			);
			const hasil = await response.json();
			recipes = hasil.meals;
			loading = false;
			searchName = search;
			if (!recipes) {
				notFound = search;
			}
			search = "";
		}
	}
</script>

<main>
	<h2 class="text-primary mb-5">Find Meals for Your Ingredients</h2>
	<form on:submit|preventDefault={searchRecipe} class="search-input" action="">
		<input type="text" placeholder="Write the ingredient" bind:value={search} />
		<Button color="primary" type="submit">Search</Button>
	</form>

	<!-- <div class="dropdown">
		<h4 class="mx-2">Search by :</h4>
		<Dropdown>
			<DropdownToggle color="primary" outline caret>{menu}</DropdownToggle>
			<DropdownMenu>
				<DropdownItem header class="btn text-left" on:click={changeMenu}
					>Menu</DropdownItem
				>
				<DropdownItem header class="btn text-left" on:click={changeIngridient}
					>Ingrdients</DropdownItem
				>
			</DropdownMenu>
		</Dropdown>
	</div> -->

	{#if loading}
		<Spinner class="my-5" color="primary" />
	{/if}
	{#if !loading}
		<div>
			{#if recipes}
				{#if searchName}
					<h5 class="my-5">Displays foods made from "{searchName}"</h5>
				{/if}
				<Row class="my-5">
					{#each recipes as recipe (recipe.idMeal)}
						<Col md="4" class="my-3">
							<div class="pointer" on:click={() => openRecipe(recipe)}>
								<Card body>
									<img
										src={recipe.strMealThumb}
										class="card-img-top mb-3"
										alt={recipe.strCategory}
									/>
									<h5 class="card-title">{recipe.strMeal}</h5>
								</Card>
							</div>
						</Col>
					{/each}
				</Row>
			{/if}
			{#if !recipes}
				<h5 class="my-5">
					Food made from "{notFound}" was not found.
				</h5>
			{/if}
		</div>
	{/if}
	<div>
		<Modal body header={menuName} isOpen={open} {toggle}>
			{#if menuLoading}
				<div class="center">
					<Spinner class="my-5 mx-auto" color="primary" />
				</div>
			{/if}
			{#if !menuLoading}
				<img
					src={menu[0].strMealThumb}
					alt={menu[0].strMeal}
					class="img-thumbnail rounded mx-auto d-block my-2"
				/>
				<p class="d-block text-center font-weight-bold">Category :</p>
				<p class="ingredients text-center">{menu[0].strCategory}</p>
				<p class="d-block text-center font-weight-bold">Instructions :</p>
				<p class="ingredients">{menu[0].strInstructions}</p>
				<a href={menu[0].strYoutube} class="youtube" target="_blank"
					>Watch Video</a
				>
			{/if}
		</Modal>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	.youtube {
		margin-top: 40px;
		display: block;
		text-align: center;
	}
	.center {
		display: flex;
		width: 100%;
		height: 100%;
		justify-content: center;
		align-items: center;
	}

	.search-input {
		margin-top: 30px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	input {
		width: 400px;
		margin-right: 10px;
	}

	.pointer {
		cursor: pointer;
	}

	/* .dropdown {
		margin: 50px;
		display: flex;
		justify-content: center;
		align-items: center;
	} */

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
