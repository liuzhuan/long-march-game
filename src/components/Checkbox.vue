<template>
	<div class='checkbox'>
		<div class="check-ui">
			<div class="icon-check" v-show='state == 1 || state == 3'></div>
			<div class="icon-cross" v-show='state == 2'></div>
		</div>
		{{ label }}
		<div class="icon-hand" v-if="state == 3"></div>
	</div>
</template>

<script>
	export default {
		props: {
			label: String,
			state: {
				type: Number,
				default: 0
			}
		}
	}
</script>

<style scoped>
	.checkbox {
		/*background: rgba(255,0,0,.2);*/
		/*margin: 1px;*/
		position: relative;
		padding-left: 50px;
		cursor: pointer;
	}

	.check-ui {
		position: absolute;
		left: 0px;
		top: 7px;
		width: 29px;
		height: 28px;
		background-image: url(../assets/icon-checkbox.png);
		cursor: pointer;
	}

	.icon-check {
		position: absolute;
		left: -8px;
		top: -10px;
		width: 49px;
		height: 35px;
		background-image: url(../assets/icon-check.png);
	}

	.icon-cross {
		position: absolute;
		left: -4px;
		top: -6px;
		width: 38px;
		height: 38px;
		background-image: url(../assets/icon-cross.png);
	}

	.icon-hand {
		/*position: absolute;
		right: 10px;
		top: 0px;*/
		width: 55px;
		height: 34px;
		background-image: url(../assets/icon-hand.png);
		position: static;
		display: inline-block;
		vertical-align: middle;
		animation: poke .7s infinite ease;
	}

	@keyframes poke {
		0% {transform: translateX(0);}
		50% {transform: translateX(10px);}
		100% {transform: translateX(0);}
	}
</style>