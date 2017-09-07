<template>
	<div class="star" :class="starType">
		<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span >
	</div>
</template>

<script type="text/javascript">
	const LENGTH = 5;
	const CLS_ON = 'on';
	const CLS_HALF = 'half';
	const CLS_OFF = 'off';

	export default {
		props: {
			size: {
				type: Number
			},
			score: {
				type: Number
			}
		},
		computed: {
			starType() {
				return 'star-' + this.size;
			},
			itemClasses() {
				let result = [];
				let score = Math.floor(this.score * 2) / 2;
				let hasDecimal = score % 1 !== 0;
				let integer = Math.floor(score);
				for (var i = 0; i < integer; i++) {
					result.push(CLS_ON);
				}
				if (hasDecimal) {
					result.push(CLS_HALF);
				}
				while (result.length < LENGTH) {
					result.push(CLS_OFF);
				}
				return result;
			}
		}
	};
</script>

<style lang="less">
	.star{
		font-size: 0;
		&.star-48{
			.star-item{
				margin-right: 0.44rem;
				width: 0.4rem;
				height: 0.4rem;
				display: inline-block;
				background-repeat: no-repeat;
				&:last-child{
					margin-right: 0;
				}
				&.on{
					background: url(star48_on@2x.png);
					background-size: 0.4rem 0.4rem;
				}
				&.half{
					background: url(star48_half@2x.png);
					background-size: 0.4rem 0.4rem;
				}
				&.off{
					background: url(star48_off@2x.png);
					background-size: 0.4rem 0.4rem;
				}
			}
		}
		&.star-36{
			.star-item{
				width: 0.3rem;
				height: 0.3rem;
				margin-right: 0.32rem;
				display: inline-block;
				background-size: 0.3rem 0.3rem;
				&:last-child{
					margin-right: 0;
				}
				&.on{
					background: url(star36_on@2x.png);
					background-size: 0.3rem 0.3rem;
				}
				&.half{
					background: url(star36_half@2x.png);
					background-size: 0.3rem 0.3rem;
				}
				&.off{
					background: url(star36_off@2x.png);
					background-size: 0.3rem 0.3rem;
				}
			}
			
		}
		&.star-24{
			.star-item{
				width: 0.2rem;
				height: 0.2rem;
				display: inline-block;
				margin-right: 0.06rem;
				background-size: 0.2rem 0.2rem;
				&:last-child{
					margin-right: 0;
				}
				&.on{
					background: url(star24_on@2x.png);
					background-size: 0.2rem 0.2rem;
				}
				&.half{
					background: url(star24_half@2x.png);
					background-size: 0.2rem 0.2rem;
				}
				&.off{
					background: url(star24_off@2x.png);
					background-size: 0.2rem 0.2rem;
				}
			}
			
		}
		
	}
</style>