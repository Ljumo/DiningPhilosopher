<template>
	<view>
	    <text class="circle"></text>
		<view class="food-container">
			<image src="../../static/food.png" class="food"></image>
		</view>
		<view class="chopsticks-container" id="cho1">
			<image src="../../static/chopsticks.png" class="chopsticks"></image>
		</view>
		<view class="chopsticks-container" id="cho2">
			<image src="../../static/chopsticks.png" class="chopsticks"></image>
		</view>
		<view class="chopsticks-container" id="cho3">
			<image src="../../static/chopsticks.png" class="chopsticks"></image>
		</view>
		<view class="chopsticks-container" id="cho4">
			<image src="../../static/chopsticks.png" class="chopsticks"></image>
		</view>
		<view class="chopsticks-container" id="cho5">
			<image src="../../static/chopsticks.png" class="chopsticks"></image>
		</view>
		<view class="people-container" id="peo1">
			<text class="behavior">{{behavior[0]}}...</text>
			<image src="../../static/people.png" class="people"></image>
		</view>
		<view  class="people-container" id="peo2">
			<text class="behavior">{{behavior[1]}}...</text>
			<image src="../../static/people.png" class="people"></image>
		</view>
		<view  class="people-container" id="peo3">
			<text class="behavior">{{behavior[4]}}...</text>
			<image src="../../static/people.png" class="people"></image>
		</view>
		<view  class="people-container" id="peo4">
			<text class="behavior behavior-right">{{behavior[3]}}...</text>
			<image src="../../static/people.png" class="people"></image>
		</view>
		<view  class="people-container" id="peo5">
			<text class="behavior behavior-left">{{behavior[2]}}...</text>
			<image src="../../static/people.png" class="people"></image>
		</view>
		<view class="foot">
			©️copyright by {{writer}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				writer: "廖晓伟",
				behavior: [
					"思考中",
					"思考中",
					"思考中",
					"思考中",
					"思考中",
				],
				used: [
					false,
					false,
					false,
					false,
					false
				],
				
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			Philosopher : class {
				constructor(id,that) {					
				    this.id = id;
					this.that = that;
				}
				async run(){
					while(true){
						await this.thinking();
						await this.takefork();
						await this.eating();
						await this.putfork();
					}
				}
				async thinking(){
					console.log("哲学家"+(this.id+1)+"思考")
					await this.delay(parseInt(Math.random()*(6000-3000+1)+3000,10));
				}
				async eating(){
					console.log("哲学家"+(this.id+1)+"吃饭")
					await this.delay(parseInt(Math.random()*(6000-3000+1)+3000,10))		
				}
				async takefork(){
					console.log("哲学家"+(this.id+1)+"拿餐具")		
					while(this.that.used[this.id] || this.that.used[(this.id + 1) % 5]){
						await this.wait(0);
					}
					this.that.used[this.id] = true;
					this.that.used[(this.id + 1) % 5] = true;
					this.that.behavior[this.id] = "吃饭中";
					this.that.$forceUpdate();			//强制更新视图解决数组数据更新不渲染
					await this.delay(0);
				}
				async putfork(){
					console.log("哲学家"+(this.id+1)+"放餐具")
					this.that.used[this.id] = false;
					this.that.used[(this.id + 1) % 5] = false;
					this.that.behavior[this.id] = "思考中";
					this.that.$forceUpdate();			//强制更新视图解决数组数据更新不渲染
					await this.delay(0);
				}
				delay(t){
					return new Promise(resolve => setTimeout(resolve,t));
				}
				wait(t){
					this.that.behavior[this.id] = "等待中"
					this.that.$forceUpdate();
					return new Promise(resolve => setTimeout(resolve,t));
				}
			},
			init(){
				let a = new Array();
				// a[0] = new this.Philosopher(0,this);
				// a[1] = new this.Philosopher(1,this);
				// a[2] = new this.Philosopher(2,this);
				// a[3] = new this.Philosopher(3,this);
				// a[4] = new this.Philosopher(4,this);
				for(let i = 0;i<5;i++){
					a[i] = new this.Philosopher(i,this);
					a[i].run();
				}
			},
		}
	}
</script>

<style>
	.circle {
		position: absolute;
		top: 50%;
		left: 50%;
		margin-top: -150rpx;
		margin-left: -150rpx;
		display: inline-block;
		width: 300rpx;
		height: 300rpx;
		border-radius: 50%;
		border: #000 5rpx solid;
	}
	.food-container {
		position: absolute;
		display: inline-block;	
		top: 50%;
		left: 50%;
		margin-top: -50rpx;
		margin-left: -50rpx;
	}
	.food {
		width: 100rpx;
		height: 100rpx;
	}
	.people-container,
	.chopsticks-container {
		display: inline-block;
		position: absolute;
		top: 50%;
		left: 50%;
	}
	.chopsticks {
		width: 50rpx;
		height: 50rpx;
	}
	#cho1 {
		margin-top: -110rpx;
		margin-left: -80rpx;
		transform: rotate(-40deg);
	}
	#cho2 {
		margin-top: -110rpx;
		margin-left: 40rpx;
		transform: rotate(40deg);
	}
	#cho3 {
		margin-top: 11rpx;
		margin-left: 75rpx;
		transform: rotate(-65deg);
	}
	#cho4 {
		margin-top: 6rpx;
		margin-left: -126rpx;
		transform: rotate(65deg);
	}
	#cho5 {
		margin-top: 75rpx;
		margin-left: -25rpx;
		transform: rotate(180deg);
	}
	.people {
		width: 80rpx;
		height: 80rpx;
	}
	.behavior {
		position: absolute;
		position: inline-block;
		margin-top: -50rpx;
		width: 150rpx;
		
	}
	.behavior-left {
		margin-top: 90rpx;
	}
	.behavior-right {
		margin-top: 90rpx;
	}
	#peo1 {
		margin-top: -260rpx;
		margin-left: -40rpx;
	}
	#peo2 {
		margin-top: -100rpx;
		margin-left: -260rpx;	
	}
	#peo3 {
		margin-top: -100rpx;
		margin-left: 180rpx;
	}
	#peo4 {
		margin-top: 135rpx;
		margin-left: 110rpx;
	}
	#peo5 {
		margin-top: 135rpx;
		margin-left: -190rpx;
	}
	.foot {
		color: #555555;
		position: absolute;
		bottom: 60rpx;
		left: 50%;
		width: 350rpx;
		margin-left: -175rpx;
	}
</style>
