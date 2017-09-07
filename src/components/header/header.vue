<template>
<div class="header">
	<div class="content-wrapper">
		<div class="avatar">
			<img :src="seller.avatar">
		</div>
		<div class="content">
			<div class="title">
				<span class="brand"></span>
				<span class="name">{{seller.name}}</span>
			</div>
			<div class="description">
				{{seller.description}}/{{seller.deliveryTime}}分钟送达
			</div>
			<div v-if="seller.supports" class="support">
				<span class="icon" :class="classMap[seller.supports[0].type]"></span>
				<span class="text">{{seller.supports[0].description}}</span>
			</div>
		</div>
		<div v-if="seller.supports"  @click="showDetail" class="support-count">
			<span class="count">{{seller.supports.length}}个</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
	</div>
	<div class="bulletin-wrapper" @click="showDetail">
		<span class="bulletin-title"></span>
		<p class="bulletin-text">{{seller.bulletin}}</p>
		<i class="icon-keyboard_arrow_right"></i>
	</div>
	<div class="background">
		<img :src="seller.avatar" width="100%" height="100%" />
	</div>
	<transition name="fade">
		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix">
				<div class="detail-main">
					<h1 class="name">{{seller.name}}</h1>
					<div class="star-wrapper">
						<star :size="48" :score="seller.score"></star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul v-if="seller.supports" class="supports">
						<li class="support-item" v-for="(item, index) in seller.supports">
							<span class="icon" :class="classMap[seller.supports[index].type]"></span>
							<span class="text">{{seller.supports[index].description}}</span>
						</li>
					</ul>
					<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="bulletin">
						<p class="content">{{seller.bulletin}}</p>
					</div>
				</div>
			</div>
			<div class="detail-close" @click="hideDetail">
				<i class="icon-close"></i>
			</div>
		</div>
	</transition>
</div>
</template>

<script>
import star from '../star/star';
export default {
	props: {
		seller: {
			type: Object
		}
	},
	data() {
		return {
			detailShow: false
		};
	},
	created() {
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	},
	methods: {
		showDetail() {
			this.detailShow = true;
		},
		hideDetail() {
			this.detailShow = false;
		}
	},
	components: {
		star
	}
};
</script>

