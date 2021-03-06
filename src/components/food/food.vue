<template>
    <transition name="move">
        <div v-show="showFlag" class="food" ref="food">
            <div class="food-content">
                <div class="image-header">
                    <img :src="food.image">
                    <div class="back" @click="hide">
                        <i class="icon-arrow_lift"></i>
                    </div>
                </div>
                <div class="content">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="detail">
                        <span class="sell-count">月售{{food.sellCount}}</span>
                        <span class="rating">好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                        <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                    <div class="cartcontrol-wrapper">
                        <cartcontrol @add-cart="addCart" :food="food"></cartcontrol>
                    </div>
                    <transition name="fade">
                        <div class="buy" @click.stop.prevent="addFirst" v-show="!food.count || food.count===0">加入购物车</div>
                    </transition>
                </div>
                <split v-show="food.info"></split>
                <div class="info" v-show="food.info">
                    <h1 class="title">商品信息</h1>
                    <p class="text">{{food.info}}</p>
                </div>
                <split></split>
                <div class="rating">
                    <h1 class="title">商品评价</h1>
                    <ratingselect @select="ratingType" @toggle="toggleCon" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings">
                    </ratingselect>
                    <div class="rating-wrapper">
                        <ul v-show="food.ratings && food.ratings.length">
                            <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item border-1px">
                                <div class="user">
                                    <span class="name">{{rating.username}}</span>
                                    <img class="avatar" width="12" height="12" :src="rating.avatar">
                                </div>
                                <div class="time">{{rating.rateTime | formatDate}}</div>
                                <div class="text">
                                    <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                                </div>
                            </li>
                        </ul>
                        <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>
<script>
import BScroll from 'better-scroll';
import Vue from 'vue';
import cartcontrol from '../cartcontrol/cartcontrol';
import {
    formatDate
} from '../../common/js/date';
import split from '../split/split';
import ratingselect from '../ratingselect/ratingselect';

const ALL = 2;
export default {
    props: {
        food: {
            type: Object
        }
    },
    data() {
        return {
            showFlag: false,
            selectType: ALL,
            onlyContent: true,
            desc: {
                all: '全部',
                positive: '推荐',
                negative: '吐槽'
            }
        };
    },
    methods: {
        addCart(target) {
            this.$emit('add-cart', target);
        },
        show() {
            this.showFlag = true;
            this.selectType = ALL;
            this.onlyContent = true;
            this.$nextTick(() => {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.food, {
                        click: true
                    });
                } else {
                    this.scroll.refresh();
                }
            });
        },
        hide() {
            this.showFlag = false;
        },
        addFirst(event) {
            if (!event._constructed) {
                return;
            }
            this.$emit('add-cart', event.target);
            Vue.set(this.food, 'count', 1);
        },
        needShow(type, text) {
            if (this.onlyContent && !text) {
                return false;
            }
            if (this.selectType === ALL) {
                return true;
            } else {
                return type === this.selectType;
            }
        },
        ratingType(type) {
            this.selectType = type;
            this.$nextTick(() => {
                this.scroll.refresh();
            });
        },
        toggleCon(onlyContent) {
            this.onlyContent = !onlyContent;
            this.$nextTick(() => {
                this.scroll.refresh();
            });
        }
    },
    filters: {
        formatDate(time) {
            let date = new Date(time);
            return formatDate(date, 'yyyy-MM-dd hh:mm');
        }
    },
    components: {
        cartcontrol,
        split,
        ratingselect
    }
};
</script>
<style lang="less">
@import url(../../common/style/mixin.less);
.food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0.96rem;
    z-index: 30;
    width: 100%;
    background: #fff;
    transform: translate3d(0, 0, 0);
    &.move-enter-active,
    &.move-leave-active {
        transition: all 0.2s linear;
        transform: translate3d(0, 0, 0);
    }
    &.move-enter,
    &.move-leave-active {
        transform: translate3d(100%, 0, 0);
    }
    .food-content {
        position: relative;
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
                top: 0.2rem;
                left: 0;
                .icon-arrow_lift {
                    display: block;
                    padding: 0.2rem;
                    font-size: 0.4rem;
                    color: #fff;
                }
            }
        }
        .content {
            padding: 0.36rem;
            position: relative;
            .title {
                line-height: 0.28rem;
                margin-bottom: 0.16rem;
                font-size: 0.28rem;
                font-weight: 700;
                color: rgb(7, 17, 27);
            }
            .detail {
                margin-bottom: 0.36rem;
                line-height: 0.2rem;
                font-size: 0;
                height: 0.2rem;
                .sell-count,
                .rating {
                    font-size: 0.2rem;
                    color: rgb(147, 153, 159);
                }
                .sell-count {
                    margin-right: 0.24rem;
                }
            }
            .price {
                font-weight: 700;
                line-height: 0.48rem;
                .now {
                    font-size: 0.28rem;
                    color: rgb(240, 20, 20);
                }
                .old {
                    text-decoration: line-through;
                    font-size: 0.2rem;
                    color: rgb(147, 153, 159);
                }
            }
            .cartcontrol-wrapper {
                position: absolute;
                right: 0.24rem;
                bottom: 0.24rem;
            }
            .buy {
                position: absolute;
                right: 0.36rem;
                bottom: 0.36rem;
                z-index: 10;
                height: 0.48rem;
                line-height: 0.48rem;
                padding: 0 0.24rem;
                box-sizing: border-box;
                font-size: 0.2rem;
                border-radius: 0.24rem;
                color: #fff;
                background: rgb(0, 160, 220);
                &.fade-enter-active,
                &.fade-leave-active {
                    transition: all 0.2s;
                    opacity: 1;
                }
                &.fade-enter-active,
                &.fade-leave-to {
                    opacity: 0;
                }
            }
        }
        .info {
            padding: 0.36rem;
            .title {
                line-height: 0.28rem;
                margin-bottom: 0.12rem;
                font-size: 0.28rem;
                color: rgb(7, 17, 27);
            }
            .text {
                line-height: 0.48rem;
                padding: 0 0.16rem;
                font-size: 0.24rem;
                color: rgb(77, 85, 93);
            }
        }
        .rating {
            padding-top: 0.36rem;
            .title {
                line-height: 0.28rem;
                margin-left: 0.36rem;
                font-size: 0.28rem;
                color: rgb(7, 17, 27);
            }
            .rating-wrapper {
                padding: 0 0.36rem;
                .rating-item {
                    position: relative;
                    padding: 0.32rem 0;
                    .border-1px(rgba(7, 17, 27, 0.1));
                    .user {
                        position: absolute;
                        right: 0;
                        top: 0.32rem;
                        line-height: 0.24rem;
                        font-size: 0;
                        .name {
                            display: inline-block;
                            vertical-align: top;
                            margin-right: 0.12rem;
                            font-size: 0.2rem;
                            color: rgb(147, 153, 159);
                        }
                        .avatar {
                            border-radius: 50%;
                        }
                    }
                    .time {
                        line-height: 0.24rem;
                        color: rgb(147, 153, 159);
                        font-size: 0.2rem;
                        margin-bottom: 0.12rem;
                    }
                    .text {
                        line-height: 0.32rem;
                        font-size: 0.24rem;
                        color: rgb(7, 17, 27);
                        .icon-thumb_up,
                        .icon-thumb_down {
                            line-height: 0.32rem;
                            margin-right: 0.08rem;
                            font-size: 0.24rem;
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
                    padding: 0.32rem 0;
                    font-size: 0.24rem;
                    color: rgb(147, 153, 159);
                }
            }
        }
    }
}
</style>
