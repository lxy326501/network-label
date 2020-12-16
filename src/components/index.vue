<template>
	<div>
		<div>
			<!-- 上传文件并预览 -->
			<form action='uploadFile.php' enctype="multipart/form-data" type='post'>
				<input type="file" @change="getFile($event)">　　
				<button @click="submit($event)">提交</button>
			</form>
			<img v-if="imageUrl" :src="imageUrl" class="uploadAvatar">
		</div>

		<div id="content" @contextmenu.prevent="$refs.ctxMenu.open($event)">
			<p id="test">
				河中鱼类离奇死亡，下游居民频染怪病，沿岸植物不断变异，是残留农药？还是生化攻击？
			</p>
			<p id='2'>河中鱼奇死亡，下游居民频染怪病，沿岸植物不断变异，是残留农药？还是生化攻击？</p>
		</div>
		<context-menu id="context-menu" ref="ctxMenu">
			<li @mousedown="doSomething()">标记</li>
			<li class="disabled">option 2</li>
			<li @click="cancel()">取消</li>
		</context-menu>
	</div>
</template>

<script>
	import contextMenu from 'vue-context-menu'
	export default {
		name: 'my-component',
		components: {
			contextMenu
		},
		data: function() {
			return {
				fileList: [],
				file: '',
				imageUrl: false,
			}
		},
		methods: {
			'doSomething': function() {
				/*
				 1.获取选中元素
				 2.获取选中的内容（txt）
				 2.获取id并替换html内容
				 */
				var select = document.getSelection()
				var selection = select.toString()
				var nodeId = select.anchorNode.parentElement.id
				if (nodeId) {
					var repace = document.getElementById(nodeId).innerHTML.replace(selection, '<a>' + selection + '</a>')
					document.getElementById(nodeId).innerHTML = repace
				}
			},
			'cancel': function() {
				var select = document.getSelection()
				var selection = select.toString()
				var nodeId = select.anchorNode.parentElement.id
				if (nodeId) {
					var repace = document.getElementById(nodeId).innerHTML.replace(selection, '<a>' + selection + '</a>')
					document.getElementById(nodeId).innerHTML = repace
				}
			},
			'getFile': function(evevt) {
				/*
				 1.获取文件信息后，调用FileReaderonload(读取完成时触发)
				 2.获取图片base64，将图片的url转化成base64(将图片的src绑定为DataURL)
				 3.将图片信息传给file
				 */
				var that = this;
				var reader = new FileReader();
				reader.readAsDataURL(event.target.files[0])
				reader.onload = function(e){
					that.imageUrl = e.target.result
				}
				that.file = event.target.files[0]
			},
			'submit': function(event) {
				/*
				 1.创建formData对象，
				 2.将获取的文件添加到formData中，并传给后台
				 */
				var that = this;
				let formData = new FormData();
				event.preventDefault(); //取消默认行为
				formData.append('file', that.file);
				console.log(formData.get("file"))
				
			},
		}
	}
</script>

<style>
	#content a {
		background-color: #00ffff;
	}

	.upload-demo {
		/* border-style: solid; */
		width: 16%;
		height: 8%;
		margin: auto;
		border-width: 2px;
	}

	.upload-demo input {
		/* border-style: solid; */
		cursor: none;
	}
	
	.uploadAvatar {
		margin: auto;
		width: 20%;
		height: 10%;
		padding: 10px;
	}
</style>
