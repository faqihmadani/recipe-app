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
	import { Form, FormGroup, FormText, Input, Label } from "sveltestrap";

	let menu = "Menu";

	const changeMenu = () => {
		menu = "Menu";
	};

	const changeIngridient = () => {
		menu = "Ingredients";
	};

	let open = false;
	let modalTitle = "";
	let modalImage = "";
	const openRecipe = (recipe) => {
		open = true;
		modalTitle = recipe.strMeal;
		modalImage = recipe.strMealThumb;
	};

	const toggle = () => (open = !open);

	let search = "";
	let notFound = "";
	let loading = false;
	let recipes = [];
	//Fetching Data
	async function searchRecipe(e) {
		loading = true;
		const response = await fetch(
			`https://www.themealdb.com/api/json/v1/1/search.php?s=${search}`
		);
		const hasil = await response.json();
		recipes = hasil.meals;
		loading = false;
		if (!recipes) {
			notFound = search;
		}
	}
</script>

<main>
	<h1 class="text-primary">Recipe App</h1>
	<div class="search-input">
		<FormGroup>
			<Input type="text" placeholder="Search recipe" bind:value={search} />
		</FormGroup>
		<Button color="primary" on:click={searchRecipe}>Search</Button>
	</div>

	<div class="dropdown">
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
	</div>

	{#if loading}
		<Spinner color="primary" />
	{/if}
	{#if !loading}
		<div>
			{#if recipes}
				<Row class="my-5">
					{#each recipes as recipe (recipe.idMeal)}
						<Col xs="4" class="my-3">
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
				<h3>{notFound} is not found.</h3>
			{/if}
		</div>
	{/if}
	<div>
		<Modal body header={modalTitle} isOpen={open} {toggle}>
			<img
				src={modalImage}
				alt={modalTitle}
				class="img-thumbnail rounded mx-auto d-block my-2"
			/>
			<p class="d-block text-center font-weight-bold mt-5">Ingredients :</p>
			<p class="ingredients text-center">oi</p>
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

	.pointer {
		cursor: pointer;
	}

	.dropdown {
		margin: 50px;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
