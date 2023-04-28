<template>
	<view style="background-color: #f8f8f8;;width:100%;height:100vh;background-size: cover;background-attachment: fixed;" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" username="" userheadimage="" :admin="0"></titles>
		<view style="height:10%;width: 100%;opacity: 0;">
			
		</view>
		<view style="width: 60%;height:75vh;margin-top: 0vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;">
			<view style="margin-top: 3%;">
				<text style="font-size: 24px;font-weight: 700;">登录</text>
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
					style=";margin-top:20px;margin-bottom: 40px;"
					v-model="password"
					password
					@confirm="login()"
				  ></u--input>
				<view style="width: 100%;">
				  <u-button text="登录" type="primary" @click="login()"></u-button>
				</view>
				<view style="margin-top: 20px;display: flex;align-items: center;justify-content: center;flex-direction: row;">
					<text style="font-size: 16px;">没有账号?</text><text style="font-size: 16px;color:#99dc6d" @click="register()">去注册</text>
				</view>
				<view style="margin-top: 5px;display: flex;align-items: center;justify-content: center;flex-direction: row;">
					<text style="font-size: 16px;color:#99dc6d" @click="Retrieve_password()">找回密码</text>
				</view>
			</view>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import md5 from '@/pages/md5.js'
	import titles from './title.vue'
	export default {
		components:{
			titles
		},
		data() {
			return {
				email:"",
				password:"",
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
											message: "邮箱或密码错误",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "邮箱未激活",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "登陆失败",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
											type: 'success',
											icon: false,
											title: '成功主题',
											message: "登陆成功",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
											url:"index"
					},
				]
			}
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
					//////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.op=getApp().globalData.op;
		},
		methods: {
			Retrieve_password:function(){
				uni.navigateTo({
					url:"Retrieve_password"
				})
			},
			register:function(){
				uni.navigateTo({
					url:"register"
				})
			},
			login:function(){
				if(!uni.$u.test.email(this.email)){
					this.$refs.uToast.show({
						...this.list[0],
						complete() {
							return;
						}
					})
				}
				if(this.password.length<7){
					this.$refs.uToast.show({
						...this.list[1],
						complete() {
							return;
						}
					})
				}
				var password=md5.hex_md5(this.password);
				uni.request({
					url:getApp().globalData.http+"/api/login",
					method:"POST",
					data:{
						"email":this.email,
						"password":password
					},
					success: (res) => {
						////console.log(res);	
						if(res.data.code==1001){
							getApp().globalData.username=res.data.data.username;
							getApp().globalData.userheadimage=res.data.data.userheadimage;
							getApp().globalData.userbackgroundimage=res.data.data.userbackgroundimage;
							getApp().globalData.cookie=res.data.data.cookie;
							getApp().globalData.sign=res.data.data.sign;
							getApp().globalData.save=res.data.data.save;
							getApp().globalData.bylike=res.data.data.bylike;
							getApp().globalData.registertime=res.data.data.registertime;
							getApp().globalData.userid=res.data.data.userid;
							getApp().globalData.admin=res.data.data.admin;
							//console.log(getApp().globalData.admin);
							uni.request({
								url:getApp().globalData.http+"/api/user_visit",
								method:"POST",
								data:{
									"userid":getApp().globalData.userid
								},
								success: (res) => {;
								}
							})
							uni.setStorage({
							    key: "admin",
							    data: getApp().globalData.admin,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "userid",
							    data: getApp().globalData.userid,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "username",
							    data: getApp().globalData.username,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "userheadimage",
							    data: getApp().globalData.userheadimage,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "userbackgroundimage",
							    data: getApp().globalData.userbackgroundimage,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "cookie",
							    data: getApp().globalData.cookie,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "save",
							    data: getApp().globalData.save,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "bylike",
							    data: getApp().globalData.bylike,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "ban",
							    data: getApp().globalData.ban,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "registertime",
							    data: getApp().globalData.registertime,
							    success: function () {
							    }
							});
							this.$refs.uToast.show({
								...this.list[5],
								complete() {
									uni.reLaunch({
										url:"index"
									})
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.list[2],
								complete() {
									
								}
							})
						}
					},
					fail: (res) => {
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
								
							}
						})
					}
				})

			}
		}
	}
</script>

<style>

</style>
