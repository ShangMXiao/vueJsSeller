<template>
    <div class="ratingselect">
    	<div class="rating-type border-1px">
    		<span @click="select(2, $event)" class="block positive" :class="{'active':typeSelect===2}" >
    			{{desc.all}}
    			<span class="count">47</span>
    		</span>
    		<span @click="select(0, $event)" class="block positive" :class="{'active':typeSelect===0}">
    			{{desc.positive}}
    			<span class="count">47</span>
    		</span>
    		<span @click="select(1, $event)" class="block negative" :class="{'active':typeSelect===1}">
    			{{desc.negative}}
    			<span class="count">47</span>
    		</span>
    	</div>
    	<div class="switch">
    		<span :class="{'on':contentOnly}" @click="toggleContent($event)" class="icon-check_circle">
    		</span>
    		<span class="text">只看有内容对的评价</span>
    	</div>
    </div>
</template>

<script type="ecmascript-6">
	const POSITIVE = 0
	const NEGATIVE = 1
	const ALL = 2

    export default {
    	props: {
    		ratings: {
    			type: Array,
    			default() {
    				return []
    			}
    		},
    		selectType: {
    			type: Number,
    			default: ALL
    		},
    		onlyContent: {
    			type: Boolean,
    			default: false
    		},
    		desc: {
    			type: Object,
    			default() {
    				return {
    					all: '全部',
    					positive: '满意',
    					negative: '不满意'
    				}
    			}
    		}
    	},
        data() {
            return {
                typeSelect: this.selectType,
                contentOnly: this.onlyContent 
            }
        },
        methods: {
            select(type, event) {
                if (!event._constructed) {
                    return
                }
                this.typeSelect = type
                this.$emit('ratingTypeSelect', type)
            },
            toggleContent(event) {
                if (!event._constructed) {
                    return
                }
                this.contentOnly = !this.contentOnly
                this.$emit('contentToggle', this.contentOnly)
            }
        }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
	@import '../../common/sass/mixin.scss';

    .ratingselect {
		.rating-type {
			padding: 18px 0;
			margin: 0 18px;
			font-size: 0;
			@include border-1px(rgba(7, 17, 27, 0.1));

			.block {
				display: inline-block;
				padding: 8px 12px;
				margin-right: 8px;
				border-radius: 1px;
				font-size: 12px;
				color: rgb(77, 85, 93);

				&.active {
					color: #fff;
				}

				.count {
					margin-left: 2px;
					line-height: 16px;
					font-size: 8px;
				}

				&.positive {
					background: rgba(0, 160, 220, 0.2);
					&.active {
						background: rgb(0, 160, 220);
					}
				}

				&.negative {
					background: rgba(77, 85, 93, 0.2);
					&.active {
						background: rgb(77, 85, 93);
					}
				}
			}
		}
        .switch {
            padding: 12px 18px;
            line-height: 24px;
            border-bottom: 1px solid rgba(7, 17, 27, 0.1);
            color: rgb(147, 153, 159);
            font-size: 0;
            .icon-check_circle {
                display: inline-block;
                font-size: 24px;
                margin-right: 4px;
                &.on {
                    color: green;
                }
            }
            .text {
                display: inline-block;
                font-size: 12px;
            }
        }
    }
</style>