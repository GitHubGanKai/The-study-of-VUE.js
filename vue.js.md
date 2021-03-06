# vue.js

## 2019年1月1日00:24:49  001

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
    	<button @click.stop='handleClick'></button> // .stop阻止这个事件向外冒泡,这个是从里到外的事件.
    </div>
    <div @click.capture='handleDiv'>
    	<button @click='handleClick'></button> // .capture捕获里面的这个按钮的事件,这个是从外到里的事件
    </div>
    <div @click.self='handleDiv'>
    	<button @click='handleClick'></button> // .self只有点击自己才会出发的事件
    </div>

**.stop和.self的区别:**

.stop会阻止从本身开始,一直向外冒泡的事件,但是.self只会阻止本身事件,不会自身以外的事件.

    //使用.prevent阻止默认行为
    <a @click.prevent='handleLink'>点击去百度</a>
    //使用.once只触发一次事件处理函数,会阻止一次默认行为,第二次就是回复原样,这两个事件之间,没有联系.
    <a @click.prevent.once='handleLink'>点击去百度查询</a>
**对于 eval()方法,将字符串解析成表达式**

	var codeStr = 'parseInt(this.val1)' + this.pot + 'parseInt(this.val2)'
	this.result = eval(codeStr);
<h4>简单的计算器</h4>

	<!DOCTYPE html>
	<html lang="en">

	<head>
	  <meta charset="UTF-8">
	  <meta name="viewport" content="width=device-width, initial-scale=1.0">
	  <meta http-equiv="X-UA-Compatible" content="ie=edge">
	  <title>Document</title>
	  <script src="./lib/vue-2.4.0.js"></script>
	</head>

	<body>
	  <div id="app">
	    <input type="text" v-model="n1">

	    <select v-model="opt">
	      <option value="+">+</option>
	      <option value="-">-</option>
	      <option value="*">*</option>
	      <option value="/">/</option>
	    </select>

	    <input type="text" v-model="n2">

	    <input type="button" value="=" @click="calc">

	    <input type="text" v-model="result">
	  </div>

	  <script>
	    // 创建 Vue 实例，得到 ViewModel
	    var vm = new Vue({
	      el: '#app',
	      data: {
	        n1: 0,
	        n2: 0,
	        result: 0,
	        opt: '+'
	      },
	      methods: {
	        calc() { // 计算器算数的方法  
	          // 逻辑：
	          /* switch (this.opt) {
	            case '+':
	              this.result = parseInt(this.n1) + parseInt(this.n2)
	              break;
	            case '-':
	              this.result = parseInt(this.n1) - parseInt(this.n2)
	              break;
	            case '*':
	              this.result = parseInt(this.n1) * parseInt(this.n2)
	              break;
	            case '/':
	              this.result = parseInt(this.n1) / parseInt(this.n2)
	              break;
	          } */

	          // 注意：这是投机取巧的方式，正式开发中，尽量少用
	          var codeStr = 'parseInt(this.n1) ' + this.opt + ' parseInt(this.n2)'
	          this.result = eval(codeStr)
	        }
	      }
	    });
	  </script>
	</body>

	</html>

<h3>通过设置绑定class 样式</h3>

	在数组中使用三元表达式:
	<h1 :class="['thin', 'italic', flag?'active':'']">这是一个很大很大的H1</h1>
	也可以传入一个对象,在数组中使用 对象来代替三元表达式，提高代码的可读性
	<h1 :class="['thin', 'italic', {'active':flag} ]">这是一个很大很大的H1</h1>

<h3>通过设置绑定style 样式</h3>

	 <h1 :style="[ styleObj1, styleObj2 ]">这是一个h1</h1>
	 <script>
	 // 创建 Vue 实例，得到 ViewModel
	 var vm = new Vue({
		 el: '#app',
		 data: {
			 styleObj1: { color: 'red', 'font-weight': 200 },
			 styleObj2: { 'font-style': 'italic' }
		 },
		 methods: {}
	 });
 	</script>
