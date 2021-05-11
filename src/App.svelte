<script>
	import { Button } from "sveltestrap";
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
	import data from "./mockData";

	let recipes = data.results;
	let menu = "Menu";

	const changeMenu = () => {
		menu = "Menu";
	};

	const changeIngridient = () => {
		menu = "Ingredients";
	};

	let open = false;
	let modalTitle = "";
	let modalIngredients = "";
	let modalImage = "";
	let modalLink = "";
	const openRecipe = (recipe) => {
		open = true;
		modalTitle = recipe.title;
		modalIngredients = recipe.ingredients;
		modalImage = recipe.thumbnail;
		modalLink = recipe.href;
	};

	const toggle = () => (open = !open);

	let search = "";
	let loading = false;
	let page = 1;
	//Fetching Data
	async function searchRecipe(e) {
		loading = true;
		const response = await fetch(
			"http://www.recipepuppy.com/api/?i=onions,garlic&q=omelet&p=3"
		);
		const hasil = await response.json();
		console.log(response);
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
	<div>
		<Row class="my-5">
			{#each recipes as recipe (recipe.href)}
				<Col xs="4" class="my-3">
					<div class="pointer" on:click={() => openRecipe(recipe)}>
						<Card body>
							<img
								src={recipe.thumbnail}
								class="card-img-top mb-3"
								alt={recipe.title}
							/>
							<h5 class="card-title">{recipe.title}</h5>
						</Card>
					</div>
				</Col>
			{/each}
		</Row>
	</div>

	<div class="d-flex justify-content-between">
		{#if page > 1}
			<Button color="primary" on:click={() => page--}>Previous Page</Button>
		{/if}
		<Button color="primary" on:click={() => page++} class="ml-auto"
			>Next Page</Button
		>
	</div>
	<div>
		<Modal body header={modalTitle} isOpen={open} {toggle}>
			<img
				src={modalImage}
				alt={modalTitle}
				class="img-thumbnail rounded mx-auto d-block my-2"
			/>
			<p class="d-block text-center font-weight-bold mt-5">Ingredients :</p>
			<p class="ingredients text-center">{modalIngredients}</p>
			<a href={modalLink} target="_blank" class="link text-center d-block"
				>View recipe</a
			>
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
