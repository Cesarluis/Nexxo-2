<script>
    import { prevent_default } from "svelte/internal";
	import {v4} from "uuid";
//	import Noty from "noty";

	// import "noty/lib/noty.css";
	// import "noty/lib/themes/sunset.css"
	
	let products = [
	{	
		id: 1,
		name: "Toshiba lsn",
		description: "Laptop de Toshiba",
		category: "laptop"
	},
	{	
		id: 2,
		name: "Racer Mouse",
		description: "Game mouse",
		category: "peripherials"
	},
	
	]
	let product = {
		id: "",
		name: "",
		description: "",
		imageUrl: "",
		category: "",
	};

	let editStatus = false;

	const cleanProduct = () => {
		product = {
			id:"",
			name:"",
			description:"",
			category:"",
			imageUrl:""
		}
	}
	const deleteProduct = (id) => {
		products = products.filter(product => product.id != id)
	}
	const editProduct = (productEdited) => {
		product = productEdited;
		editStatus = true;
	}
	const addProduct = () => {
		
		const newProduct = {
			id: v4(),
			name: product.name,
			description: product.description,
			category: product.category,
			imageUrl: product.imageUrl
		}
		products = products.concat(newProduct)
		cleanProduct()
	}
	const updateProduct = () => {
		let updatedProduct = {
			name: product.name,
			description: product.description,
			id: product.id,
			imageUrl: product.imageUrl,
			category: product.category
		};
		const productIndex = products.findIndex(p => p.id === product.id);
		products[productIndex] = updatedProduct;
		cleanProduct();
		editStatus = false;
		// new Noty ({
		// 	theme: "sunset",
		// 	type: "success",
		// 	timeout: 3000,
		// 	text: "Product update successfully",
		// }).show();
	}
	const onSubmitHandler = (e) => {
		if(!editStatus){
			addProduct()
		}else{
			updateProduct()
		}

	};
</script>

<main>
	<div class="container">
		<div class="row">
			<div class="col-md-6">
				{#each products as product }
					<div class="card mt-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageUrl}
									<img src="images/no-products-found.png" alt="" class="img-fluid p-2">
								{:else}
									<img src="{product.imageUrl}" alt="" class="img-fluid p-2">
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5>
										<strong>{product.name}</strong>
										<span><small>{product.category}</small></span>
									</h5>
									<p class="card-text">{product.description}</p>
									<button on:click={deleteProduct(product.id)} class="btn btn-danger">
										Delete
									</button>
									<button on:click={editProduct(product)} class="btn btn-secondary">
										Edit
									</button>
								</div>
							</div>
						</div>
					</div>
					
				{/each}
			</div>
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<div class="form-group">
								<input
								class="form-control"
								bind:value={product.name}
								type="text"
								placeholder="Product name"
								id="product-name"
							/>
							</div>
							<div class="form-group">
								<textarea
								class="form-control"
								bind:value={product.description}
								id="product-description"
								rows="3"
								placeholder="product description"
							/>
							</div>
							<div class="form-group">
								<input
								class="form-control"
								bind:value={product.imageUrl}
								type="url"
								name="product-image-url"
								placeholder="product image"
								id=""
							/>
							</div>
							<div class="form-group">
								<select
								class="form-control"
								id="category"
								bind:value={product.category}
							>
								<option value="laptops">Laptops</option>
								<option value="peripherials"
									>Peripherials</option
								>
								<option value="servers">Servers</option>
							</select>
							</div>
							<button class="btn btn-secondary"> 
								{#if !editStatus}
									Save Product
								{:else}
									Update Status
								{/if}
								 </button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>
</style>
