<template>
	<div class="page quiz-page" v-show='show'>
		<div class="quiz-zone">
			<div class="tigan">
				{{ currentQuiz.tigan }}
			</div>

			<ul class="options">
				<li v-for="option in currentQuiz.option" :class='{short:option.length < 6}'>
					<checkbox 
						:label='option' 
						:state='optionStates[$index]' 
						@click="checkResult($index)">
							
					</checkbox>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
	import quizdatas from './quizdatas'
	import Checkbox from './Checkbox'

	function alph2num(str) {
		var result = []
		for (let i = 0; i < str.length; i++) {
			result.push(str.charCodeAt(i) - 97)
		}
		return result
	}

	export default {
		components: {
			Checkbox
		},
		data(){
			return {
				qid: 0,
				quizdatas: quizdatas,
				optionStates: [],
				show: true,
				correctAnswers: 0
			}
		},
		computed: {
			currentQuiz(){
				return this.quizdatas[this.qid]
			},
			currentAnswerIndex(){
				return alph2num(this.quizdatas[this.qid].answer)
			}
		},
		methods: {
			checkResult($index) {
				if (this.optionStates[$index] == 0) {
					// 判断对错
					var answerIndex = this.currentAnswerIndex

					// console.log('answer: ' + answerIndex + ', userAnswer: ' + $index)
					if (answerIndex.indexOf($index) != -1) {
						// console.log('right!') right
						this.optionStates.$set($index, 1)

						this.correctAnswers++
						if (this.correctAnswers == answerIndex.length) {
							setTimeout(() => {
								this.nextQuiz()
							}, 500)
						}
						
					} else {
						// console.log('wrong!') wrong
						this.optionStates.$set($index, 2)
						this.optionStates.$set(answerIndex[0], 3)
						if (answerIndex.length > 1) {
							for (let i = 1; i < answerIndex.length; i++) {
								this.optionStates.$set(answerIndex[i], 1)
							}
						}
					}

				} else if (this.optionStates[$index] == 3){
					// 跳转下一题
					// console.log('show next quiz')
					this.nextQuiz()
				}
				
			},

			nextQuiz(){
				this.correctAnswers = 0
				
				if (this.qid + 1 < this.quizdatas.length) {
					this.qid = this.qid + 1
					this.optionStates = this.getOptionStates()
					// console.log('type: ' + this.currentQuiz.type)

					if (this.currentQuiz.type == 2) {
						// console.log('show game!')
						this.$dispatch('showGame')
					}
				} else {
					this.qid = this.quizdatas.length -1
					// console.log('quiz complete!')
					this.$dispatch('showHero')
				}
			},

			getOptionStates(){
				var result = [];
				var numOptions = this.currentQuiz.option.length;
				for (let i = 0; i < numOptions; i++) {
					result.push(0)
				}
				return result;
			}

		},

		created(){
			this.optionStates = this.getOptionStates()
			// console.log('optionStates: ' + this.optionStates)

			this.$on('nextQuiz', ()=>{
				this.nextQuiz()
			})
		}
	}
</script>

<style scoped>
	.quiz-page {
		background-image: url(../assets/bg-pop-quiz.png);
	}

	.quiz-zone {
		/*border: 1px solid red;*/
		margin: 100px;
	}

	.tigan {
		text-align: center;
		font-size: 30px;
	}

	.tigan::before {
		content: '';
		width: 43px;
		height: 40px;
		display: inline-block;
		background-image: url(../assets/icon-redstar.png);
		background-repeat: no-repeat;
		background-position: center center;
		vertical-align: text-bottom;
	}

	.options {
		margin: 20px 10px 10px 140px;
		padding: 0;
		list-style: none;
	}

	.options li {
		font-size: 30px;
		list-style-type: none;
		margin: 20px 10px;
	}

	.options li.short {
		width: 40%;
		height: 45px;
		float: left;
		/*border: 2px solid steelblue;*/
		margin: 15px 10px;
	}
</style>