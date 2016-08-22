<template>
	<div class="page game-page">
		<div id="game-index" class="bg-pop" v-if='state == 0'>
			<div class="title-game"></div>
			<div class="tip-game"></div>
			<div class="btn-go-rescue btn" @click='showCountdown'></div>
		</div>

		<div id="game-countdown" class="pop-bg-game-tip" v-if='state == 1'>
			<div class="tip-game-push-space"></div>
			<div class="spacebar"></div>
			<div class="tip-arrow"></div>
			<div class="count-num n3" v-if='countdown == 3'></div>
			<div class="count-num n2" v-if='countdown == 2'></div>
			<div class="count-num n1" v-if='countdown == 1'></div>
			<div class="count-num n-go" v-if='countdown == 0'></div>
		</div>

		<div id="game-play" class="bg-game-playing" v-if='state == 2'>
			<div class="hero-fail hero" v-show='frame==0'></div>
			<div class="hero-action-f4 hero" v-show='frame==1'></div>
			<div class="hero-action-f3 hero" v-show='frame==2'></div>
			<div class="hero-action-f2 hero" v-show='frame==3'></div>		
			<div class="hero-action-1 hero" v-show='frame==4'></div>
			<div class="hero-action-2 hero" v-show='frame==5'></div>
			<div class="hero-action-3 hero" v-show='frame==6'></div>
			<div class="hero-action-4 hero" v-show='frame==7'></div>
			<div class="hero-success hero" v-show='frame==8'></div>

			<div class="fg-game-playing"></div>

			<div class="btn btn-goon" 
				v-if='frame == 8'
				@click='showNextQuiz'
				>
			</div>
			
			<div class="btn btn-rescue-again" 
				v-if='frame == 0'
				@click='showCountdown'
				>		
			</div>
		</div>
	</div>
</template>

<script>
	var countdownID;
	var gravityID;
	export default {
		data() {
			return {
				state: 0,
				countdown: 3,
				frame: 4
			}
		},

		methods: {
			showCountdown(){
				this.state = 1
				this.countdown = 3
				this.frame = 4
				countdownID = setInterval(()=>{
					console.log('countdown: ' + this.countdown)
					this.countdown = this.countdown - 1
					if (this.countdown < 0) {
						clearInterval(countdownID)
						this.showGameplay()
						this.countdown = 3
					}
				}, 1000);
			},

			showGameplay(){
				this.state = 2;
				
				window.addEventListener("keyup", this.onKeyHandler);
				gravityID = setInterval(()=>{
					this.frame--
					if (this.frame <= 0) {
						clearInterval(gravityID)
						window.removeEventListener('keyup', this.onKeyHandler)
						this.frame = 0
					}
				}, 400)
			},

			onKeyHandler(e){
				if (e.keyCode == '32') {
					console.log('come on!')
					this.frame++
					if (this.frame >= 8) {
						this.frame = 8
						clearInterval(gravityID)
						window.removeEventListener('keyup', this.onKeyHandler)
					}
				}		
			},

			showNextQuiz(){
				console.log('show next quiz')
				this.$dispatch('nextQuiz')
			}
		},

		beforeDestroy(){
			clearInterval(countdownID)
		}
	}
</script>

<style scoped>
	.game-page {
		/*background-image: url(../assets/bg-game-playing.png);*/
	}

	.game-page div {
		position: absolute;
	}

/*game index*/
#game-index {
	z-index: 100;
}

.bg-pop{
	width:956px;
	height:536px;
	background-image:url(../assets/bg-pop.png);
}

.btn-go-rescue{
	width:174px;
	height:47px;
	background-image:url(../assets/btn-go-rescue.png);
}

.tip-game{
	width:441px;
	height:24px;
	background-image:url(../assets/tip-game.png);
}

.title-game {
	width:508px;
	height:217px;
	background-image:url(../assets/title-game.png);
}

.title-game{ left:242px; top:103px; }
.tip-game{ left:286px; top:454px; }
.btn-go-rescue{ left:438px; top:366px; }
.bg-pop{ left:22px; top:32px; }

/*game countdown*/
#game-countdown {
	z-index: 99;
}

.count-num {
	width: 126px;
	height: 182px;
	background-position: center center;
	background-repeat: no-repeat;
	left: 448px;
	top: 156px;
}

.n1 { background-image: url(../assets/1.png); }
.n2 { background-image: url(../assets/2.png); }
.n3 { background-image: url(../assets/3.png); }
.n-go { background-image: url(../assets/go.png); }

.pop-bg-game-tip{
	width:1000px;
	height:600px;
	background-image:url(../assets/pop-bg-game-tip.png);
	background-position: 44% 41%;
	background-repeat: no-repeat;
}

.spacebar{
	width:320px;
	height:84px;
	background-image:url(../assets/spacebar.gif);
}

.tip-arrow{
	width:96px;
	height:89px;
	background-image:url(../assets/tip-arrow.png);
}

.tip-game-push-space{
	width:186px;
	height:26px;
	background-image:url(../assets/tip-game-push-space.png);
}

.tip-game-push-space{ left:419px; top:390px; }
.spacebar{ left:356px; top:444px; }
.tip-arrow{ left:676px; top:352px; }

/*game playing*/
#game-play {
	z-index: 98;
}

.bg-game-playing{
	width:1000px;
	height:600px;
	background-image:url(../assets/bg-game-playing.png);
	background-repeat: no-repeat;
	background-position: 30% center;
}

.fg-game-playing{width:884px;height:201px;background-image:url(../assets/fg-game-playing.png);}
.hero-action-1{width:400px;height:295px;background-image:url(../assets/hero-action-1.png);}
.hero-action-2{width:473px;height:309px;background-image:url(../assets/hero-action-2.png);}
.hero-action-3{width:465px;height:300px;background-image:url(../assets/hero-action-3.png);}
.hero-action-4{width:445px;height:291px;background-image:url(../assets/hero-action-4.png);}
.hero-action-f2{width:389px;height:293px;background-image:url(../assets/hero-action-f2.png);}
.hero-action-f3{width:362px;height:277px;background-image:url(../assets/hero-action-f3.png);}
.hero-action-f4{width:367px;height:253px;background-image:url(../assets/hero-action-f4.png);}
.hero-fail{width:666px;height:291px;background-image:url(../assets/hero-fail.png);}
.hero-success{width:649px;height:327px;background-image:url(../assets/hero-success.png);}

.fg-game-playing{ left:52px; top:334px; }

/*.hero { opacity: 0; }*/
.hero.show { opacity: 1; }

.hero-success{ left:225px; top:89px; }
.hero-action-4{ left:373px; top:120px; }
.hero-action-3{ left:325px; top:116px; }
.hero-action-2{ left:245px; top:116px; }
.hero-action-1{ left:279px; top:118px; }
.hero-action-f2{ left:261px; top:108px; }
.hero-action-f3{ left:280px; top:124px; }
.hero-action-f4{ left:276px; top:146px; }
.hero-fail{ left:111px; top:131px; }

.btn-goon{width:174px;height:47px;background-image:url(../assets/btn-goon.png);}
.btn-rescue-again{width:174px;height:47px;background-image:url(../assets/btn-rescue-again.png);}

.btn-goon,
.btn-rescue-again {
	left: 402px;
	top: 466px;
}
</style>