<style lang="less">
	@import "../../common/style/base.less";
	.header{
		color: #fff;
		overflow: hidden;
		position:relative;
		background: rgba(7,17,27,0.5);
		.content-wrapper{
			padding: 0.48rem 0.24rem 0.36rem 0.48rem;
			position: relative;
			display: flex;
			.avatar{
				img{
					width: 1.28rem;
					height: 1.28rem;
					display: block;
				}
			}
			.content{
				margin-left: 0.32rem;
				.title{
					margin: 0.04rem 0 0.16rem 0;
					display: flex;
					.brand{
						width: 0.6rem;
						height: 0.36rem;
						display: block;
						background:url(brand@2x.png) no-repeat center;
						background-size: 0.6rem 0.36rem;
					}
					.name{
						margin-left: 0.12rem;
						font-size: 0.32rem;
						color: rgb(255,255,255);
						font-weight: bold;
						line-height: 0.36rem;
					}
				}
				.description{
					font-size: 0.24rem;
					color: rgb(255,255,255);
					font-weight: 200;
					line-height: 0.24rem;
				}
				.support{
					margin: 0.2rem 0 0.04rem 0;
					display: flex;
					align-items: center;
					.icon{
						width: 0.24rem;
						height: 0.24rem;
						&.decrease{
							background: url(decrease_1@2x.png) no-repeat center;
							background-size: 0.24rem 0.24rem;
						}
						&.discount{
							background: url(discount_1@2x.png) no-repeat center;
							background-size: 0.24rem 0.24rem;
						}
						&.guarantee{
							background: url(guarantee_1@2x.png) no-repeat center;
							background-size: 0.24rem 0.24rem;
						}
						&.invoice{
							background: url(invoice_1@2x.png) no-repeat center;
							background-size: 0.24rem 0.24rem;
						}
						&.special{
							background: url(special_1@2x.png) no-repeat center;
							background-size: 0.24rem 0.24rem;
						}
					}
					.text{
						margin-left: 0.08rem;
						font-size: 0.2rem;
						color: rgb(255,255,255);
						font-weight: 200;
						line-height: 0.24rem;
					}
				}
			}
			.support-count{
				display: flex;
				align-items: center;
				position: absolute;
				right: 0.24rem;
				bottom: 0.28rem;
				padding: 0 0.16rem;
				height: 0.48rem;
				line-height: 0.48rem;
				border-radius: 0.28rem;
				background:rgba(0,0,0,0.2);
				color: rgb(255,255,255);
				text-align: center;
				.count{
					font-size: 0.2rem;
				}
				.icon-keyboard_arrow_right{
					font-size: 0.2rem;
					margin-left: 0.04rem;
				}
			}
		}
		.bulletin-wrapper{
			height: 0.56rem;
			padding: 0 0.24rem;
			display: flex;
			justify-content: space-between;
			align-items: center;
			background:rgba(7,17,27,0.2);
			.bulletin-title{
				background: url(bulletin@2x.png) no-repeat center;
				background-size: 0.44rem 0.24rem;
				display: block;
				width: 0.44rem;
				height: 0.24rem;
			}
			.bulletin-text{
				margin-top: 0.04rem;
				width: 6.2rem;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
				font-size: 0.2rem;
				color: rgb(255,255,255);
				font-weight: 200;
				height: 0.56rem;
				margin-left: 0.08rem;
				line-height: 0.56rem;
			}
			.icon-keyboard_arrow_right{
				margin-left: 0.08rem;
				font-size: 0.2rem;
			}
		}
		.background{
			position:absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			z-index: -1;
			filter:blur(0.1rem);
		}
		.detail{
			width: 100%;
			position:fixed;
			z-index: 100;
			height: 100%;
			top: 0;
			left: 0;
			overflow:auto;
			background: rgba(7,17,27,0.8);
			&.fade-enter-active, &.fade-leave-active {
			  	transition: all .5s;
			  	opacity: 1;
				background: rgba(7,17,27,0.8);
			}
			&.fade-enter, &.fade-leave-active {
			  	opacity: 0;
				background: rgba(7,17,27,0);
			}
			.detail-wrapper{
				min-height: 100%;
				.detail-main{
					margin-top: 1.28rem;
					padding-bottom: 1.28rem;
					.name{
						line-height: 0.32rem;
						font-size: 0.32rem;
						font-weight: 700;
						color: rgb(255,255,255);
						text-align: center;
					}
					.star-wrapper{
						margin-top: 0.36rem;
						padding: 0.04rem 0;
						text-align: center;
					}
					.title{
						display: flex;
						width: 80%;
						margin: 0.56rem auto 0.48rem;
						.line{
							flex: 1;
							position: relative;
							top: -0.12rem;
							border-bottom: 1px solid rgba(255,255,255,0.2);
						}
						.text{
							padding: 0 0.24rem;
							font-size: 0.28rem;
							font-weight: 700;
						}
					}
					.supports{
						width: 80%;
						margin: 0 auto;
						.support-item{
							padding: 0 0.24rem;
							margin-bottom: 0.24rem;
							font-size: 0;
							&:last-child{
								margin-bottom: 0;
							}
							.icon{
								display: inline-block;
								width: 0.32rem;
								height: 0.32rem;
								vertical-align: top;
								margin-right: 0.12rem;
								background-size: 0.32rem 0.32rem;
								&.decrease{
									background: url(./decrease_2@2x.png) no-repeat center;
									background-size: 0.32rem 0.32rem;
								}
								&.discount{
									background: url(./discount_2@2x.png) no-repeat center;
									background-size: 0.32rem 0.32rem;
								}
								&.guarantee{
									background: url(./guarantee_2@2x.png) no-repeat center;
									background-size: 0.32rem 0.32rem;
								}
								&.invoice{
									background: url(./invoice_2@2x.png) no-repeat center;
									background-size: 0.32rem 0.32rem;
								}
								&.special{
									background: url(./special_2@2x.png) no-repeat center;
									background-size: 0.32rem 0.32rem;
								}
							}
							.text{
								line-height: 0.32rem;
								font-size: 0.24rem;
							}
						}
					}
					.bulletin{
						width: 80%;
						margin: 0 auto;
						.content{
							padding: 0 0.24rem;
							line-height: 0.48rem;
							font-size: 0.24rem;
						}
					}
				}
			}
			.detail-close{
				position:relative;
				width: 0.64rem;
				height: 0.64rem;
				margin: -1.28rem auto 0 auto;
				clear:both;
				font-size: 0.64rem;
			}
		}
	}
</style>