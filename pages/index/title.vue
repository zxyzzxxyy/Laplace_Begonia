<template>
	<view style="height:50px;padding-top: 5px;padding-bottom: 5px;;position: fixed;top:0;background-color: white;border-bottom: 1px solid #e6e6e6;display: flex;align-items: center;z-index: 10;width:100vw;flex-direction: row;
	
	">
	<!--icon-->
		<view style="width:40vw;display: flex;flex-direction: row;margin-left: 8vw;">
		<view style="display: flex;flex-direction: row;width:60%;overflow: hidden;margin-right:20px" @click="click,shouye()">
		<!--<u--image :fade="false" :showLoading="true" src="../../static/1.png" style="margin-left:0%;"  width="160" height="35" @click="click,shouye()" ></u--image>-->
		<view style="width: 550px;font-size:30px;color:#8bc863;height:35px;display: flex;align-items: center;justify-content: center;font-family: zhuzi;;"><text>{{'Laplace Begonia'}}</text>
		<image src="../../static/book.png" style="width:30px;height:30px;margin-left:5px;margin-top:5px"> </image>
		</view>
		<!--搜索框-->
		</view>
			<view style="margin-left: 0%;width:100%;border:1px solid #c0c4cc;height:35px;display: flex;align-items: center;border-radius: 5px;
			padding-left: 5px;
			">
			 <input
			    :placeholder="searchtuijian"
			    @confirm="search"
				v-model="key"
				style="color:#909399;border:none;margin-left: 5px;width:95%"
			  >
			  <u-icon name="search" color="#909399" size="28" style="margin-left: auto;margin-right: 5px;" @click="search()"></u-icon>
			</view>
		</view>
		<!--个人信息-->
		<view style=";height:100%;display: flex;align-items: center;max-width:60vw;margin-left:auto">
			<!--图标-->
			<view style="display:inline-flex;">
				<view style="width:78px;margin-right: 15px;" >
					<u--text prefixIcon="hourglass" iconStyle="font-size: 18px"  text="后台管理" type="info" @click="guanli()" v-if="admin==1"></u--text>
				</view>
				<view style="width:50px;margin-right: 15px;">
					<u--text prefixIcon="home" iconStyle="font-size: 18px" style="max-width:50px;margin-right:15px;" text="首页" type="info" @click="shouye()"></u--text>
				</view>
				<view style="margin-right:15px;width:50px;min-width:50px;max-width:50px;position: relative;display:flex;flex-direction:row" @click="message()" v-if="jugelogin()">
					<u--text prefixIcon="email" iconStyle="font-size: 18px" text="消息" style="min-width:50px;max-width:50px;margin-right: 15px;" type="info"></u--text>
				</view>
				<view style="width:50px;">
					<u--text prefixIcon="more-circle" iconStyle="font-size: 18px" text="发现" style="max-width:50px;margin-right: 15px;" type="info" @click="find()"></u--text>
				</view>
			<view style="width:50px;margin-right: 0px;margin-left:15px" v-if="juge1()">
			<u--text prefixIcon="grid" iconStyle="font-size: 18px" text="工具" style="max-width:50px;margin-right: 15px;" type="info" @click="tool()"></u--text>
				</view>
			</view>
			<!--分割线-->
			<view style="width:1px;height:85%;background-color: #e6e6e6;"></view>
			<!--头像-->
			<view  style="margin-left: 15px;">
				<u-button v-if="username==''" type="primary"  text="登录" @click="login()"></u-button>
			</view>
			<view style="margin-left: 15px;margin-right: 30px;width:40px;justify-content:center">
			<u-avatar mode="aspectFill" :src="userheadimage" v-if="username!=''&&userheadimage!=''"  @click="linkmyself()"></u-avatar>
			<u-avatar :text="username.substr(0,1)"  randomBgColor v-if="username!=''&&userheadimage==''"  @click="linkmyself()"></u-avatar>
			</view>
			<!--用户名-->
			<view style="display: flex;flex-direction: row;"  v-if="username!=''">
				<view style="margin-right: 15px;">
			<u--text  type="info"   text="个人中心" @click="linkmyself()"></u--text>
				</view>
				<view style="margin-right: 15px;">
			<u--text  text="背景图" type="info" @click="bg()" style=""></u--text>
				</view>
				<view style="margin-right: 30px;">
			<u--text  text="退出登录" type="info" @click="loginout()" style=""></u--text>
				</view>
			
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				key:"",
				op:1,
				//icon:"../../static/icon.png",
				icon:"../../static/1.jpg",
				searchtuijian:"",
			}
		},
		props:{
			username:{
				type:String,
				default:"",
				required:true
			},
			userheadimage:{
				type:String,
				default:"",
				required:true
			},
			admin:{
				type:Number,
				default:0,
				required:true
			},
			replymessageshow:{
				type:Boolean,
				default:false,
				require:true
			}
		},
		
		mounted() {
			
			uni.request({
				url:getApp().globalData.http+"/api/gethottagschart",
				method:"POST",
				data:{},
				success: (res) => {
					{
						var sub=Math.floor(Math.random() * (res.data.hottagschart.length - 0)) + 0;
						this.searchtuijian=res.data.hottagschart[sub].name;
					}
											
				}
			})
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			
		},
		methods: {
			tool:function(){
				uni.navigateTo({
					url:"./tool"
				})
			},
			juge:function(){
				
				if(this.replymessageshow==true)return true;return false;
			},
			juge1:function(){
				if(getApp().globalData.cookie!='')return true;return false;
			},
			message:function(){
				
				if(getApp().globalData.cookie=='')return;
				getApp().globalData.replymessagenum=0;
				this.juge();
				uni.navigateTo({
					url:"./message"
				})
			},
			guanli:function(){
				uni.navigateTo({
					url:"../admin/admin"
				})
			},
			search:function(){
				
				if(this.key=="")
				uni.navigateTo({
					url:"search?key="+this.searchtuijian
				})
				else
				uni.navigateTo({
					url:"search?key="+this.key
				})
				this.key="";
			},
			changeop:function(){
				getApp().globalData.op=this.op;
				uni.setStorage({
				    key: "op",
				    data: getApp().globalData.op,
				    success: function () {
				    }
				});
			},
			shouye:function(){
				uni.navigateTo({
					url:"index"
				})
			},
			find:function(){
				uni.navigateTo({
					url:"find"
				})
			},
			linkmyself:function(){
				uni.navigateTo({
					url:"myself"
				})
			},
			jugelogin(){
				if(getApp().globalData.cookie!='')return true;return false;
			},
			bg:function(){
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: function(res) {
						////console.log(res.tempFilePaths);
						uni.uploadFile({
						    url:"https://www.txtz.club:8808/api/upload",		//post请求的地址
						    filePath:res.tempFilePaths[0],
						    name:'file',	
						    success: (res) => {
									 res.data = JSON.parse(res.data)
									
								uni.request({
									url:getApp().globalData.http+"/api/setuserbackgroundimage",
									method:"POST",
									data:{
										"userbackgroundimage":res.data.location,
										"userid":getApp().globalData.userid,
										"cookie":getApp().globalData.cookie
									},
									success: (res1) => {
										if(res1.data.code==1001){
											getApp().globalData.userbackgroundimage=res.data.location;
											this.userbackgroundimage=getApp().globalData.userbackgroundimage;
											uni.setStorage({
											    key: "userbackgroundimage",
											    data: getApp().globalData.userbackgroundimage,
											    success: function () {
											    }
											});
											let routes = getCurrentPages(); // 获取当前打开过的页面路由数组
											 
											let curRoute = routes[routes.length - 1].route
											
											uni.reLaunch({
												url:curRoute.substr(12,curRoute.length)
											})
										}
										else{
											this.$refs.uToast.show({
												...this.list[3],
												complete() {
													return;
												}
											})
										}
									},
									fail:(res)=>{
										this.$refs.uToast.show({
											...this.list[3],
											complete() {
												return;
											}
										})
									}
								})
							}
						})
					}
				});
			},
			login:function(){
				uni.navigateTo({
					url:"login"
				})
			},
			loginout(){
				getApp().globalData.username='';
				getApp().globalData.userheadimage='';
				getApp().globalData.userbackgroundimage='';
				getApp().globalData.cookie='';
				getApp().globalData.sign='';
				getApp().globalData.save='';
				getApp().globalData.bylike='';
				getApp().globalData.registertime='';
				getApp().globalData.userid='';
				getApp().globalData.admin='';
				getApp().globalData.attention=[],
				getApp().globalData.mylike=[],
				getApp().globalData.firstopen=[],
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
				uni.reLaunch({
					url:"index"
				})
			}
		}
	}
</script>

<style>

</style>
