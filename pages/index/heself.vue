<template>
<view style="background-color: #f8f8f8;height:100vh;width:100%;background-size: cover;background-attachment: fixed;overflow-y: auto;overflow-x: auto;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}">
		<!--top-->
		<!--顶部栏-->
		<u-transition mode="fade-down" show>
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="myusername" :userheadimage="myuserheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		</u-transition>
		<scroll-view @scroll="scroll" :scroll-top="scroll_top"  :scroll-with-animation="true"  style="height:100vh;width:100%;background-size: cover;background-attachment: fixed;overflow-y: auto;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" v-if="username!=''" scroll-y @scrolltolower="getmoretheme">
		<view style="height:10%;width:100%;opacity: 0;">	
		</view>
		<u-transition mode="fade-down" show>
		<view style="width:60%;height:auto;background-color: white;margin: 0 auto;border-radius: 15px;overflow: hidden;" :style="{'opacity':op}">
			<view style="width:100%;height:150px">
				<image :src="toutu" style="width:100%;height:100%"  mode="aspectFill" >
					
				<image>
			</view>
			<view style="display: flex;flex-direction: row;">
				
				<view style="margin-top: -50px;z-index: 2;">
				<u-avatar mode="aspectFill" size="110" :src="userheadimage" v-if="username!=''&&userheadimage!=null" style="margin-top: 0px;margin-left: 50px;" ></u-avatar>
				<u-avatar  font-size="60" size="110" :text="username.substr(0,1)"   v-if="username!=''&&userheadimage==null" style="margin-top: 0px;margin-left: 50px;"></u-avatar>
				</view>
				<view style="height:100px;margin-top: -50px;display: flex;flex-direction: column;width:80%;justify-content: center;margin-left: 10px;
				padding-bottom: 20px;"
				>
					<text style="z-index: 2;font-size: 30px;font-weight: 700;">{{username}}</text>
					<view style="display: flex;flex-direction: row;align-items: center;">
					<u--text :text="sign" type="info" v-if="sign!=null"></u--text>
					<u--text text="暂无签名" type="info" v-if="sign==null"></u--text>
					<view style="width:180px;margin-left: auto;margin-top: 30px;">
					<view style="display: flex;flex-direction: row;width:180px">
					<view style="width:75px;height:38px;background-color: #99dc6d;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;"  v-show="!jugeattention()&&jugelogin()" @click="addattention()">关注</view>
					<view style="width:73px;height:36px;background-color: #ecf5ff;border: 1px solid #99dc6d;color:#99dc6d;display: flex;align-items: center;justify-content: center;border-radius: 3px;"  v-show="jugeattention()&&jugelogin()" @click="deleteattention()">已关注</view>
					<view style="margin-left: 20px;width:73px;height:36px;background-color: #ecf5ff;border: 1px solid #99dc6d;color:#99dc6d;display: flex;align-items: center;justify-content: center;border-radius: 3px;"  v-show="jugelogin()"  @click="tochat()">私信</view>
					</view>
					</view>
					</view>
				</view>
			</view>
		</view>
		</u-transition>
		
		<view style="width:60%;height:70%;margin: 0 auto;margin-top: 20px;display: flex;flex-direction: row;" :style="{'opacity':op}">
			
			<view style="width:65%;height:auto">
			<u-transition mode="fade-left" show>
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;"  v-if="themelist.length==0">
					<text style="margin-top: 20px;margin-left: 20px;font-size: 20px;font-weight: 700;">他的主题</text>
					<text style="font-size: 15px;color:#999999;margin-left: 10px;">暂无主题</text>
				</view>
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;" v-for="(item,index) in themelist " :id="'t'+index">
					<view v-if="index==0">
					<text style="margin-top: 20px;margin-left: 20px;font-size: 20px;font-weight: 700;">他的主题</text>
					<text style="font-size: 15px;color:#999999;margin-left: 10px;">{{themelist.length}}个主题</text>
					<u-divider text="分割线" :dot="true" style="width:98%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					</view>
					<view style="width: 100%;padding: 0%;display: flex;flex-direction: row;;
					margin-left: 0%;
					">
						<view style="width:50px;margin-right: 5px;height:45px;display: flex;align-items: center;">
						<u-avatar mode="aspectFill" :src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
						<u-avatar :text="item.username.substr(0,1)"  v-if="item.userheadimage==null" size="45"></u-avatar>
						</view>
						<view style="height:45px;display: flex;flex-direction: column;margin-left: 5px;margin-bottom: 10px;width: 100%;">
							<view>
								<view style="display: flex;flex-direction: row; width:100%;">
									<view style="display: flex;align-items:center">
										<text style="margin-right: 10px;font-size:15px">{{item.username}}</text><text style="font-size: 15px;" v-if="item.shenfen.length!=0">·</text><text style="margin-left:10px;font-size: 14px;color:#909399" v-if="item.shenfen.length!=0">{{item.shenfen}}</text>
									</view>
									<view style="margin-left: auto;margin-right: 2%;display: flex;flex-direction: row;"> 
										<u-tag text="原创"  plain plainFill size="mini" v-if="item.type==0" type="warning"></u-tag>
										<u-tag text="搬运"  plain plainFill size="mini" v-if="item.type==1" type="warning"></u-tag>
										<u-tag text="精华"  plain plainFill size="mini" v-if="item.jing==1" type="error" style="margin-left: 10px;"></u-tag>
									</view>
								
								</view>
								<view style="display: flex;flex-direction: row;align-items: center;">
									<text style="font-size: 14px;color:#838383;margin-right: 5px;">{{time(item.createtime)}}</text>
									<u-icon name="eye"></u-icon>
									<text style="color:#838383;font-size: 14px;margin-left: 2px;">{{item.look}}</text>
								</view>
							</view>
						</view>
					</view>
					<view style="width:auto;display: flex;flex-direction: column;align-items: center;" @click="link(item.themeid)">
						<text style="font-size: 20px;font-weight: 700;margin-bottom:20px">
							{{item.title}}
						</text>
						<mp-html
						 
						:content="item.text" 
						style="max-height: 500px;overflow-y: hidden;width:80%;margin-bottom: 25px;"
						/>
						<view style="width:100%;height:80px;background:linear-gradient(to top,rgba(255,255,255,1) 50%,rgba(255,255,255,0) );margin-top: -80px;
						z-index: 2;
						" 
						v-if="jugetops[index]"
						>
							 &nbsp
						</view>
						<view style="width:100%;height:20px;background:rgba(255,255,255,1);margin-top: -20px;
						z-index: 2;display: flex;align-items: center;justify-content: center;flex-direction: row;color:#99dc6d;margin-bottom: 10px;
						" 
						v-if="jugetops[index]"
						>
							查看更多<u-icon name="arrow-down" size=18 color="#99dc6d"></u-icon>
						</view>
						<view v-if="item.image.length==1">
							<view  style="display: flex;width:100%;flex-wrap: warp;border-radius: 3px;">
								<u--image 
								:lazy-load="true" 
								radius="3" 
								:src="item.image[0]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" 
								mode="aspectFill" 
								height="300px"
								:width="imagewidth/0.3/0.85" 
								style="margin-left: 2.5px;margin-right: 2.5px;">
								</u--image>
							</view>
						</view>
						<view v-if="item.image.length==2">
							<view  style="display: flex;width:100%;flex-wrap: warp;justify-content: center;">
								<u--image :lazy-load="true"v-for="(image,index) in item.image" radius="3" :src="image+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill" :width="imagewidth/0.3*0.57" style="margin-left: 2.5px;margin-right: 2.5px;" height="300"></u--image>
								
							</view>
						</view>
						<view v-if="item.image.length==3">
							<view  style="display: flex;width:100%;justify-content: center;flex-wrap: warp;">
								<u--image :lazy-load="true"radius="3" :src="item.image[0]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill" :width="imagewidth/0.33*0.45" :height="imagewidth/0.33*0.45"></u--image>
								<u--image :lazy-load="true"radius="3" :src="item.image[1]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" :width="imagewidth/0.33*0.45" :height="imagewidth/0.33*0.45" style="margin-left: 5px;margin-right: 5px;"></u--image>
								<u--image :lazy-load="true"radius="3" :src="item.image[2]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" :width="imagewidth/0.33*0.45" :height="imagewidth/0.33*0.45"></u--image>
							</view>
						</view>
						<view v-if="item.image.length==4">
							<view style="display: flex;width:100%;justify-content: center;margin-bottom: 0;">
								<u--image :lazy-load="true" radius="3" :src="item.image[0]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill" :width="imagewidth/0.3*0.57" style="margin-right: 2.5x;" height="300" ></u--image>
								<u--image :lazy-load="true"  radius="3" :src="item.image[1]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" :width="imagewidth/0.33*0.57" style=";margin-left: 2.5px;" height="300"></u--image>
							</view>
							<view style="display: flex;width:100%;justify-content: center;margin-bottom: 0;margin-top: 10px;">
								<u--image :lazy-load="true" radius="3" :src="item.image[2]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill" :width="imagewidth/0.33*0.57" style="min-width: 300px;margin-right: 5px;" height="300"></u--image>
								<u--image :lazy-load="true" radius="3" :src="item.image[3]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill":width="imagewidth/0.33*0.57" style="min-width: 300px;margin-left: 5px;" height="300"></u--image>
							</view>
						</view>
						
						<view v-if="item.image.length>=5" >
						
							<view style="display: flex;width:100%;justify-content: center;margin-bottom: 0;align-items: center;">
								<u--image  radius="3" :src="item.image[0]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" :width="imagewidth/0.3*0.55" mode="aspectFill"  style="min-width:300px;margin-right: 5px;"></u--image>
								<u--image  radius="3" :src="item.image[1]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" :width="imagewidth/0.3*0.55" mode="aspectFill"  style=";min-width:300px;margin-left: 5px;"></u--image>
							</view>
							<view  style="display: flex;width:100%;flex-wrap: warp;margin-top: 5px;">
								<u--image radius="3" :src="item.image[2]+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill"  :width="imagewidth/0.3*0.365"></u--image>
								<u--image  radius="3" :src="item.image[3]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" :width="imagewidth/0.3*0.365" style="margin-left: 7px;margin-right: 7px;"></u--image>
								<u--image v-if="item.image.length<=5" radius="3" :src="item.image[4]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" style="min-;width:11.5%;margin-right:0px" :width="imagewidth/0.3*0.37"></u--image>
								
								<view v-if="item.image.length>5" style="position: relative;border-radius:3px;overflow: hidden;" :style="{width:(imagewidth/0.3*0.365)+'px'}">
								<u--image  radius="3" :src="item.image[4]+'?imageMogr2/crop/x3000/thumbnail/1000000@'"  mode="aspectFill" style="position: absolute;z-index: 1;" :width="imagewidth/0.3*0.365"></u--image>
								<view style="height:100%;position: absolute;z-index: 4;background-color: rgba(0,0,0,0.5);display: flex;align-items: center;justify-content: center;font-size: 50px;color:white" :style="{width:imagewidth/0.3*0.365+'px'}">
									+{{item.image.length-5}}
								</view>
								</view>
							</view>
						</view>
						<view style="width:100%;height:30px"></view>
					</view>
				</view>
				<view style="height:20px;opacity: 0;"></view>
			</u-transition>
			</view>
			
			<view style="display: flex;flex-direction: column;width:33%;margin-left: 5%;border-radius: 15px;height:auto">
			<u-transition mode="fade-right" show>
			<view style="margin-bottom: 10px;background-color: white;border-radius: 15px;">
				<view style="margin-top: 20px;margin-left: 10px;margin-right: 10px;">
					<text style="font-size: 20px;font-weight: 700;">个人资料</text>
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					
				<view style="display: flex;flex-direction: row;align-items: center;width:100;justify-content: center;font-size: 18px;color:black;font-weight: 500;">
					
					<text>关注:{{attention}}</text>
					<text style="margin-left: 20%;margin-right: 20%;">粉丝:{{fans}}</text>
					<text>获赞:{{bylike}}</text>
				</view>
					<view style="color:#787878">
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					<text style="margin-right:5px;">UID :</text><text>{{userid}}</text>
					<br><br>
					<view style="width:100%;display: flex;flex-direction: row;align-items: center;">
					<view style="width: 100%;display: flex;flex-direction: row;align-items: center;">
					<text style="margin-right: 5px;">签名 :</text>
					<u--text :text="sign" type="info" :lines="1" v-if="sign!=null"></u--text>
					<u--text text="暂无签名" type="info" :lines="1" v-if="sign==null"></u--text>
					</view>
					</view>
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					</view>
				</view>
			</view>
			
			<view  style="width:90%;height:100px;background-color:white;padding-left:5%;padding-right: 5%;padding-bottom: 12%;border-radius: 15px;padding-top:10px;margin-top: 0px;"
			>
			<view style="display:flex;flex-direction: row;">
				<view>
					<image src="../../static/二维码.png" style="width:100px;height:100px"></image>
				</view>
				<view style="margin-left: 5px;display:flex;justify-content: center;flex-direction: column;">
					<view>
					<text style="font-size: 22px;font-weight:800;">扫描下载移动端</text>
					</view>
					<view style="display: flex;;align-items: center;">
				<text style="font-size: 22px;font-weight:500;color:#707070;">Laplace Begonia</text>
				<image style="height:22px;width:22px;margin-left:5px;margin-top:3px" src="../../static/book.png"></image>
					</view>
				</view>
			</view>
				<view @click="beian()" >
					<text   style="font-size: 12px;color:#707070;">吉ICP备2021004411号</text>
				</view>
			</view>
			</u-transition>
			</view>
		
		</view>
		</scroll-view>
		<view style="width:40px;height:40px;border-radius: 50%;display: flex;align-items: center;justify-content: center;background-color: #99dc6d;
		position: fixed;bottom:20px;z-index: 5;;margin-left: 95%;transition-duration:0.3s"
		 @click="backtop()"  v-show="open1==1"
		>
			<u-icon name="arrow-up-fill" size="30" style="" color="#ffffff"></u-icon>
		</view>
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
				replymessageshow:false,
				jugetops:[],
				key:"",
				attention:0,
				fans:0,
				userid:-1,
				username:"",
				userbackgroundimage:"",
				op:1,
				open1:0,
				attentionlist:[],
				sign:"",
				save:0,
				bylike:0,
				registertime:0,
				admin:0,
				icon:"/static/icon.png",
				themelist:[],
				email:"",
				toutu:"",
				getmore:true,
				limit:0,
				scroll_top:0
			}
		},

		onLoad(user) {
			this.imagewidth = uni.getSystemInfoSync().windowWidth*0.4*0.8*0.8*0.33;
			this.attentionlist=getApp().globalData.attention;
			if(user.userid==getApp().globalData.userid){
				uni.reLaunch({
					url:"myself"
				})
				return;
			}
			this.myusername=getApp().globalData.username;
			this.myuserheadimage=getApp().globalData.userheadimage;
			uni.request({
				url:getApp().globalData.http+"/api/getheself",
				method:"POST",
				data:{
					"userid":parseInt(user.userid)
				},
				success: (res) => {
					if(res.data.data.username==null||res.data.data.username==undefined){
						uni.reLaunch({
							url:"/pages/index/index"
						})
					}
					this.userid=res.data.data.userid;
					this.username=res.data.data.username;
					this.userheadimage=res.data.data.userheadimage;
					this.userbackgroundimage=res.data.data.userbackgroundimage;
					if(this.userbackgroundimage==""||this.userbackgroundimage==null||this.userbackgroundimage==undefined){
					this.userbackgroundimage='../../static/bg.webp'
					}
					this.sign=res.data.data.sign;
					this.save=res.data.data.save;
					this.bylike=res.data.data.bylike;
					this.registertime=res.data.data.registertime;
					this.admin=res.data.data.admin;
					this.toutu=res.data.data.toutu;
					this.fans=res.data.data.fans;
					this.attention=res.data.data.attention;
					uni.setNavigationBarTitle({
							title:this.username
					})
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getthemelistby_user",
				data:{
					"userid":parseInt(user.userid),
					"limit":this.limit
				},
				method:"POST",
				success: (res) => {
					for(var i=0;i<res.data.data.length;i++){
						this.jugetops.push(false);
						var src=res.data.data[i].image.toString();
						res.data.data[i].image=src.split(";");
						res.data.data[i].image=res.data.data[i].image.slice(0,-1);
						if(res.data.data[i].tags!=undefined){
						res.data.data[i].tags=res.data.data[i].tags.split(";");
						res.data.data[i].tags.pop();
						for(var j=0;j<res.data.data[i].image.length;j++)
						;
						}
						res.data.data[i].text=this.test(res.data.data[i].text);
					}
					this.jugetop();
					this.limit++;
					this.themelist=res.data.data;
					if(res.data.data.length<10){
						this.getmore=false;
					}
				}
			})
		},
		onShow(){
			this.replymessageshow=getApp().globalData.replymessageshow
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
			this.attentionlist=getApp().globalData.attention;
			this.admin=getApp().globalData.admin;
		},
		methods: {
			backtop:function(){
				this.scroll_top=Math.random();
			},
			scroll:function(e){
				
				if(e.detail.scrollTop>20){
					this.open1=1;
				}
				else {this.open1=0;}
			},
			
			tochat:function(){
				getApp().globalData.chat_userid=this.userid;
				getApp().globalData.chat_username=this.username;
				getApp().globalData.chat_userheadimage=this.userheadimage
				uni.setStorage({
				    key: "chat_userid",
				    data: this.userid,
				    success: function () {
				    }
				});
				uni.setStorage({
				    key: "chat_username",
				    data: this.username,
				    success: function () {
				    }
				});
				uni.setStorage({
				    key: "chat_userheadimage",
				    data:this.userheadimage,
				    success: function () {
				    }
				});
				var url="chat";
				uni.navigateTo({
					url:url
				})
			},
			beian:function(){
				window.location.href="https://beian.miit.gov.cn/"
				
			},
			jugetop:function(){
				let that=this;
				setTimeout(() => {  
					for(var i=0;i<this.themelist.length;i++){
						let info = uni.createSelectorQuery().select("#t"+i);
						　　　  　info.boundingClientRect(function(data) { //data - 各种参数
						　　　  　if(data.height>500)that.jugetops[i]=true;  // 获取元素宽度
								 else that.jugetops[i]=false;
						}).exec()
					}
					
					this.$set(this.jugetops,this.jugetops);
				} , 5)
				
			},
			getmoretheme:function(){
				if(this.getmore==false)return;
				this.getmore=false;
				uni.request({
					url:getApp().globalData.http+"/api/getthemelistby_user",
					data:{
						"userid":this.userid,
						"limit":this.limit
					},
					method:"POST",
					success: (res) => {
						for(var i=0;i<res.data.data.length;i++){
							this.jugetops.push(false);
							var src=res.data.data[i].image.toString();
							res.data.data[i].image=src.split(";");
							res.data.data[i].image=res.data.data[i].image.slice(0,-1);
							if(res.data.data[i].tags!=undefined){
							res.data.data[i].tags=res.data.data[i].tags.split(";");
							res.data.data[i].tags.pop();
							for(var j=0;j<res.data.data[i].image.length;j++)
							;
							}
							res.data.data[i].text=this.test(res.data.data[i].text);
							this.themelist.push(res.data.data[i]);
						}
						this.limit++;
						
						this.getmore=true;
						if(res.data.data.length<10){
							this.getmore=false;
						}
						this.jugetop();
					}
				})
			},
			addattention:function(){
				uni.request({
					url:getApp().globalData.http+"/api/addattention",
					method:"POST",
					data:{
						"a":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"b":this.userid
					},
					success:(res)=>{
						if(res.data.code==1001){
							getApp().globalData.attention.push(this.userid);
							this.attentionlist=getApp().globalData.attention;
						}
					}
				})
			},
			deleteattention:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deleteattention",
					method:"POST",
					data:{
						"a":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"b":this.userid
					},
					success:(res)=>{
						if(res.data.code==1001){
							for(var i=0;i<getApp().globalData.attention.length;i++){
								if(getApp().globalData.attention[i]==this.userid){
									getApp().globalData.attention.splice(i,1);
									this.attentionlist=getApp().globalData.attention;
								}
							}
						}
					}
				})
			},
			jugeattention:function(){
				this.attentionlist=getApp().globalData.attention;
				for(var i=0;i<this.attentionlist.length;i++){
					if(this.userid==this.attentionlist[i])
						return true;
				}
				return false;
			},
			test:function(e){
				
				var imgReg = /<img.*?(?:>|\/>)/gi;//定义正则表达式（拿到img标签所有值）
				      
				var deArray  = e.match(imgReg);//使用正则表达式，拿到的是数组
				//["<img src="images/01.gif">", "<img src="images/02.gif">"]
				if (deArray != null && deArray != undefined) {
				        for (var i=0;i<deArray.length;i++) {
				            e = e.replace(deArray[i], "") //放在循环中剔除img标签
				        }
				    }
				
				return e;
			},

			link:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/lookadd",
					method:"POST",
					data:{
						"themeid":e
					},
					success: (res) => {
						;
					}
				})
				uni.navigateTo({
					url:"theme?themeid="+e
				})
			},

			time:function(e){
				var g = new Date().getTime()/1000;
				var time=parseInt(g)-parseInt(e);
				if(time<60)
					return "发布于"+parseInt(time)+"秒前";
				else if(time>=60&&time<60*60)
					return "发布于"+parseInt(time/60)+"分钟前";
				else if(time>=60*60&&time<60*60*24)
					return "发布于"+parseInt(time/60/60)+"小时前"
				else{
					var g = new Date().getTime();
					var d = new Date(g);
					var e = new Date(e*1000);
					var year = d.getFullYear(); 
					var month = d.getMonth() + 1; //取得日期中的月份，其中0表示1月，11表示12月
					var date = d.getDate(); 
					var year1 = e.getFullYear();
					var month1 = e.getMonth() + 1; //取得日期中的月份，其中0表示1月，11表示12月
					var date1 = e.getDate(); 
					if(year!=year1)
						return year1+"-"+month1+"-"+date1;
					else return month1+"-"+date1;
				}
			},
			

			jugelogin:function(){
				if(getApp().globalData.cookie!='')return true;return false;
			},
			
		
		}
	}
</script>

<style>

</style>
