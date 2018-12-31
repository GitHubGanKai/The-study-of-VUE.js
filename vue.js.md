# vue.js #
## day 01
<h3>1.1 mvvm的分层思想</h3>

![](imgs/01.MVC和MVVM的关系图解.png)

	var vm = new Vue({
		el:'#app',
		data:{
			msg:'这是个消息!',
			intervalId:null,//在data上定义一个定时器,
		}
	})

<h3>1.2 v-block 能解决插值表达式在网速慢的时候,页面渲染的时候的闪烁问题.</h3>

<b>用法:</b>
	<style>
		[v-block]{
			display:none;
		}
	</style>

	<p v-block>{{msg1}} 这里的内容不会被覆盖</p>
	<h4 v-text='msg2'>这里的内容会被覆盖</h4>  默认v-text是没有闪烁问题的.
	<h4 v-html='msg3'>这里如果写内容,也会被覆盖.</h4>

<h3>1.3 对于ES6中的this指向问题:</h3>

	一般写法:
	methods:{
		lang(){
			let _this = this
			setInterval(function () {
				console.log('打印日志...',_this.msg3);
				},400)
		}
	}

	ES6中的箭头函数写法:
	methods:{
		lang(){
			this.intervalId = setInterval(() => {
				console.log('打印日志...',this.msg);
				// 这个内部的this 就指向外部的this,箭头函数内部的this 始终和外部的this保持一致.
				},400)
		},
		stop(){
			clearInterval(this.intervalId);
		}
	}

<h3>1.4 事件修饰符</h3>

	<div @click='handleDiv'>
		<button @click.stop='handleClick'></button> // .stop阻止这个事件向外冒泡,这个是从里到外的事件
	</div>
	<div @click.capture='handleDiv'>
		<button @click='handleClick'></button> // .capture捕获里面的这个按钮的事件,这个是从外到里的事件
	</div>
	<div @click.self='handleDiv'>
		<button @click='handleClick'></button> // .self只有点击自己才会出发的事件
	</div>

	//使用.prevent阻止默认行为
	<a @click.prevent='handleLink'>点击去百度</a>