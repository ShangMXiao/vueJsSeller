<template>
    <div class="cartcontrol">
    	<transition name="move">
	    	<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCount">
	    		<span class="inner icon-remove_circle_outline"></span>
	    	</div>
	    </transition>
    	<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    	<div class="cart-add icon-add_circle" @click.stop.prevent="addCount"></div>
    </div>
</template>

<script type="ecmascript-6">
	import Vue from 'vue'

    export default {
    	props: {
    		food: {
    			type: Object
    		}
    	},

    	methods: {
    		addCount(event) {
    			if (!event._constructed) {
    				return
    			}
    			if (!this.food.count) {
    				Vue.set(this.food, 'count', 1)
    			}else {
    				this.food.count++
    			}
    			this.$emit('cartAdd', event.target)
    		},
    		decreaseCount(event) {
    			if (!event._constructed) {
    				return
    			}
    			if (this.food.count) {
    				this.food.count--
    			}
    		}
    	}
    }
</script>

<style lang="scss" rel="stylesheet/scss">
    .cartcontrol {
    	font-size: 0;
    	.cart-decrease, .cart-add {
    		display: inline-block;
    		color: rgb(0, 160, 220);
    		.inner {
    			display: inline-block;
	    		padding: 6px;
	    		line-height: 24px;
	    		font-size: 24px;
	    	}
	    	&.move-enter-active, &.move-leave-active {
				transition: all 0.4s linear;
				.inner {
					transition: all 0.4s linear;
				}
	    	}
	    	&.move-enter, &.move-leave-to {
				opacity: 0;
				transform: translate3D(24px, 0, 0);
				.inner {
					transform: rotate(180deg);
				}
	    	}
    	}
    	.cart-count {
    		display: inline-block;
    		vertical-align: top;
    		width: 12px;
    		padding-top: 6px;
    		line-height: 24px;
    		font-size: 10px;
    		color: rgb(147, 153, 159);
    	}
		.cart-add {
			display: inline-block;
    		color: rgb(0, 160, 220);
    		padding: 6px;
    		line-height: 24px;
    		font-size: 24px;
		}
    }
</style>