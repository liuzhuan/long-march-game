<template>
	<div class="page board-page">
		<div class="board-title"></div>
		<form name='form1' action="http://comments.people.com.cn/bbs_new/app/src/main/?action=parse_publish&op=ins&nid=406710&" method='post'>
			<input type="text" 
				id='yonghuming' 
				name='yonghuming' 
				placeholder="昵称"
				v-model='yonghuming'>
			
			<textarea name="content" 
				id="content" 
				placeholder="请输入留言信息"
				v-model='content'>					
			</textarea>

			<div class="btn btn-send-msg" @click="sendMsg"></div>
			
			<a class='check-msg' 
				href="http://comments.people.com.cn/bbs_new/app/src/main/?action=list&uid=406710"
				target="_blank">查看更多留言</a>
		</form>
	</div>
</template>

<script>
	export default {
		data(){
			return {
				yonghuming: '',
				content: ''
			}
		},

		methods: {
			sendMsg(){
				if (this.yonghuming.trim() == '') {
					alert('昵称不能为空')
				} else if (this.content.trim() == '') {
					alert('留言内容不能为空')
				} else {
					console.log('send msg')
					document.form1.submit()
				}
			}
		}
	}
</script>

<style scoped>
	.board-page {
		background-image: url(../assets/bg-pop.png);
	}

	.board-title {
		width: 608px;
		height: 58px;
		background-image: url(../assets/board-title.png);
		position: absolute;
		left: 150px;
		top: 100px;
	}

	#yonghuming {
		width: 764px;
		height: 40px;
		border: 1px solid #CCC;
		border-radius: 20px;
		box-sizing: border-box;
		padding-left: 20px;
		position: absolute;
		top: 180px;
		left: 100px;
	}

	#content {
		width: 764px;
		height: 184px;
		border: 1px solid #CCC;
		border-radius: 20px;
		box-sizing: border-box;
		padding: 20px;
		position: absolute;
		top: 240px;
		left: 100px;	
	}

	.btn-send-msg {
		width: 75px;
		height: 36px;
		background-image: url(../assets/btn-send-msg.png);
		position: absolute;
		bottom: 100px;
		right: 140px;
	}

	.check-msg {
		position: absolute;
		bottom: 100px;
		right: 240px;
		text-decoration: none;
		color:#cf2016;
	}
</style>