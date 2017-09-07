<template>
    <div class="goods">
        <div class="menu-wrapper" ref='menuWrapper'>
            <ul>
                <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
                    <span class="text border-1px">
					<span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref='foodsWrapper'>
            <ul>
                <li v-for='item in goods' class="food-list food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
                            <div class="icon">
                                <img width="57" height="57" :src="food.icon">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol @add-cart="addCart" :food="food"></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
        <food :food="selectedFood" ref="food"></food>
    </div>
</template>
<script>
import BScroll from 'better-scroll';
import shopcart from '../shopcart/shopcart';
import cartcontrol from '../cartcontrol/cartcontrol';
import food from '../food/food';
const ERR_OK = 0;
export default {
    props: {
        seller: {
            type: Object
        }
    },
    data() {
        return {
            goods: [],
            listHeight: [],
            scrollY: 0,
            selectedFood: {}
        };
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                    return i;
                }
            }
            return 0;
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach((good) => {
                good.foods.forEach((food) => {
                    if (food.count) {
                        foods.push(food);
                    }
                });
            });
            return foods;
        }
    },
    created() {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        this.$http.get('/api/goods').then(response => {
            if (response.data.error === ERR_OK) {
                this.goods = response.data.data;
                this.$nextTick(() => {
                    this._initScroll();
                    this._calculateHeight();
                });
            }
        });
    },
    methods: {
        addCart(target) {
            this._drop(target);
        },
        _drop(target) {
            // 体验优化，异步执行下落动画
            this.$nextTick(() => {
                this.$refs.shopcart.drop(target);
            });
        },
        selectMenu(index, event) {
            if (!event._constructed) {
                return;
            }
            let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
            let el = foodList[index];
            this.foodsScroll.scrollToElement(el, 300);
        },
        selectFood(food, event) {
            if (!event._constructed) {
                return;
            }
            this.selectedFood = food;
            this.$refs.food.show();
        },
        _initScroll() {
            this.menuScroll = new BScroll(this.$refs.menuWrapper, {
                click: true
            });
            this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
                probeType: 3,
                click: true
            });
            this.foodsScroll.on('scroll', (pos) => {
                this.scrollY = Math.abs(Math.round(pos.y));
            });
        },
        _calculateHeight() {
            let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
            let height = 0;
            this.listHeight.push(height);
            for (let i = 0; i < foodList.length; i++) {
                let item = foodList[i];
                height += item.clientHeight;
                this.listHeight.push(height);
            }
        }
    },
    components: {
        shopcart,
        cartcontrol,
        food
    }
};
</script>
<style lang="less">
@import "../../common/style/mixin.less";
.goods {
    position: absolute;
    width: 100%;
    top: 3.48rem;
    bottom: 0.92rem;
    display: flex;
    overflow: hidden;
    .menu-wrapper {
        flex: 0 0 1.6rem;
        width: 1.6rem;
        background: #f3f5f7;
        .menu-item {
            display: table;
            height: 1.08rem;
            width: 1.12rem;
            line-height: 0.28rem;
            padding: 0 0.24rem;
            &.current {
                position: relative;
                z-index: 10;
                margin-top: -0.02rem;
                background: #fff;
                font-weight: 700;
                .text {
                    .border-none();
                }
            }
            .icon {
                display: inline-block;
                vertical-align: top;
                margin-right: 0.04rem;
                width: 0.24rem;
                height: 0.24rem;
                &.decrease {
                    background: url(decrease_3@2x.png) no-repeat center;
                    background-size: 0.24rem 0.24rem;
                }
                &.discount {
                    background: url(discount_3@2x.png) no-repeat center;
                    background-size: 0.24rem 0.24rem;
                }
                &.guarantee {
                    background: url(guarantee_3@2x.png) no-repeat center;
                    background-size: 0.24rem 0.24rem;
                }
                &.invoice {
                    background: url(invoice_3@2x.png) no-repeat center;
                    background-size: 0.24rem 0.24rem;
                }
                &.special {
                    background: url(special_3@2x.png) no-repeat center;
                    background-size: 0.24rem 0.24rem;
                }
            }
            .text {
                font-size: 0.24rem;
                display: table-cell;
                width: 1.02rem;
                vertical-align: middle;
                .border-1px(rgba(7, 17, 27, 0.1));
            }
        }
    }
    .foods-wrapper {
        flex: 1;
        background: #fff;
        .title {
            padding-left: 0.28rem;
            height: 0.52rem;
            line-height: 0.52rem;
            border-left: 0.04rem solid #d9dde1;
            font-size: 0.24rem;
            color: rgb(147, 153, 159);
            background: #f3f5f7;
        }
        .food-item {
            display: flex;
            margin: 0.36rem;
            padding-bottom: 0.36rem;
            .border-1px(rgba(7, 17, 27, 0.1));
            &:last-child {
                .border-none();
                margin-bottom: 0;
            }
            .icon {
                flex: 0 0 0.57rem;
                margin-right: 0.2rem;
            }
            .content {
                flex: 1;
                .name {
                    font-size: 0.34rem;
                    margin: 0.04rem 0 0.16rem 0;
                    height: 0.28rem;
                    line-height: 0.28rem;
                    color: rgb(7, 17, 27);
                }
                .desc,
                .extra {
                    line-height: 0.2rem;
                    color: rgb(147, 153, 159);
                    font-size: 0.2rem;
                }
                .desc {
                    margin-bottom: 0.16rem;
                    line-height: 0.24rem;
                }
                .extra {
                    .count {
                        margin-right: 0.24rem;
                    }
                }
                .price {
                    font-weight: 700;
                    line-height: 0.48rem;
                    .now {
                        margin-left: 0.16rem;
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
                    right: 0;
                    bottom: 0.24rem;
                }
            }
        }
    }
}
</style>
