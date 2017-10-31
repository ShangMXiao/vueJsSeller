<template>
	<transition name="move">
    	<div v-show="showflag" class="food" ref="food">
    		<div class="food-content">
    			<div class="image-header">
    				<img :src="food.image">
    				<div class="back" @click="hidden">
    					<i class="icon-arrow_lift"></i>
    				</div>
    			</div>
    			<div class="content">
    				<div class="title">{{food.name}}</div>
    				<div class="detail">
    					<span class="sell-count">月售{{food.sellCount}}份</span>
    					<div class="rating">好评率{{food.rating}}</div>
	    				
    				</div>
    				<div class="price">
	                    <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">{{food.oldPrice}}</span>
	                </div>
                    <div class="cartcontrol-wrapper">
                    <cartcontrol :food="food" v-on:cartAdd="_drop"></cartcontrol>
                    </div>
                    <transition name="fade">
                        <div @click.stop.prevent="addFirst($event)" class="buy" v-show="!food.count || food.count===0">
                            加入购物车
                        </div>
                    </transition>
    			</div>
                <split v-show="food.info"></split>
                <div class="info" v-show="food.info">
                    <h1 class="title">商品信息</h1>
                    <p class="text">{{food.info}}</p>
                </div>
                <split></split>
                <div class="rating">
                    <div class="title">商品评价</div>
                    <ratingselect 
                        :select-Type="selectType"
                        :only-Content="onlyContent"
                        :desc="desc"
                        :ratings="food.ratings"
                        v-on:ratingTypeSelect="ratingTypeSelect"
                        v-on:contentToggle="contentToggle"></ratingselect>
                    <div class="rating-wrapper">
                        <ul v-show="food.ratings && food.ratings.length">
                           <li v-show="needShow(rating.rateType, rating.text)" v-for="rating in food.ratings" class="rating-item border-1px">
                               <div class="user">
                                    <span class="name">{{rating.username}}</span>
                                    <img src="" alt="" class="avater" width="12" height="12" :src="rating.avatar">
                                </div>
                                <div class="time">{{rating.rateTime | formatDate}}</div>
                                <p class="text">
                                    <span :class="{'icon-thumb_up' :rating.rateType===0, 'icon-thumb_down': rating.rateType===1}"></span>
                                    {{rating.text}}
                                </p>
                           </li> 
                        </ul>
                        <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
                            暂无评价
                        </div>
                    </div>
                </div>
                
    		</div>
    	</div>
    </transition>
</template>

