<template>
	<view style="font-size: 16px;overflow-x: hidden;overflow-y:auto">
		<view v-if="admin==1">
			<view style="width:100vw;height:8vh;background-color: #8bc863;display: flex;flex-direction: row;
			align-items: center;">
				<view style="width: 10rem;margin-left: 2rem;margin-right: 2rem;">
					
				</view>
				<view style="margin-right: 2rem;color:#f4f4f5" @click="index()">
					<text>主页</text>
				</view>
				<view :class="{'t1':show==3,'t2':show!=3}" @click="show=3,title('统计')" style="margin-right: 2rem;">
					<text>统计</text>
				</view>
				<view :class="{'t1':show==1,'t2':show!=1}"  @click="show=1,title('用户管理')" style="margin-right: 2rem;">
					<text>用户管理</text>
				</view>
				<view :class="{'t1':show==2,'t2':show!=2}" @click="show=2,title('主题管理')">
					<text>主题管理</text>
				</view>
				
			</view>
		<user style="width:100vw;height:92vh;" v-if="show==1"></user>
		<theme style="width:100vw;height:92vh;" v-if="show==2"></theme>
		<Statistics style="width:100vw;height:92vh;" v-if="show==3"></Statistics>
		</view>
	</view>
</template>

<script>
	import user from './user.vue'
	import theme from './theme.vue'
	import Statistics from './Statistics.vue'
	export default {
		components:{
			user,theme,Statistics
		},
		data() {
			return {
				show:3,
				show1:0,
				admin:0,
			}
		},
		onLoad() {
			uni.request({
				url:getApp().globalData.http+"/api/jugeadmin",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie
				},
				success: (res) => {
					if(res.data.code==1001)
						this.admin=1;
					else
						this.index();
				},
				fail:(res)=> {
					this.index();
				}
			})
		},
		methods: {
			title:function(e){
				uni.setNavigationBarTitle({
						title:e
				})
			},
			index:function(){
				uni.reLaunch({
					url:"../index/index"
				})
			}
		}
	}
</script>

<style>
.t1{
	color:#f4f4f5
}
.t2{
	color:white
}
body{
	overflow: auto;
}
</style>
