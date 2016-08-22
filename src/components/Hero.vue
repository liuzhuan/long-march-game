<template>
	<div class='hero-container'>
		<div class="page win-page" v-if='state == 0'>
			<div class="btn search-btn" @click='showHero'></div>
		</div>

		<div class='page hero-page' v-if='state == 1'>
			<h2>我的长征战友是：</h2>
			<img class='headimg' :src="currentFriend.img">
			<section class="detail" id='myscroll'>
				<article>
					<h3>{{ currentFriend.name }}</h3>
					<p>{{ currentFriend.detail }}</p>
				</article>
			</section>
			<div class="btn btn-search-others" @click='searchOthers'></div>
			<div class="btn btn-msg-board" @click='showBoard'></div>
		</div>
	</div>
</template>

<script>
	import friendDatas from './friendDatas'
	import IScroll from 'iscroll'

	var myscroll;

	export default {
		data(){
			return {
				state: 0,
				friends: friendDatas,
				currentFriend: {}
			}
		},

		methods: {
			showHero(){
				this.state = 1
				this.$nextTick(()=>{
					myscroll = new IScroll('#myscroll', {
						scrollbars: true,
						mouseWheel: true
					})
				})
			},

			showBoard() {
				this.$dispatch('showBoard')
			},

			randomCurrentFriend(){
				var idx = Math.floor(Math.random() * this.friends.length)
				this.currentFriend = this.friends[idx]

				this.$nextTick(()=>{
					if (myscroll) {
						myscroll.refresh()
						myscroll.scrollTo(0,0)
					}
				})
			},

			searchOthers(){
				console.log('search other friends')
				this.randomCurrentFriend()
			}
		},

		created(){
			this.randomCurrentFriend()
		}
	}
</script>

<style scoped>
	.win-page {
		background-image: url(../assets/pop-bg-end.png);
	}

	.hero-page {
		background-image: url(../assets/bg-pop.png);
	}

	.search-btn {
		width: 163px;
		height: 40px;
		background-image: url(../assets/btn-search-friends.png);
		position: absolute;
		right: 160px;
		bottom: 200px;
	}

	.headimg {
		position: absolute;
		left: 162px;
		top: 144px;
		width: 247px;
		height: 324px;
	}

	.hero-page h2 {
		color: #cf2016;
		font-size: 30px;
		position: absolute;
		left: 162px;
		top: 60px;
	}

	.hero-page .detail {
		position: absolute;
		top: 144px;
		left: 450px;
		width: 420px;
		height: 320px;
		/*border: 1px solid red;*/
		color: #cf2016;
		/*overflow-y: scroll;*/
		overflow-y: hidden;
		padding-right: 20px;
	}

	.detail h3 {
		margin-top: 0;
	}

	.detail p {
		font-size: 18px;
		line-height: 180%;
		text-indent: 2em;
	}

	.btn-search-others {
		width: 167px;
		height: 38px;
		background-image: url(../assets/btn-search-others.png);
		position: absolute;
		bottom: 80px;
		right: 320px;
	}

	.btn-msg-board {
		width: 107px;
		height: 38px;
		background-image: url(../assets/btn-msg-board.png);
		position: absolute;
		right: 180px;
		bottom: 80px;
	}
</style>