<template>
    <div class="ratings">
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
                        <star :size="36" :score="seller.foodScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <ratingselect @select="ratingType" @toggle="toggleCon" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings">
            </ratingselect>
        </div>
    </div>
</template>
<script>
import star from '../star/star';
import split from '../split/split';
import ratingselect from '../ratingselect/ratingselect';

const ALL = 2;
const ERR_OK = 0;
export default {
    props: {
        seller: {
            type: Object
        }
    },
    data() {
        return {
            ratings: [],
            showFlag: false,
            selectType: ALL,
            onlyContent: true
        };
    },
    created() {
        this.$http.get('/api/ratings').then((response) => {
            if (response.data.error === ERR_OK) {
                this.ratings = response.data.data;
            }
        });
    },
    methods: {
        ratingType(type) {
            this.selectType = type;
        },
        toggleCon(onlyContent) {
            this.onlyContent = !onlyContent;
        }
    },
    components: {
        star,
        split,
        ratingselect
    }
};
</script>
<style lang="less">
.ratings {
    position: absolute;
    top: 3.48rem;
    bottom: 0;
    width: 100%;
    left: 0;
    overflow: hidden;
    .overview {
        display: flex;
        padding: 0.36rem 0;
        .overview-left {
            flex: 0 0 2.75rem;
            padding: 0.12rem 0;
            width: 2.75rem;
            border-right: 1px solid rgba(7, 17, 27, 0.2);
            text-align: center;
            @media only screen and (max-width: 6.4rem) {
                flex: 0 0 2.4rem;
                width: 2.4rem;
            }
            .score {
                font-size: 0.48rem;
                color: rgb(255, 153, 0);
                line-height: 0.56rem;
                margin-bottom: 0.12rem;
            }
            .title {
                font-size: 0.24rem;
                line-height: 0.24rem;
                color: rgb(7, 17, 27);
                margin-bottom: 0.16rem;
            }
            .rank {
                font-size: 0.2rem;
                line-height: 0.2rem;
                color: rgb(147, 153, 159);
            }
        }
        .overview-right {
            flex: 1;
            padding: 0.12rem 0 0.12rem 0.48rem;
            @media only screen and (max-width: 6.4rem) {
                padding-left: 0.12rem;
            }
            .score-wrapper {
                font-size: 0;
                line-height: 0.36rem;
                margin-bottom: 0.16rem;
                .title {
                    font-size: 0.24rem;
                    color: rgb(7, 17, 27);
                    display: inline-block;
                    vertical-align: top;
                    line-height: 0.36rem;
                }
                .star {
                    display: inline-block;
                    vertical-align: top;
                    margin: 0 0.12rem;
                    .star-item{
                    	margin-right: 0.22rem;
                    }
                }
                .score {
                    display: inline-block;
                    vertical-align: top;
                    line-height: 0.36rem;
                    font-size: 0.24rem;
                    color: rgb(255, 153, 0);
                }
            }
        }
        .delivery-wrapper {
            font-size: 0;
            .title {
                font-size: 0.24rem;
                color: rgb(7, 17, 27);
                line-height: 0.36rem;
            }
            .delivery {
                font-size: 0.24rem;
                color: rgb(147, 159, 153);
                margin-left: 0.24rem;
            }
        }
    }
}
</style>
