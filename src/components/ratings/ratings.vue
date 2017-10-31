<template>
    <div class="ratings" ref="ratings">
    	<div class="ratings-content">
    		<div class="overview">
    			<div class="overview-left">
    				<h1 class="score">{{seller.score}}</h1>
    				<div class="title">综合评分</div>
    				<div class="rank">高于周边商家{{seller.rankRate}}%</div>
    			</div>
    			<div class="overview-right">
    				<div class="score-wrapper">
    					<span class="title">服务态度</span>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
    				</div>
    				<div class="score-wrapper">
    					<span class="title">商品评分</span>
    					<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.foodScore}}</span>
    				</div>
    				<div class="delivery-wrapper">
    					<span class="title">送达时间</span>
    					<span class="deliverytime">{{seller.deliveryTime}}分钟</span>
    				</div>
    			</div>
    		</div>
    		<split></split>
    		<ratingselect 
                :select-Type="selectType"
                :only-Content="onlyContent"
                :desc="desc"
                :ratings="ratings"
                v-on:ratingTypeSelect="ratingTypeSelect"
                v-on:contentToggle="contentToggle"></ratingselect>
            <div class="rating-wrapper">
            	<ul>
            		<li v-show="needShow(rating.rateType, rating.text)" v-for="rating in ratings" class="ratingitem border-1px">
            			<div class="avatar">
            				<img width="28" height="28" :src="rating.avatar">
            			</div>
            			<div class="content">
            				<h1 class="name">{{rating.username}}</h1>
            				<div class="star-wrapper">
            					<div class="star">
            						<star :size="36" :score="rating.score" ></star>
            					</div>
								<span class="deliverytime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
            				</div>
            				<p class="text">{{rating.text}}</p>
            				<div class="recommend" v-show="rating.recommend && rating.recommend.length>0">
            					<span class="icon-thumb_up"></span>
            					<span v-for="recommendItem in rating.recommend" class="item">
            						{{recommendItem}}
            					</span>
            				</div>
            				<div class="time">
            					{{rating.rateTime | formatDate}}
            				</div>
            			</div>
            		</li>
            	</ul>
            </div>
    	</div>
    </div>
</template>

<script type="ecmascript-6">
	import star from '../../components/star/star.vue'
	import split from '../split/split.vue'
    import ratingselect from '../ratingSelect/ratingSelect.vue'

    import { formatDate } from '../../common/js/data.js'

    import BScroll from 'better-scroll'

    const POSITIVE = 0
    const NEGATIVE = 1
    const ALL = 2

    const ERR_OK = 0

    export default {
    	props: {
    		seller: {
    			type: Object
    		}
    	},
    	components: {
    		star,
    		split,
    		ratingselect
    	},
    	data() {
    		return {
    			ratings: [],
	            selectType: ALL,
	            onlyContent: false,
	            desc: {
	                all: '全部',
	                positive: '推荐',
	                negative: '不推荐'
	            }
    		}
    	},
    	created() {
    		this.$http.get('/api/ratings').then((response) => {
    			let result = response.body
    			if (result.erron === ERR_OK) {
    				this.ratings = result.data
    				this.$nextTick(() => {
    					this.scroll = new BScroll(this.$refs.ratings, {
    						click: true
    					})
    				})
    			}
    		})
    	},
    	methods: {
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
            }
    	}, 
        filters: {
            formatDate(time) {
                let date = new Date(time)
                return formatDate(date, 'yyyy-MM-dd hh:mm')
            }
        }
    }
</script>

<style lang="scss" rel="stylesheet/scss">
	@import '../../common/sass/mixin.scss';
    .ratings {
    	position: absolute;
    	top: 174px;
    	bottom: 0;
    	left: 0;
    	width: 100%;
    	overflow: hidden;

    	.overview {
    		display: flex;
    		padding: 18px 0;

    		.overview-left {
    			flex: 0 0 137px;
    			width: 137px;
    			border-right: 1px solid rgba(7, 17, 27, 0.1); 
				padding: 6px 0; 
				text-align: center;

				@media only screen and (max-width: 320px) {
					flex: 0 0 120px;
					width: 120px;
				}

				.score {
					line-height: 28px;
					font-size: 24px;
					color: rgb(255, 153, 0);
					margin-bottom: 6px; 
				}

				.title {
					line-height: 12px;
					font-size: 12px;
					color: rgb(7, 17, 27);
					margin-bottom: 6px;
				}

				.rank {
					line-height: 10px;
					font-size: 10px;
					color: rgb(147, 153, 159);
				}
    		}

    		.overview-right {
    			flex: 1;
    			padding: 6px 0 20px 24px;

    			@media only screen and (max-width: 320px) {
    				flex: 1;
    				padding: 6px 0 10px 24px;
    			}
				
				.score-wrapper {
					line-height: 18px;
					margin-bottom: 8px;
					font-size: 0;

					.title {
						display: inline-block;
						vertical-align: top;
						font-size: 12px;
						color: rgb(7, 17, 27);
					}
					.star {
						display: inline-block;
						vertical-align: top;
						margin: 0 12px;
						vertical-align: top;
						font-size: 12px;
						color: rgb(255, 153, 0);
					}
					.score {
						display: inline-block;
						line-height: 18px;
						vertical-align: top;
						font-size: 12px;
						color: rgb(255, 153, 0);
					}
				}

				.delivery-wrapper {
					font-size: 0;

					.title {
						line-height: 18px;
						font-size: 12px;
						color: rgb(7, 17, 27);
					}
					.deliverytime {
						margin-left: 12px;
						font-size: 12px;
						color: rgb(147, 153, 159);
					}

				}
    		}
    	}

    	.rating-wrapper {
    		padding: 0 18px;
    		.ratingitem {
				display: flex;
				padding: 18px 0;
				@include border-1px(rgba(7, 17, 27, 0.1));

				.avatar {
					flex: 0 0 28px;
					width: 28px;
					margin-right: 12px;
					img {
						border-radius: 50%;
					}
				}

				.content {
					position: relative;
					flex: 1;
					.name {
						line-height: 12px;
						margin-bottom: 4px;
						font-size: 10px;
						color: rgb(7, 17, 27);
					}
					.star-wrapper {
						margin-bottom: 6px;
						font-size: 0;
						.star {
							display: inline-block;
							vertical-align: middle;
							margin-right: 6px;
						}
						.deliverytime {
							display: inline-block;
							vertical-align: middle;
							text-align: center;
							font-size: 12px;
							color: gray;
						}
					}

					.text {
						line-height: 18px;
						font-size: 12px;
						margin-bottom: 8px;
						color: rgb(7, 17, 27);
					}

					.recommend {
						line-height: 16px;
						font-size: 0;
						.icon-thumb_up, .item {
							display: inline-block;
							margin: 0 8px 4px 0;
							font-size: 9px;
						}
						.icon-thumb_up {
							color: rgb(0, 160, 220);
						}
						.item {
							padding: 0 6px;
							border: 1px solid rgba(7, 17, 27, 0.1);
							border-radius: 1px;
							background: #fff;
							color: rgb(147, 153, 159);
						}
					}

					.time {
						position: absolute;
						top: 0;
						right: 0;
						line-height: 12px;
						font-size: 12px;
						color: rgb(147, 153, 159);
					}
				}
    		}
    	}
    }
</style>