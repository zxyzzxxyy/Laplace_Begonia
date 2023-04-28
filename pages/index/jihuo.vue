<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" username=" " userheadimage=" " :admin="0"></titles>
		<!--center-->
		<view style="height:10%;width: 100%;opacity: 0;">
			
		</view>
		<view style="width: 60vw;height:75vh;margin-top: 10vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;">
			<view style="margin-top: 3%;">
				<text style="font-size: 24px;font-weight: 700;">激活</text>
			</view>
			<view style="width:25%;margin:20px">

				<view>
					<text style="font-size: 18px;font-weight: 500;">用户名</text>
				</view>
				  <u--input
				    placeholder="请设置用户名"
				    clearable
					style="margin-top:20px;margin-bottom: 20px;"
					v-model="username"
					maxlength=10
				  ></u--input>

				<view style="width: 100%;">
				  <u-button text="激活" type="primary" @click="jihuo()"></u-button>
				</view>
			</view>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import titles from './title.vue'
	export default {
		components:{
			titles
		},
		data() {
			return {
				username:"",
				key:"",
				op:1,
				cookie:"",
				icon:"../../static/icon.png",
				list:[
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "用户名不能为空",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "用户名已被使用",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: 'error',
											message: "error",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "error",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "注册失败",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'success',
											icon: false,
											title: '激活成功',
											message: "激活成功",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
											url:"index"
					},
				]
			}
		},
		methods: {
			jihuo:function(){
				if(this.username==''){
					this.$refs.uToast.show({
						...this.list[0],
						complete() {
							return;
						}
					})
				}
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/jihuo",
					method:"POST",
					data:{
						"cookie":this.cookie,
						"username":this.username
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.list[5],
								complete() {
									that.zhuye();
									return;
								}
							})
						}
						else if(res.data.code==1003){
							this.$refs.uToast.show({
								...this.list[1],
								complete() {
									return;
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.list[2],
								complete() {
									return;
								}
							})
						}
					},
					fail: (res) => {
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
								return;
							}
						})
					}
				})
			},

		
		},
		onLoad(res) {
			//console.log(res.cookie);
			this.cookie=res.cookie;
		},
		onShow() {
			if(getApp().globalData.cookie!=''){
				uni.navigateTo({
					url:"index"
				})
				return;
			}
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.op=getApp().globalData.op;
		}
	}
</script>

<style>

</style>
