<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" username="" userheadimage="" :admin="0"></titles>
		<!--center-->
		<view style="height:10vh;width: 100vw;opacity: 0;">
			
		</view>
		<view style="width: 60vw;height:75vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;">
			<view style="margin-top: 3%;">
				<text style="font-size: 24px;font-weight: 700;">注册</text>
			</view>
			<view style="width:25%;margin:20px">

				<view>
					<text style="font-size: 18px;font-weight: 500;">邮箱</text>
				</view>
				  <u--input
				    placeholder="请输入邮箱"
				    clearable
					style="margin-top:20px;margin-bottom: 20px;"
					v-model="email"
				  ></u--input>
				<view>
					<text style="font-size: 18px;font-weight: 500;">密码</text>
				</view>
				  <u--input
				    placeholder="请输入密码"
				    clearable
					style=";margin-top:20px;margin-bottom: 20px;"
					v-model="password"
					password
				  ></u--input>
				<view>
					<text style="font-size: 18px;font-weight: 500;">确认密码</text>
				</view>
				<u--input
					placeholder="请确认密码"
					clearable
					style=";margin-top:20px;margin-bottom: 40px;"
					v-model="repassword"
					password
				></u--input>
				<view style="width: 100%;">
				  <u-button text="注册" type="primary" @click="register()"></u-button>
				</view>
				<view style="margin-top: 20px;display: flex;align-items: center;justify-content: center;flex-direction: row;">
					<text style="font-size: 16px;">已有账号?</text><text style="font-size: 16px;color:#99dc6d" @click="login()">去登录</text>
				</view>
			</view>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import md5 from "@/pages/md5.js";
	import titles from './title.vue'
	export default {
		components:{
			titles
		},
		data() {
			return {
				email:"",
				password:"",
				repassword:"",
				username:"",
				key:"",
				op:1,
				backgroundimage:"",
				icon:"../../static/icon.png",
				list:[
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "邮箱格式错误",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "密码长度小于7位",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "两次输入的密码不一致",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "邮箱已被注册",
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
											title: '成功主题',
											message: "注册成功,已向你的邮箱发送了激活邮件,请尽快激活",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
											url:"index"
					},
				]
			}
		},
		methods: {
			login:function(){
				uni.navigateTo({
					url:"login"
				})
			},
			register:function(){
				if(!uni.$u.test.email(this.email)){
					this.$refs.uToast.show({
						...this.list[0],
						complete() {
							
						}
					})
					return;
				}
				if(this.password.length<7){
					this.$refs.uToast.show({
						...this.list[1],
						complete() {
							return;
						}
					})
					return;
				}
				if(this.password!=this.repassword){
					this.$refs.uToast.show({
						...this.list[2],
						complete() {
							return;
						}
					})
					return;
				}
				let that=this;
				var password=md5.hex_md5(this.password);
				uni.request({
					url:getApp().globalData.http+"/api/register",
					method:"POST",
					data:{
						"password":password,
						"email":this.email
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
						else if(res.data.code==1002){
							this.$refs.uToast.show({
								...this.list[3],
								complete() {
									return;
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.list[4],
								complete() {
									return;
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
								return;
							}
						})
					}
				})
			},
			
		
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
