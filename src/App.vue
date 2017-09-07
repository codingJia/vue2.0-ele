<template>
<div id="app">
	<v-header :seller="seller"></v-header>	
	<div class="tab border-1px">
		<div class="tab-item">
			<router-link to="/goods">商品</router-link>
		</div>
		<div class="tab-item">
			<router-link to="/ratings">评价</router-link>
		</div>
		<div class="tab-item">
			<router-link to="/seller">商家</router-link>
		</div>
	</div>
	<router-view :seller="seller"></router-view>
</div>
</template>

<script>
import header from './components/header/header.vue';
const ERR_OK = 0;
export default {
	data() {
		return {
			seller: {}
		};
	},
	created() {
		this.$http.get('/api/seller').then(response => {
			if (response.data.error === ERR_OK) {
				this.seller = response.data.data;
			}
		});
	},
	components: {
		'v-header': header
	}
};
</script>

<style lang="less">
@import url('./common/style/mixin.less');
#app{
	.tab{
		display: flex;
		width: 100%;
		height: 0.8rem;
		line-height: 0.8rem;
		.border-1px(rgba(7,17,27,0.1));
		.tab-item{
			flex: 1;
			text-align: center;
			a{
				display: block;
				font-size: 0.28rem;
				color: rgb(77,85,93);
				&.active{
					color:rgb(240,20,20);
				}
			}
		}
	}
}
</style>
