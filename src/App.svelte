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

	let mode = "Menu";

	const changeMenu = () => {
		mode = "Menu";
	};

	const changeIngredient = () => {
		mode = "Ingredient";
	};

	let open = false;
	let menu = [];
	let menuLoading = false;
	let menuName = "";
	let ingredientList = [];
	async function openRecipe(recipe) {
		if (mode === "Ingredient") {
			ingredientList = [];
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
			ingredientList.push(
				{ i: menu[0].strIngredient1, m: menu[0].strMeasure1 },
				{ i: menu[0].strIngredient2, m: menu[0].strMeasure2 },
				{ i: menu[0].strIngredient3, m: menu[0].strMeasure3 },
				{ i: menu[0].strIngredient4, m: menu[0].strMeasure4 },
				{ i: menu[0].strIngredient5, m: menu[0].strMeasure5 },
				{ i: menu[0].strIngredient6, m: menu[0].strMeasure6 },
				{ i: menu[0].strIngredient7, m: menu[0].strMeasure7 },
				{ i: menu[0].strIngredient8, m: menu[0].strMeasure8 },
				{ i: menu[0].strIngredient9, m: menu[0].strMeasure9 },
				{ i: menu[0].strIngredient10, m: menu[0].strMeasure10 },
				{ i: menu[0].strIngredient11, m: menu[0].strMeasure11 },
				{ i: menu[0].strIngredient12, m: menu[0].strMeasure12 },
				{ i: menu[0].strIngredient13, m: menu[0].strMeasure13 },
				{ i: menu[0].strIngredient14, m: menu[0].strMeasure14 },
				{ i: menu[0].strIngredient15, m: menu[0].strMeasure15 },
				{ i: menu[0].strIngredient16, m: menu[0].strMeasure16 },
				{ i: menu[0].strIngredient17, m: menu[0].strMeasure17 },
				{ i: menu[0].strIngredient18, m: menu[0].strMeasure18 },
				{ i: menu[0].strIngredient19, m: menu[0].strMeasure19 },
				{ i: menu[0].strIngredient20, m: menu[0].strMeasure20 }
			);
		} else if (mode === "Menu") {
			ingredientList = [];
			menuName = "";
			menuLoading = true;
			open = true;
			menu = recipe;
			menuLoading = false;
			menuName = menu.strMeal;
			ingredientList.push(
				{ i: menu.strIngredient1, m: menu.strMeasure1 },
				{ i: menu.strIngredient2, m: menu.strMeasure2 },
				{ i: menu.strIngredient3, m: menu.strMeasure3 },
				{ i: menu.strIngredient4, m: menu.strMeasure4 },
				{ i: menu.strIngredient5, m: menu.strMeasure5 },
				{ i: menu.strIngredient6, m: menu.strMeasure6 },
				{ i: menu.strIngredient7, m: menu.strMeasure7 },
				{ i: menu.strIngredient8, m: menu.strMeasure8 },
				{ i: menu.strIngredient9, m: menu.strMeasure9 },
				{ i: menu.strIngredient10, m: menu.strMeasure10 },
				{ i: menu.strIngredient11, m: menu.strMeasure11 },
				{ i: menu.strIngredient12, m: menu.strMeasure12 },
				{ i: menu.strIngredient13, m: menu.strMeasure13 },
				{ i: menu.strIngredient14, m: menu.strMeasure14 },
				{ i: menu.strIngredient15, m: menu.strMeasure15 },
				{ i: menu.strIngredient16, m: menu.strMeasure16 },
				{ i: menu.strIngredient17, m: menu.strMeasure17 },
				{ i: menu.strIngredient18, m: menu.strMeasure18 },
				{ i: menu.strIngredient19, m: menu.strMeasure19 },
				{ i: menu.strIngredient20, m: menu.strMeasure20 }
			);
		}
	}

	const toggle = () => (open = !open);

	let search = "";
	let searchName = "";
	let notFound = "";
	let loading = false;
	let recipes = [];

	//Fetching Data
	async function searchRecipe() {
		if (mode === "Ingredient") {
			if (search === "") {
				alert(
					"Please input the ingredients of the food that you want to find."
				);
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
		} else if (mode === "Menu") {
			if (search === "") {
				alert("Please input the name of the food that you want to find.");
			} else {
				loading = true;
				const response = await fetch(
					`https://www.themealdb.com/api/json/v1/1/search.php?s=${search}`
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
	}
</script>

<main>
	<h2 class="text-primary mb-5">Recipe App</h2>
	<form on:submit|preventDefault={searchRecipe} class="search-input" action="">
		<input
			type="text"
			placeholder="Write the {mode.toLowerCase()} ..."
			bind:value={search}
		/>
		<Button color="primary" type="submit">Search</Button>
	</form>

	<div class="dropdown">
		<h5 class="mx-2">Search by :</h5>
		<Dropdown>
			<DropdownToggle color="primary" outline caret>{mode}</DropdownToggle>
			<DropdownMenu>
				<DropdownItem header class="btn text-left" on:click={changeMenu}
					>Menu</DropdownItem
				>
				<DropdownItem header class="btn text-left" on:click={changeIngredient}
					>Ingredient</DropdownItem
				>
			</DropdownMenu>
		</Dropdown>
	</div>

	{#if loading}
		<Spinner class="my-5" color="primary" />
	{/if}
	{#if !loading}
		<div>
			{#if recipes}
				{#if searchName && mode === "Ingredient"}
					<h5 class="my-5">Displays foods made from "{searchName}"</h5>
				{/if}
				{#if searchName && mode === "Menu"}
					<h5 class="my-5">Displays foods named "{searchName}"</h5>
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
									<h6 class="card-title">{recipe.strMeal}</h6>
								</Card>
							</div>
						</Col>
					{/each}
				</Row>
			{/if}
			{#if !recipes && mode === "Ingredient"}
				<h5 class="my-5">
					Food made from "{notFound}" was not found.
				</h5>
			{/if}
			{#if !recipes && mode === "Menu"}
				<h5 class="my-5">
					Food named "{notFound}" was not found.
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
			{#if !menuLoading && mode === "Ingredient"}
				<img
					src={menu[0].strMealThumb}
					alt={menu[0].strMeal}
					class="img-thumbnail rounded mx-auto d-block my-2"
				/>
				<p class="d-block text-center font-weight-bold">Category :</p>
				<p class="ingredients text-center">{menu[0].strCategory}</p>
				<p class="d-block text-center font-weight-bold">Ingredients :</p>
				<ul>
					{#each ingredientList as list}
						{#if list.i != "" && list.i != null}
							<li>{list.i} ({list.m})</li>
						{/if}
					{/each}
				</ul>
				<p class="d-block text-center font-weight-bold">Instructions :</p>
				<p class="ingredients">{menu[0].strInstructions}</p>
				<a href={menu[0].strYoutube} class="youtube" target="_blank"
					>Watch Video</a
				>
			{/if}
			{#if !menuLoading && mode === "Menu"}
				<img
					src={menu.strMealThumb}
					alt={menu.strMeal}
					class="img-thumbnail rounded mx-auto d-block my-2"
				/>
				<p class="d-block text-center font-weight-bold">Category :</p>
				<p class="ingredients text-center">{menu.strCategory}</p>
				<p class="d-block text-center font-weight-bold">Ingredients :</p>
				<ul>
					{#each ingredientList as list}
						{#if list.i != "" && list.i != null}
							<li>{list.i} ({list.m})</li>
						{/if}
					{/each}
				</ul>
				<p class="d-block text-center font-weight-bold">Instructions :</p>
				<p class="ingredients">{menu.strInstructions}</p>
				<a href={menu.strYoutube} class="youtube" target="_blank">Watch Video</a
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

	.dropdown {
		margin-top: 55px;
		display: flex;
		justify-content: center;
		align-items: center;
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
