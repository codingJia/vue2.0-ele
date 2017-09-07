<template>
	<div class="cartcontrol">
		<transition name="move">
			<div class="cart-decrease" v-show="food.count>0" @click.stop="decreaseCart">
				<span class="inner icon-remove_circle_outline"></span>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click.stop="addCart"></div>
	</div>
</template>
<script>
	import Vue from 'vue';
	export default {
		props: {
			food: {
				type: Object
			}
		},
		methods: {
			addCart(event) {
				if (!event._constructed) {
					return;
				}
				if (!this.food.count) {
					Vue.set(this.food, 'count', 1);
				} else {
					this.food.count++;
				}
				this.$emit('add-cart', event.target);
			},
			decreaseCart(evnet) {
				if (!event._constructed) {
					return;
				}
				if (this.food.count) {
					this.food.count--;
				}
			}
		}
	};
</script>
<style lang="less">
	.cartcontrol{
		font-size: 0;
		.cart-decrease{
			display: inline-block;
			padding: 0.12rem;
			opacity: 1;
			transform: translate3d(0,0,0);
			.inner{
				display: inline-block;
				font-size: 0.48rem;
				line-height: 0.48rem;
				color: rgb(0,160,220);
				transition: all 0.4s linear;
				transform: rotate(0);
			}
			&.move-enter, &.move-leave-active {
			  	opacity: 0;
				transform: translate3d(0.48rem,0,0);
				.inner{
					transform: rotate(180deg);
				}
			}
			&.move-enter-active, &.move-leave-active {
			  	transition: all 0.4s linear;
			}

		}
		.cart-count{
			display: inline-block;
			padding: 0.12rem;
			vertical-align: top;
			width: 0.24rem;
			padding-top: 0.12rem;
			line-height: 0.48rem;
			text-align: center;
			font-size: 0.2rem;
			color: rgb(147,153,159);
		}
		.cart-add{
			display: inline-block;
			padding: 0.12rem;
			font-size: 0.48rem;
			line-height: 0.48rem;
			color: rgb(0,160,220);		
		}
	}
</style>