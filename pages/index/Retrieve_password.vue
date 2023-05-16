<template>
		<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;"  v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}"	 id="body">
		<titles style="width:100vw;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessagenum="replymessagenum"></titles>
	<view style="height:10%;width: 100%;opacity: 0;">
		
	</view>
	<view style="width: 60%;height:75vh;margin-top: 0vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;" v-if="type==0">
		<view style="margin-top: 3%;">
			<text style="font-size: 24px;font-weight: 700;">找回密码</text>
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
			<view style="width: 100%;">
			  <u-button text="发送验证码" type="primary" @click="sendemail()"></u-button>
			</view>
		</view>
	</view>
	<!--************************************************************************************-->
	<view style="width: 60%;height:75vh;margin-top: 0vh;background-color: white;margin: 0 auto;display: flex;align-items: center;flex-direction: column;" v-if="type==1">
		<view style="margin-top: 3%;">
			<text style="font-size: 24px;font-weight: 700;">找回密码</text>
		</view>
		<view style="width:25%;margin:20px">
			<view>
				<text style="font-size: 18px;font-weight: 500;">验证码</text>
			</view>
			  <u--input
			    placeholder="请输入邮箱验证码"
			    clearable
				style="margin-top:20px;margin-bottom: 20px;"
				v-model="findpassword"
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
			  <u-button text="修改密码" type="primary" @click="changepassword_byemail()"></u-button>
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
				findpassword:"",
				password:"",
				repassword:"",
				type:0,
				username:"",
				userheadimage:"",
				userbackgroundimage:"",
				replymessagenum:0,
				email:"",
				admin:0,
				juge1:0,
				list:[
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "邮箱未注册",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},					{
											type: 'error',
											icon: false,
											title: '失败主题',
											message: "邮箱格式错误",
											iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
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
					{
						type: 'success',
						icon: false,
						title: '成功主题',
						message:"邮件已发送",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
				]
			}
		},
		methods: {
			changepassword_byemail(){
				if(this.password!=this.repassword){
					this.$refs.uToast.show({
						...this.list[2],
						complete() {
							
						}
					})
					return;
				}
				else if(this.password.length<7){
					this.$refs.uToast.show({
						...this.list[3],
						complete() {
							
						}
					})
					return;
				}
				var password=md5.hex_md5(this.password);
				uni.request({
					url:getApp().globalData.http+"/api/changepassword_byemail",
					method:"POST",
					data:{
						"pass":this.password,
						"password":password,
						"findpassword":this.findpassword,
						"email":this.email
					},
					success:(res)=>{
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.list[5],
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
								...this.list[4],
								complete() {
									
								}
							})
						}
					},fail:(res)=>{
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
								
							}
						})
					}
				})
			},
			sendemail:function(){
				if(!uni.$u.test.email(this.email)){
					this.$refs.uToast.show({
						...this.list[1],
						complete() {
							
						}
					})
					return;
				}
				if(this.juge1==1)return;
				this.juge=1;
				uni.request({
					url:getApp().globalData.http+"/api/sendemail",
					method:"POST",
					data:{
						"email":this.email
					},
					success:(res)=>{
						if(res.data.code==1001){
							this.type=1;
							this.$refs.uToast.show({
								...this.list[6],
								complete() {
									
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.list[0],
								complete() {
									
								}
							})
						}
					}
				})
			}
		}
	}
</script>

<style>

</style>