<h3>v-for 循环对象</h3>

	// 创建 Vue 实例，得到 ViewModel
	var vm = new Vue({
	 el: '#app',
	 data: {
		 user: {
			 id: 1,
			 name: '张三',
			 gender: '男'
		 }
	 },
	 methods: {}
	});
<b>注意：</b><i>在遍历对象身上的键值对的时候,除了有value key,在第三个位置还有一个索引 </i>

	 <p v-for="(val, key, i) in user">值是： {{ val }} --- 键是： {{key}} -- 索引： {{i}}</p>

<b>注意：</b><i>如果使用 v-for 迭代数字的话，前面的 count 值从 1 开始 </i>

	<p v-for="count in 10">这是第 {{ count }} 次循环</p>
<font color=red size=72>
</font>

<h4>v-for 中的 key </h4>

<div id="app">

	<div>
		<label>Id:
			<input type="text" v-model="id">
		</label>

		<label>Name:
			<input type="text" v-model="name">
		</label>

		<input type="button" value="添加" @click="add">
	</div>

	注意： v-for 循环的时候，key 属性只能使用 number获取string
	注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值
	 在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须 在使用 v-for 的同时，指定 唯一的 字符串/数字 类型 :key 值
	<p v-for="item in list" :key="item.id">
		<input type="checkbox">{{item.id}} --- {{item.name}}
	</p>
	</div>

	// 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        list: [
          { id: 1, name: '张三' },
          { id: 2, name: '李四' },
          { id: 3, name: '王五' },
          { id: 4, name: '翟柳' },
          { id: 5, name: '邰南奇' }
        ]
      },
      methods: {
        add() { // 添加方法
          this.list.unshift({ id: this.id, name: this.name })
        }
      }
    });

 v-if 的特点：每次都会重新删除或创建元素, v-if 有较高的切换性能消耗;
 v-show 的特点： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式,v-show 有较高的初始渲染消耗;
 如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show;
 如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if.

<h2>2019年1月1日10:46:45</h2>
<h3>1.1 一些小方法</h3>
1. 在数组中的 some() 方法中,如果返回 true ,那么,这个方法会立即终止这数组的后续循环.
2. 也可以用
  `let index = this.array.findIndex(item => {
    if(item.it == id){
      return true;
    }
  })`来找到数组中的索引.
3. 在指令 v-for中 , 除了可以循环数组,还可以循环方法的返回值,指令可以拿到vue实例对象data中所有的数据.
4. 之前， v-for 中的数据，都是直接从 data 上的list中直接渲染过来的,现在， 我们自定义了一个 search 方法，同时，把 所有的关键字，通过传参的形式，传递给了 search 方法,在 search 方法内部，通过 执行 for 循环， 把所有符合 搜索关键字的数据，保存到 一个新数组中，返回

        <tr v-for="item in search(keywords)" :key="item.id">
            <td>{{ item.id }}</td>
            <td v-text="item.name"></td>
            <td>{{ item.ctime | dateFormat() }}</td>
            <td>
              <a href="" @click.prevent="del(item.id)">删除</a>
            </td>
          </tr>
5. jQuery中有个contains 选择器,选择内容中所有包含 "is" 的 `<p>` 元素$("p:contains(is)");$(":contains(text)")选择内容中所有包含 "text" 的元素

6. ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串') //  如果包含，则返回 true ，否则返回 false.


<h3>过滤器</h3>

  -全局过滤器,

  -私有过滤器.

  -思考:函数中,形参的默认值是怎么样设置的??

  -padStart ES6中的,新语法.为字符串添加补充数字.常用在时间的格式.

  -@keyup.enter='add()' ,键盘抬起事件.

  -自定义全局按键修饰符  Vue.config.keyCodes.f2 = 113

  -自定义全局指令  Vue.directive('focus')   // 例如:v-color="'red'";
	
	2019年1月6日17:47:03