<script type="ecmascript-6">
	import BScroll from 'better-scroll'
	import Vue from 'Vue'

	import cartcontrol from '../cartControl/cartControl.vue'
    import split from '../split/split.vue'
    import ratingselect from '../ratingSelect/ratingSelect.vue'

    import { formatDate } from '../../common/js/data.js'

    const POSITIVE = 0
    const NEGATIVE = 1
    const ALL = 2

    export default {
    	props: {
    		food: {
    			type: Object
    		}
    	},
    	data() {
    		return {
    			showflag: false,
                selectType: ALL,
                onlyContent: false,
                desc: {
                    all: '全部',
                    positive: '推荐',
                    negative: '不推荐'
                }
    		}
    	},
    	methods: {
    		show() {
    			this.showflag = true
                this.selectType = ALL
                this.onlyContent = false
    			this.$nextTick(() => {
    				if (!this.scroll) {
    					this.scroll = new BScroll(this.$refs.food, {
    						click: true
    					})
    				}else {
    					this.scroll.refresh()
    				}
    			})
    		},
    		hidden() {
    			this.showflag = false
    		},
    		addFirst(event) {
    			if(!event._constructed) {
    				return
    			}
    			Vue.set(this.food, 'count', 1)
    		},
    		_drop(target) {
    			this.$emit('cartAdd', target)
    		},
            needShow(type, text) {
                if (this.onlyContent && !text) {
                    return false
                }
                if (this.selectType === ALL) {
                    return true
                }else {
                    return type === this.selectType
                }
            },
            ratingTypeSelect(type) {
                this.selectType = type
                this.$nextTick(() => {
                    this.scroll.refresh()
                })
            },
            contentToggle(onlyContent) {
                this.onlyContent = onlyContent
                this.$nextTick(() => {
                    this.scroll.refresh()
                })
            }
    	},
    	components: {
    		cartcontrol,
            split,
            ratingselect
    	}, 
        filters: {
            formatDate(time) {
                let date = new Date(time)
                return formatDate(date, 'yyyy-MM-dd hh:mm')
            }
        }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
    @import '../../common/sass/mixin.scss';


    .food {
    	position: fixed;
    	left: 0;
    	top: 0;
    	bottom: 48px;
    	z-index: 30;
    	width: 100%;
    	background: #fff;
    	&.move-enter-active, &.move-leave-active {
    		transition: all 0.2s linear;
    		transform: translate3d(0, 0, 0);
    	}
    	&.move-enter, &.move-leave-to {
    		transform: translate3d(-100%, 0, 0);
    	}
    	.image-header {
    		position: relative;
    		width: 100%;
    		height: 0;
    		padding-top: 100%;
    		img {
    			position: absolute;
    			top: 0;
    			left: 0;
    			width: 100%;
    			height: 100%;

    		}
    		.back {
    			position: absolute;
    			top: 10px;
    			left: 0;
    			.icon-arrow_lift {
    				display: block;
    				padding: 10px;
    				font-size: 20px;
    				color: #fff;
    			}
    		}
    	}
    	.content {
            position: relative;
            padding: 18px;
    		.title {
    			line-height: 14px;
    			margin-bottom: 8px;
    			font-size: 14px;
    			font-weight: 700;
    			color: rgb(7, 17, 27);
    		}
    		.detail {
    			margin-bottom: 18px;
    			line-height: 10px;
    			height: 10px;
    			font-size: 0;
    			.sell-count, .rating {
    				display: inline-block;
    				font-size: 10px;
    				color: rgb(147, 153, 159);
    			}
    			.sell-count {
    				margin-right: 12px;
    			}
    		}
    		.price {
				margin-top: 10px;
                font-weight: 700;
                line-height: 24px;
                .now {
                    margin-right: 8px;
                    font-size: 14px;
                    color: rgb(240, 20, 20)
                }

                .old {
                    text-decoration: line-through;
                    font-size: 10px;
                    color: rgb(147, 153, 159);
                }
            }

            .cartcontrol-wrapper {
                position: absolute;
                right: 12px;
                bottom: 12px;
            }
            .buy {
                position: absolute;
                bottom: 18px;
                right: 18px;
                z-index: 10;
                height: 24px;
                line-height: 24px;
                padding: 0 12px;
                box-sizing: border-box;
                border-radius: 12px;
                font-size: 10px;
                color: #fff;
                background: rgb(0, 160, 220);
                &.fade-enter-active, &.fade-leave-active {
                    transition: all 0.5s;
                    opacity: 1;
                }
                &.fade-enter, &.fade-leave-to {
                    opacity: 0;
                }
            }
    	}
        .info {
            padding: 18px;
            .title {
                line-height: 14px;
                margin-bottom: 6px;
                font-size: 14px;
                color: rgb(77, 17, 27);
            }
            .text {
                line-height: 24px;
                padding: 0 8px;
                font-size: 12px;
                color: rgb(77, 85, 93);
            }
        }
        .rating {
            padding-top: 18px;
            .title {
                line-height: 14px;
                margin-left: 6px;
                font-size: 14px;
                color: rgb(77, 17, 27);
            }
            .rating-wrapper {
                padding: 0 18px;
                .rating-item {
                    position: relative;
                    padding: 16px 0;
                    @include border-1px(rgba(7, 17, 27, 0.1));

                    .user {
                        position: absolute;
                        right: 0;
                        top: 16px;
                        line-height: 12px;
                        font-size: 0;
                        .name {
                            display: inline-block;
                            vertical-align: top;
                            font-size: 10px;
                            color: rgb(147, 153, 159);
                            margin-right: 6px;
                        }
                        .avater {
                            border-radius: 50%;
                        }
                    }

                    .time {
                        margin-bottom: 6px;
                        line-height: 12px;
                        font-size: 10px;
                        color: rgb(147, 153, 159);
                    }

                    .text {
                        line-height: 16px;
                        font-size: 12px;
                        color: rgb(7, 17, 27);

                        .icon-thumb_up, .icon-thumb_down {
                            margin-right: 4px;
                            line-height: 16px;
                            font-size: 12px;
                        }
                        .icon-thumb_up {
                            color: rgb(0, 160, 220);
                        }
                        .icon-thumb_down {
                            color: rgb(147, 153, 159);
                        }
                    }
                }
                .no-rating {
                    padding: 16px 0;
                    font-size: 12px;
                    color: rgb(147, 153, 159);
                }
            }
        }
    }
</style>