<script>
	import { slide } from 'svelte/transition';
	import Currency from './Currency.svelte';
	import Modal from './Modal.svelte';
	let { cartItems = $bindable(), totalQuantity, removeItem } = $props();
	let openModal = $state(false);
</script>

<div
	class="container mt-4 flex h-auto w-full flex-col gap-5 rounded-xl bg-white px-8 py-4 md:px-8 lg:m-2 lg:mt-14"
>
	{#if cartItems.length > 0}
		<h1 class="text-red text-2xl font-bold">Your cart ({totalQuantity.cartQuantity})</h1>

		{#each cartItems as item (item.id)}
			<div
				in:slide={{ y: -120, duration: 280 }}
				out:slide={{ y: -420, duration: 200 }}
				class="cart-item relative grid grid-cols-[80%_20%] grid-rows-2 items-center justify-between"
			>
				<div class="col-span-1 row-span-1">
					<p class="text-md text-rose900 font-semibold opacity-70">{item.product}</p>
				</div>
				<div onclick={() => removeItem(item.id)} class="col-span-1 row-span-2">
					<img
						class="remove-icon"
						src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-remove-item.svg"
						alt=""
					/>
				</div>
				<div class="col-span-1 row-span-1">
					<div class="baseline flex flex-row items-center justify-start gap-1">
						<p class="text-red text-[0.95rem] font-semibold">{item.quantity}x</p>
						<p class="text-rose900 flex flex-row gap-1 text-sm font-light opacity-90">
							@ <Currency price={item.price} />
						</p>
						<p class="text-rose900 ml-2 text-[0.98rem] font-bold">
							${(item.quantity * item.price).toFixed(2)}
						</p>
					</div>
				</div>
			</div>
		{/each}
		<div class="order-total flex flex-row items-end justify-between">
			<p class="text-md text-rose900 font-light opacity-80">Order Total</p>
			<p class="text-rose900 mt-2 text-xl font-bold">
				${totalQuantity.totalAmt}{totalQuantity.totalAmt - Math.floor(totalQuantity.totalAmt) === 0
					? '.00'
					: '0'}
			</p>
		</div>

		<div class="bg-rose50 flex w-full flex-row items-center justify-center gap-3 rounded-lg py-3">
			<div class="tree-image color-green">
				<img
					src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-carbon-neutral.svg"
					alt="ecotree"
				/>
			</div>
			<p class="text-rose900 text-center text-sm font-normal opacity-90">
				This is a <strong class="font-bold">carbon-neutral </strong> delivery
			</p>
		</div>
		<button
			onclick={() => (openModal = !openModal)}
			class="bg-red flex w-full items-center justify-center rounded-full py-3 hover:opacity-90"
		>
			<p class="text-center font-medium tracking-wide text-white">Confirm Order</p>
		</button>
	{:else}
		<h1 class="text-red text-2xl font-bold">Your cart is empty</h1>
		<div class="image flex justify-center">
			<img
				src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/illustration-empty-cart.svg"
				alt="empty cart icon"
			/>
		</div>
		<p class="text-rose500 text-center text-sm font-semibold opacity-90">
			Your added items will appear here.
		</p>
	{/if}
</div>

{#if openModal}
	<Modal bind:cartItems bind:openModal {totalQuantity} />
{/if}

<style>
	.remove-icon {
		height: 1.2rem;
		margin: 2px;
		padding: 3px;
		border-radius: 100%;
		border: 1px solid rgba(0, 0, 0, 0.3);
		display: flex;
		justify-self: flex-end;
		cursor: pointer;
		&:hover {
			border: 1px solid rgba(0, 0, 0, 0.5);
			background-color: rgba(0, 0, 0, 0.05);
		}
	}
	.cart-item::after {
		content: '';
		position: absolute;
		width: 99%;
		height: 1px;
		opacity: 14%;
		bottom: -22%;
		left: 0%;
		background-color: rgba(0, 0, 0, 0.8);
	}
</style>
