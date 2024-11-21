<script>
	import { fade } from 'svelte/transition';
	import data from '/src/lib/data.json';
	import Currency from './Currency.svelte';
	import Cart from './Cart.svelte';
	import { get } from 'svelte/store';

	let cartItems = $state([]);

	let totalQuantity = $derived.by(() => {
		let totalAmt = 0;
		let cartQuantity = 0;
		for (const quantity of cartItems) {
			totalAmt += quantity.total;
			cartQuantity += quantity.quantity;
		}
		return {
			totalAmt,
			cartQuantity
		};
	});

	function addToCart(data) {
		let total = 0;
		let stop = cartItems.find((item) => item.product === data.name);
		if (!stop) {
			cartItems.push({
				product: data.name,
				id: data.number,
				quantity: 1,
				price: data.price,
				total: data.price,
				thumbnail: data.image.thumbnail
			});
		}
	}

	function decrement(i) {
		for (const object of cartItems) {
			if (object.id === i) {
				if (object.quantity === 1) {
					removeItem(object.id);
				} else {
					object.quantity--;
					object.total -= object.price;
				}
			}
		}
	}

	function increment(i) {
		for (let object of cartItems) {
			if (object.id === i) {
				object.quantity++;
				object.total += object.price;
			}
		}
	}

	function getQuantity(i) {
		for (let object of cartItems) {
			if (object.id === i) {
				return object.quantity;
			}
		}
	}

	function removeItem(id) {
		cartItems = cartItems.filter((item) => item.id !== id);
	}
</script>

<div class="container">
	<h1 class="mb-4 text-left text-4xl font-bold text-black">Desserts</h1>
	<div
		class="card-container grid-rows-auto flex flex-col items-center justify-start gap-5 md:grid md:grid-cols-3 md:gap-6"
	>
		{#each data as data, i}
			<div class="card flex h-auto w-full flex-col gap-6 md:gap-5">
				<div class="relative w-full">
					<img
						src={data.image.mobile}
						alt="menu item"
						class="rounded-xl border-2 border-transparent"
						class:add-outline={getQuantity(i)}
					/>

					{#if cartItems.find((item) => item.id === data.number)}
						<button
							in:fade={{ duration: 120 }}
							out:fade={{ duration: 120 }}
							class="adjust-btn text-md border-rose400 bg-red absolute -bottom-4 left-[50%] flex flex-row items-center justify-between gap-2 rounded-full border-[1px] px-5 py-2 text-center sm:w-[145px] md:w-3/5 md:gap-1 md:px-4"
						>
							<div class="minus-sign" onclick={() => decrement(data.number)}>
								<img
									src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-decrement-quantity.svg"
									alt="decrement"
								/>
							</div>
							<p class="wrap-none inline text-sm font-semibold text-white lg:text-[1rem]">
								{getQuantity(data.number)}
							</p>
							<div class="plus-sign" onclick={() => increment(data.number)}>
								<img
									src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-increment-quantity.svg"
									alt="increment"
								/>
							</div>
						</button>
					{:else}
						<button
							in:fade={{ duration: 120 }}
							out:fade={{ duration: 120 }}
							onclick={() => addToCart(data)}
							class="add-btn text-md border-rose400 absolute -bottom-4 left-[50%] flex flex-row items-center justify-center gap-2 rounded-full border-[1px] bg-white px-5 py-2 text-center text-black md:w-3/5 md:gap-1 md:px-4"
						>
							<img
								src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-add-to-cart.svg"
								alt="add to cart"
							/>
							<p class="wrap-none lg:text-md inline text-sm">Add to Cart</p>
						</button>
					{/if}
				</div>

				<div class="card-details flex flex-col items-start justify-start">
					<p class="text-rose500 text-sm opacity-80">{data.category}</p>
					<p class="text-md text-rose900 font-bold opacity-90">{data.name}</p>
					<p class="text-red text-md flex flex-row gap-1 font-semibold opacity-90">
						${data.price}{data.price - Math.floor(data.price) !== 0 ? '0' : '.00'}
					</p>
				</div>
			</div>
		{/each}
	</div>
</div>

<Cart bind:cartItems {totalQuantity} {removeItem} />

<style>
	.add-btn,
	.adjust-btn {
		transform: translateX(-50%);
	}
	.minus-sign,
	.plus-sign {
		padding: 5px;
		height: 20px;
		width: 20px;
		border: 1px solid #ffffff7b;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		&:hover {
			background-color: #ffffff2f;
		}
	}
	.add-outline {
		border: 2px solid hsla(14, 86%, 42%, 0.437);
	}
</style>
