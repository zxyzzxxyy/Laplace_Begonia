<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;"  v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}"	 id="body">
		<!--顶部栏-->
		<titles style="width:100vw;" :username="username" :userheadimage="userheadimage" :admin="admin"
		:replymessagenum="replymessagenum"
		></titles>
	<view style="height:10%;width: 100%;opacity: 0;">
		
	</view>
	<view style="width: 60%;height:75vh;margin-top: 0vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;">
		<view style="margin-top: 3%;">
			<text style="font-size: 24px;font-weight: 700;">修改密码</text>
		</view>
		<view style="width:25%;margin:20px">
			<view>
				<text style="font-size: 18px;font-weight: 500;">旧密码</text>
			</view>
			  <u--input
			    placeholder="请输入旧密码"
			    clearable
				style="margin-top:20px;margin-bottom: 20px;"
				v-model="oldpassword"
			  ></u--input>
			<view>
				<text style="font-size: 18px;font-weight: 500;">新密码</text>
			</view>
			  <u--input
			    placeholder="请输入新密码"
			    clearable
				style=";margin-top:20px;margin-bottom: 40px;"
				v-model="password"
				password
			  ></u--input>
			  <view>
			  	<text style="font-size: 18px;font-weight: 500;">确认密码</text>
			  </view>
			    <u--input
			      placeholder="请再次输入新密码"
			      clearable
			  	style=";margin-top:20px;margin-bottom: 40px;"
			  	v-model="repassword"
			  	password
			    ></u--input>
			<view style="width: 100%;">
			  <u-button text="设置密码" type="primary" @click="changepassword()"></u-button>
			</view>
		</view>
	</view>
	<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import titles from './title.vue'
	import md5 from '@/pages/md5.js'
	export default {
		components:{
			titles
		},
		data() {
			return {
				username:"",
				userheadimage:"",
				userbackgroundimage:"",
				replymessagenum:0,
				oldpassword:"",
				password:"",
				repassword:"",
				admin:0,
				list:[
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "两次输入的密码不一致",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message:"密码长度小于七位",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message:"修改失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'success',
						icon: false,
						title: '成功主题',
						message:"修改成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},

				]
			}
		},
		onShow() {
			this.replymessagenum=getApp().globalData.replymessagenum;
			let that=this;
			if(getApp().globalData.cookie!='')
			uni.request({
				url:getApp().globalData.http+"/api/jugeuser",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie
				},
				success: (res) => {
					if(res.data.code==1002){
						uni.showToast({
							title:"其它设备登录",
							icon:"error"
						})
						getApp().globalData.username='';
						getApp().globalData.userheadimage='';
						getApp().globalData.userbackgroundimage='';
						getApp().globalData.cookie='';
						getApp().globalData.sign='';
						getApp().globalData.save='';
						getApp().globalData.bylike='';
						getApp().globalData.registertime='';
						getApp().globalData.userid='';
						getApp().globalData.admin=0;
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
						    key: "registertime",
						    data: getApp().globalData.registertime,
						    success: function () {
						    }
						});
						setTimeout(() => {
							uni.reLaunch({
								url:"index"
							})
						}
						, 1000)
						
					
					}
					}
				
			})
		},
		onLoad() {
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.admin=getApp().globalData.admin;
			if(getApp().globalData.cookie==''){
				uni.reLaunch({
					url:"index"
				})
			}
		},
		methods: {
			changepassword:function(){
				if(this.password!=this.repassword){
					this.$refs.uToast.show({
						...this.list[0],
						complete() {
							
						}
					})
					return;
				}
				else if(this.password.length<7){
					this.$refs.uToast.show({
						...this.list[1],
							complete() {
				
							}
						})
						return;
					}
				else if(this.oldpassword.length<7){
					this.$refs.uToast.show({
						...this.list[1],
						complete() {
							
						}
					})
					return;
				}
				var password=md5.hex_md5(this.password);
				var oldpassword=md5.hex_md5(this.oldpassword);
				uni.request({
					url:getApp().globalData.http+"/api/changepassword",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"password":password,
						"oldpassword":oldpassword,
				
						"userid":getApp().globalData.userid
					},
					success:(res)=>{
						console.log(res.data.code);
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.list[3],
								complete() {
									
							}
							})
					setTimeout(() => {  
							uni.reLaunch({
								url:"index"
					})
				}, 1010)		
				}else{
						this.$refs.uToast.show({
								...this.list[2],
								complete() {
									
								}
							})
						}
					},
					fail:(res)=>{
						this.$refs.uToast.show({
							...this.list[2],
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
		