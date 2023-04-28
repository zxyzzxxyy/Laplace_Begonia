<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<!--center-->
		<!---->
		
		<scroll-view style="width:100%;height:60vh;min-height:90vh;display: flex;flex-direction: column;align-items: center;justify-content: center;margin-top: 10vh;display: flex;justify-content: center;" scroll-y @scrolltolower="getmoretheme">
			<view style="width:60%;height:auto;;border-radius: 10px;background-color: white;display: flex;flex-direction: column;align-items: center;min-height: 85vh;overflow-x: hidden;margin:0 auto">
				<view style="margin-left: 0%;width:50%;border:1px solid #c0c4cc;height:35px;display: flex;align-items: center;border-radius: 5px;margin-top: 50px;">
				 <input
				    placeholder="搜索"
					v-model="key"
					 @confirm="search"
					type="text"
					style="color:#909399;border:none;margin-left: 5px;min-height:35px;width:100%"
				  />
				  <u-icon name="search" color="#909399" size="28" style="margin-left: auto;margin-right: 5px;" @click="search()"></u-icon>
				</view>
				<view style="width:100%;margin-left: 10%;font-size: 25px;font-weight: 800;" v-if="userlist.length!=0">
					<text>用户</text>
				</view>
				<!--用户-->
				<view style="width:90%;margin-top: 10px;margin-bottom: 10px;"  v-if="userlist.length!=0">
					<view v-for="(item,index) in userlist" v-if="index<=5" style="margin-left: 10px;display: flex;flex-direction: column;margin-bottom: 10px;" >
					<view style="display: flex;flex-direction: row;align-items: center;">
						<view style="margin-right: 5px;" @click="heself(item.userid)">
				   		<u-avatar :src="item.userheadimage" v-if="!jugetouxiang(item.userheadimage)"></u-avatar>
				   		<u-avatar randomBgColor :text="item.username.substr(0,1)" v-if="jugetouxiang(item.userheadimage)"></u-avatar>
						</view>
						<view style="display: flex;flex-direction: column;">
							<view style="display: flex;align-items:center;margin-left: 5px;">
								<text style="margin-right: 10px;font-size:16px">{{item.username}}</text>
							</view>
						</view>
						<view v-if="jugelogin()&&jugemyself(item.userid)" style="margin-left: auto;margin-right: 10px;">
						<view style="width:50px;height:30px;background-color: #99dc6d;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px"  v-show="!jugeattention(item.userid)" @click="addattention(item.userid)" >关注</view>
						<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #99dc6d;color:#99dc6d;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px"  v-show="jugeattention(item.userid)" @click="deleteattention(item.userid)">已关注</view>
						</view>
						</view>
						<view  v-show="index!=userlist.length-1">
							<u-divider style="width:100%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
						</view>
					</view>
					<view  v-if="userlist.length!=0">
						<u-divider text="分割线" :dot="true" style="width:100%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					</view>
				</view>
				<view style="width:100%;margin-left: 10%;font-size: 25px;font-weight: 800;" v-if="themelist.length!=0">
					<text>主题</text>
				</view>
				<!--主题-->
				<view v-for="(item,index) in themelist" style="width: 90%;" :style="{'opacity':op}" :id="'t'+index">
					
					<view  style="width:96%;height:auto;margin-top: 15px;margin-bottom: 15px;border-radius: 10px;padding:2%;
					padding-left:2%;padding-right: 2%;
					">
						<view style="width: 100%;padding: 0%;display: flex;flex-direction: row;;
						margin-left: 0%;
						">
							<view style="width:50px;margin-right: 5px;height:45px;display: flex;align-items: center;">
							<u-avatar mode="aspectFill":src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)"  randomBgColor v-if="item.userheadimage==null" size="45"></u-avatar>
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
										<text style="font-size: 14px;color:#838383;margin-right: 5px;">{{time(item.replytime)}}</text>
										<u-icon name="eye"></u-icon>
										<text style="color:#838383;font-size: 14px;margin-left: 2px;">{{item.look}}</text>
									</view>
								</view>
							</view>
						</view>
						<view style="width:90%;margin-left: 5%;margin-right: 5%;display: flex;flex-direction: column;align-items: center;" @click="link(item.themeid)">
							<!--<text style="font-size: 20px;font-weight: 700;margin-bottom:20px">
								{{item.title}}
							</text>-->
							<mp-html
							 
							:content="item.title" 
							style="font-size: 20px;font-weight: 700;margin-bottom:20px"
							/>
							<mp-html
							 
							:content="item.text" 
							style="max-height: 500px;overflow-y: hidden;width:100%;margin-bottom: 25px;"
							/>
							<view style="width:100%;height:80px;background:linear-gradient(to top,rgba(255,255,255,1) 50%,rgba(255,255,255,0) );margin-top: -80px;
							z-index: 2;
							" 
							v-show="jugetops[index]"
							>
								 &nbsp
							</view>
							<view style="width:100%;height:20px;background:rgba(255,255,255,1);margin-top: -20px;
							z-index: 2;display: flex;align-items: center;justify-content: center;flex-direction: row;color:#99dc6d;margin-bottom: 10px;
							" 
							v-show="jugetops[index]"
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
									height="500px"
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
						</view>
						<view style="display: flex;flex-direction: row;padding-left: 0%;margin-top: 10px;">
						<u-tag v-for="(item,index) in item.tags" :text="item"  icon="tags-fill" style="width: auto;margin-right:10px"  plain plainFill @click="linktag(item)"></u-tag>
						</view>
						<view style="display: flex;flex-direction: row;align-items: center;padding:15px;justify-content: center;">
							<view style="display: flex;flex-direction: row;align-items: center;color:#838383" v-show="!jugelike(item.themeid)"
							@click="setlike(item.themeid)"
							>
								<u-icon name="thumb-up"  size="25" style="margin-right: 3px;"></u-icon>
								<text v-if="item.likes==0" style="font-size:17px;">赞</text>
								<text v-if="item.likes!=0" style="font-size: 17px;">{{item.likes}}</text>
							</view>
							<view style="display: flex;flex-direction: row;align-items: center;color:#838383" v-show="jugelike(item.themeid)"
							@click="deletelike(item.themeid)"
							>
								<u-icon name="thumb-up-fill" color="#99dc6d"  size="25" style="margin-right: 3px;" ></u-icon>
								<text  style="font-size: 17px;">{{item.likes}}</text>
							</view>
							<view style="display: flex;flex-direction: row;align-items: center;color:#838383;margin-left: 30%;margin-right: 30%;">
								<u-icon name="chat"  size="25" style="margin-right: 3px;"></u-icon>
								<text  style="font-size: 17px;">{{item.num}}</text>
							</view>
							<view style="display: flex;flex-direction: row;align-items: center;color:#838383">
								<u-icon name="share"  size="25" style="margin-right: 3px;"></u-icon>
								<text  style="font-size: 17px;">分享</text>
							</view>
						</view>
						<view>
							<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
						</view>
						<u-loadmore
							    :status="status" 
							    loading-text="努力加载中" 
							    loadmore-text="轻轻上拉" 
							    nomore-text="实在没有了" 
								v-if="index==themelist.length-1"
								style="margin-top:10px"
						/>
					</view>
				</view>
				<view style="width:100%;display: flex;align-items: center;justify-content: center;font-size: 16px;color:#909399;" 
				v-if="show1==1">
					<text>什么也没有找到...</text>
				</view>
				<!---->
			</view>
			<view style="height:5vh;width: 100vw;opacity: 0;">
				
			</view>
		</scroll-view>
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
				icon:"../../static/icon.png",
				username:"",
				userid:0,
				userheadimage:"",
				userbackgroundimage:"",
				op:1,
				imagewidth:0,
				key:"",
				userlist:[],
				status:"loadmore",
				getmore:true,
				themelist:[],
				mylike:[],
				attentionlist:[],
				show1:0,
				limit:0,
				jugetops:[],
				admin:0,
				replymessageshow:false,
			}
		},
		onShow(){
			this.replymessageshow=getApp().globalData.replymessageshow
			this.imagewidth = uni.getSystemInfoSync().windowWidth*0.5*0.8*0.8*0.33;
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
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.admin=getApp().globalData.admin;
		},
		onLoad(option){
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.key=option.key;
			//console.log(this.userheadimage);
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.search();
			this.op=getApp().globalData.op;
			if(getApp().globalData.cookie!='')
			uni.request({
				url:getApp().globalData.http+"/api/jugeuser",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie
				},
				success: (res) => {
					if(res.data.code==1002){
						this.$refs.uToast.show({
							...this.upload[2],
							complete() {
								that.loginout();
							}
						})
			
					}
				}
			})
		},
		methods: {
			link:function(e){
				uni.navigateTo({
					url:"./theme?themeid="+e
				})
			},
			linktag:function(e){
				uni.navigateTo({
					url:"./tag?tagname="+e
				})
			},
			jugetop:function(){
				let that=this;
				setTimeout(() => {  
				for(var i=0;i<this.themelist.length;i++){
					let info = uni.createSelectorQuery().select("#t"+i);
					　　　  　info.boundingClientRect(function(data) { //data - 各种参数
				
					　　　  　if(data.height>500)that.jugetops[i]=true;  // 获取元素宽度
					
					}).exec()
				}this.$set(this.jugetops,this.jugetops);
				} , 1)
			},
			search:function(){
				
				this.limit=0;
				uni.request({
					url:getApp().globalData.http+"/api/search",
					method:"POST",
					data:{
						"key":this.key,
						"limit":this.limit,
					
					},
					success: (res) => {
						for(var i=0;i<res.data.data.theme.length;i++){
							this.jugetops.push(false);
							var src=res.data.data.theme[i].image.toString();
							res.data.data.theme[i].image=src.split(";");
							res.data.data.theme[i].image=res.data.data.theme[i].image.slice(0,-1);
							if(res.data.data.theme[i].tags!=undefined){
							res.data.data.theme[i].tags=res.data.data.theme[i].tags.split(";");
							res.data.data.theme[i].tags.pop();
							}
							res.data.data.theme[i].text=this.test(res.data.data.theme[i].text);
						}
						this.themelist=res.data.data.theme;
						this.userlist=res.data.data.user;
						if(this.themelist.length==0&&this.userlist.length==0)
							this.show1=1;
						else this.show1=0;
						this.limit++;
						if(res.data.data.theme.length<10){
							this.getmore=false;
							this.status="nomore";
						}
						this.jugetop();
					}
				})
			},
			getmoretheme:function(){
				if(this.getmore==false)
					return;
				this.getmore=false;
				this.status="loading";
				uni.request({
					url:getApp().globalData.http+"/api/searchmoretheme",
					method:"POST",
					data:{
						"key":this.key,
						"limit":this.limit
					},
					success:(res)=>{
						for(var i=0;i<res.data.data.length;i++){
							this.jugetops.push(false);
							var src=res.data.data[i].image.toString();
							res.data.data[i].image=src.split(";");
							res.data.data[i].image=res.data.data[i].image.slice(0,-1);
							if(res.data.data[i].tags!=undefined){
							res.data.data[i].tags=res.data.data[i].tags.split(";");
							res.data.data[i].tags.pop();
							}
							res.data.data[i].text=this.test(res.data.data[i].text);
							this.themelist.push(res.data.data[i]);
						}
						this.limit++;
						this.getmore=true;
						if(res.data.data.length<10){
							this.getmore=false;
							this.status="nomore";
						}
						this.jugetop();
					}
				})
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
			time:function(e){
				var g = new Date().getTime()/1000;
				var time=parseInt(g)-parseInt(e);
				if(time<60)
					return "回复于"+parseInt(time)+"秒前";
				else if(time>=60&&time<60*60)
					return "回复于"+parseInt(time/60)+"分钟前";
				else if(time>=60*60&&time<60*60*24)
					return "回复于"+parseInt(time/60/60)+"小时前"
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
			jugeattention:function(e){
				this.attentionlist=getApp().globalData.attention;
				for(var i=0;i<this.attentionlist.length;i++){
					if(e==this.attentionlist[i])
						return true;
				}
				return false;
			},
			addattention:function(e){

				uni.request({
					url:getApp().globalData.http+"/api/addattention",
					method:"POST",
					data:{
						"a":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"b":e
					},
					success:(res)=>{
						if(res.data.code==1001){
							getApp().globalData.attention.push(e);
							this.attentionlist=getApp().globalData.attention;
						}
					}
				})
			},
			jugemyself:function(e){
				if(e==getApp().globalData.userid)return false;return true;
			},
			deleteattention:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/deleteattention",
					method:"POST",
					data:{
						"a":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"b":e
					},
					success:(res)=>{
						if(res.data.code==1001){
							for(var i=0;i<getApp().globalData.attention.length;i++){
								if(getApp().globalData.attention[i]==e){
									getApp().globalData.attention.splice(i,1);
									this.attentionlist=getApp().globalData.attention;
								}
							}
						}
					}
				})
			},
			jugelike:function(res){
				//console.log(this.mylike.length);
				this.mylike=getApp().globalData.mylike;
				for(var i=0;i<this.mylike.length;i++)
					if(this.mylike[i]==res)return true;return false
			},
			setlike:function(e){
				if(getApp().globalData.cookie==''){
					this.$refs.uToast.show({
						...this.upload[3],
						complete() {
							return;
						}
					})
					return;
				}
				uni.request({
					url:getApp().globalData.http+"/api/setmylike",
					method:"POST",
					data:{
						"themeid":e,
						"userid":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie
					},
					success: (res) => {
						//console.log(res.data.code);
						if(res.data.code==1001){
							this.mylike.push(e);
							for(var i=0;i<this.themelist.length;i++){
								if(this.themelist[i].themeid==e){
									this.themelist[i].likes=this.themelist[i].likes+1;break;
								}
							}
							getApp().globalData.mylike=this.mylike;
							
						}
					}
				})
			},
			jugetouxiang:function(e){
				var touxiang="";
				touxiang=e;
				if(touxiang==undefined){
				return true;
				}
			
				return false;
			},
			heself:function(e){
				uni.navigateTo({
					url:"heself?userid="+e
				})
			},
			deletelike:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/deletemylike",
					method:"POST",
					data:{
						"themeid":e,
						"userid":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie
					},
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<this.mylike.length;i++){
								if(this.mylike[i]==e){
									this.mylike.splice(i,1);
									//console.log(this.mylike);
									getApp().globalData.mylike=this.mylike;
									for(var i=0;i<this.themelist.length;i++){
										if(this.themelist[i].themeid==e){
											this.themelist[i].likes=this.themelist[i].likes-1;break;
										}
									}
									break;
								}
							}
							
						}
					}
				})
			},
			
			jugelogin:function(){
				if(getApp().globalData.cookie!='')return true;return false;
			},
			
		}
	}
</script>

<style>

</style>
