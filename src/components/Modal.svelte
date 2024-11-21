<script>
	import { fade, fly } from 'svelte/transition';
	import Currency from './Currency.svelte';

	let { cartItems = $bindable(), openModal = $bindable(), totalQuantity } = $props();

	function handleClick() {
		openModal = !openModal;
		cartItems = [];
	}
	let innerWidth = $state(0);
	$inspect(innerWidth);
</script>

<svelte:window bind:innerWidth />

<div class="modal-bg bg-rose900 fixed" in:fade={{ duration: 240 }} out:fade={{ duration: 200 }}>
	<div
		in:fly={{ y: 1200, duration: 320, delay: 340 }}
		out:fly={{ y: 800, duration: 440 }}
		class="modal-container s bg-rose50 container z-20 flex h-full flex-col items-center justify-center gap-4 rounded-lg p-8 opacity-100 sm:w-[374px] md:mt-0 md:w-[400px]"
	>
		<div class="check-icon flex w-full justify-start">
			<img
				src="https://raw.githubusercontent.com/35degrees/product-cart-fem/b55af8bf6f8694fb64005b3ffaab06656050622d/src/lib/images/icon-order-confirmed.svg"
				alt=""
			/>
		</div>
		<h1 class="-mt-2 flex w-full justify-start text-left text-3xl font-bold">Order Confirmed</h1>
		<p class="text-md -mt-4 flex w-full justify-start text-left font-normal opacity-80">
			We hope you enjoy your food!
		</p>
		<div class="cart-item-container bg-rose100 flex w-full flex-col gap-2 rounded-xl p-4">
			{#each cartItems as item, i}
				<div class="cart-box relative flex flex-row items-center justify-between rounded-lg p-2">
					<div class=" flex flex-row justify-start gap-4">
						<div class="cart-thumbnail">
							<img class="rounded-md shadow-xl" src={item.thumbnail} alt="thumbnail" />
						</div>
						<div class="flex flex-col items-start justify-evenly">
							<p class="text-rose900 text-[0.9rem] font-medium">{item.product}</p>
							<div class="flex flex-row gap-2">
								<div class="text-red text-[0.91rem] font-semibold">{item.quantity}x</div>
								<p
									class="baseline text-rose900 flex flex-row items-start justify-center gap-2 text-[0.91rem] opacity-80"
								>
									@ <Currency price={item.price} />
								</p>
							</div>
						</div>
					</div>
					<p class="text-rose900 mr-4 text-lg font-semibold opacity-80">
						<Currency price={item.total} />
					</p>
				</div>
			{/each}
			<div class="mt-4 flex flex-row items-center justify-between p-2 px-4">
				<h3 class="text-xl font-semibold">Total</h3>
				<p class="text-rose900 mr-2 text-lg font-bold opacity-90">
					<Currency price={totalQuantity.totalAmt} />
				</p>
			</div>
		</div>
		<button
			onclick={handleClick}
			class="bg-red mt-2 flex w-full items-center justify-center rounded-full py-3 hover:opacity-90"
		>
			<p class="text-center font-medium tracking-wide text-white">Start New Order</p>
		</button>
	</div>
</div>

<style>
	.modal-bg {
		height: 100%;
		width: 100vw;
		z-index: 25;
		top: 0;
		left: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: hsla(14, 65%, 9%, 0.941);
		overflow: hidden;
	}
	.modal-container {
		width: 500px;
		height: auto;
	}
	.cart-thumbnail {
		width: 72px;
		height: 72px;
	}
	.cart-box::after {
		content: '';
		position: absolute;
		width: 99%;
		height: 1px;
		opacity: 14%;
		bottom: -5%;
		left: 0%;
		background-color: rgba(0, 0, 0, 0.8);
	}
</style>
