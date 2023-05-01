<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<!--center-->
		<!--主题-->
		<view style="width:100%;display: flex;justify-content: center;height:110vh;overflow-y: hidden;margin-top: 10vh;" >
		
		<scroll-view :scroll-y="true" style="width:40%;height:auto;border-radius: 10px;padding-bottom: 10%;margin-bottom: 10px;overflow-y: auto;" :style="{'opacity':op}"
		:scroll-into-view="tag" scroll-with-animation  @scroll="jugetop" :scroll-top="top"
		
		>
		<view id="a3">
			<u-transition mode="fade-left" show>
			<view  style="width:96%;height:auto;margin-bottom: 15px;border-radius: 10px;background-color: white;padding:2%;
			padding-left:2%;padding-right: 2%;margin-bottom: 50px;
			">
				<text style="font-size: 20px;font-weight: 700;">热门话题</text>
			    <qiun-data-charts 
			        type="word"
			        :chartData="chartData2"
			        :canvas2d="true"
			        canvasId="owqpLZfCzNgRIMqMvBxTxmTuxabNKZXS"
					@getIndex="linktag1"
			        />
			  </view>
			<view  style="width:96%;height:auto;margin-top: 15px;margin-bottom: 15px;border-radius: 10px;background-color: white;padding:2%;
			padding-left:2%;padding-right: 2%;margin-bottom: 50px;
			">
				
				<view v-for="(item,index) in taglist" v-if="index<3" style="width:100%;margin-top: 10px;margin-bottom: 10px;display: flex;flex-direction: row;" @click="linktag(item.tagname)">
					<view style="min-width:100px;height:100px;border-radius: 10px;overflow: hidden;" v-if="item.imagesrc!=''">
						<image style="width:100px;height:100px" :src="item.imagesrc+'?imageMogr2/crop/x3000/thumbnail/1000000@'" mode="aspectFill"></image>
					</view>
					<view style="width:100px;height:100px;border-radius: 10px;background-color: #909399;color:white;display: flex;align-items: center;justify-content: center;" v-if="item.imagesrc==''">
						<view>
						<u--text :text="item.tagname.substr(0,1)" size="30" color="white"></u--text>
						</view>
					</view>
					<view style="margin-left: 10px;">
						<view style="font-size: 18px;font-weight: 700;"><text>#{{item.tagname}}#</text></view>
						<view style="display: flex;flex-direction: row;font-size: 15px;margin-top: 3px;margin-bottom: 3px;color:#909399">
							<view style="display: flex;flex-direction: row;margin-left: 5px;margin-right: 5px;align-items: center;"><u-icon name="eye" size="15"></u-icon><text>{{item.look}}</text></view>
							<view style="display: flex;flex-direction: row;align-items: center;"><u-icon name="chat" size="15"></u-icon><text>{{item.num}}</text></view>
						</view>
						<view style="font-size: 15px;">
							<text style="color:#8bc863">#{{item.tagname}}#</text><text style="opacity: 0;">1</text><text>{{item.title}}</text>
						</view>
					</view>
				</view>
			</view>
			</u-transition>
		</view>
			<view id="a2">
				<u-transition mode="fade-left" show>
				<view  style="width:96%;height:auto;margin-top: 15px;margin-bottom: 15px;border-radius: 10px;background-color: white;padding:2%;
				padding-left:2%;padding-right: 2%;margin-bottom: 50px;
				">
					<text style="font-size: 20px;font-weight: 700;">推荐用户</text>
					<view style="height:20px"></view>
					<view v-for="(item,index) in fanss"  v-if="index<5" style="margin-left: 5px;display: flex;flex-direction: row;margin-bottom: 15px;" >
						<view style="margin-right: 5px;" @click="heself(item.userid)">
							<u-avatar  mode="aspectFill" :src="item.userheadimage" v-if="item.userheadimage!=undefined" size="50"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)" randomBgColor v-if="item.userheadimage==undefined" size="50"></u-avatar>
						</view>
						<view style="display: flex;flex-direction: column;margin-left: 5px;">
							<text>{{item.username}}</text>
							<text style="color:#909399">{{item.shenfen1}}</text>
							<view style="display: flex;flex-direction: row;">
							
							<u--text :lines="1" :text="item.sign" size="13"color="#787878"></u--text>
							</view>
						</view>

						<view style="width:50px;height:30px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="!jugeattention(item.userid)&&jugemyself(item.userid)&&jugelogin()" @click="addattention(item.userid)">关注</view>
						<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="jugeattention(item.userid)&&jugemyself(item.userid)&&jugelogin()" @click="deleteattention(item.userid)">已关注</view>
					</view>
					<view style="width: 100%;display: flex;flex-direction: column;align-items: center;justify-content: center;">
					<text style="font-size: 15px;color:#999999;margin:0 auto" v-if="fanss.length<10">暂无更多</text>
					</view>
				</view>
				</u-transition>
			</view>
			<view id="a1" >
			<u-transition mode="fade-left" show>
			<view v-for="(item,index) in themelist" style="width: 100%;;" :style="{'opacity':op}" :id="'t'+index" v-if="index<5">
				<view  style="width:96%;height:auto;margin-top: 15px;margin-bottom: 15px;border-radius: 10px;background-color: white;padding:2%;
				padding-left:2%;padding-right: 2%;
				">
					<view style="font-size: 20px;font-weight: 700;padding-bottom: 30px;" v-if="index==0">热门文章</view>
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
						<text style="font-size: 20px;font-weight: 700;margin-bottom:20px">
							{{item.title}}
						</text>
						<mp-html
						 
						:content="item.text" 
						style="max-height: 500px;overflow-y: hidden;width:100%;margin-bottom: 25px;"
						/>
						<view style="width:100%;height:80px;background:linear-gradient(to top,rgba(255,255,255,1) 50%,rgba(255,255,255,0) );margin-top: -80px;
						z-index: 2;
						" 
						v-show="jugetops[index]"
						>
							
						</view>
						<view style="width:100%;height:20px;background:rgba(255,255,255,1);margin-top: -20px;
						z-index: 2;display: flex;align-items: center;justify-content: center;flex-direction: row;color:#8bc863;margin-bottom: 10px;
						" 
						v-show="jugetops[index]"
						>
							查看更多<u-icon name="arrow-down" size=18 color="#8bc863"></u-icon>
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
					<u-tag v-for="(item,index) in item.tags" :text="item"  icon="tags-fill" style="width: auto;margin-right:10px" @click="linktag(item)" plain plainFill></u-tag>
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
							<u-icon name="thumb-up-fill" color="#8bc863"  size="25" style="margin-right: 3px;" ></u-icon>
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
					
					<u-loadmore
						    status="nomore" 
						    loading-text="努力加载中" 
						    loadmore-text="轻轻上拉" 
						    nomore-text="实在没有了" 
							v-if="index==themelist.length-1"
							style="margin-top:10px"
					/>
				</view>
			</view>
			</u-transition>
			</view>
			<view style="height:20px">
				
			</view>
			</scroll-view>
			<u-transition mode="fade-right" style="width:18%" show>
			<view style="width:100%;height:250px;border-radius: 10px;background-color: white;margin-left: 20px;padding:10px"
			 :style="{'opacity':op}"
			>
				
				<text style="font-size: 20px;font-weight: 700;">推荐</text>
				<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px;color:#8bc863" v-if="choose==2">
					<u-icon name="tags" size="33" style="margin-right: 5px;" color="#8bc863"></u-icon><text>热门话题</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px; " v-if="choose!=2" @click="changetag('a3'),choose=2">
					<u-icon name="tags" size="33" style="margin-right: 5px;"  ></u-icon><text>热门话题</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px;color:#8bc863" v-if="choose==0">
					<u-icon name="account" size="33" style="margin-right: 5px;" color="#8bc863"></u-icon><text>推荐用户</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px; " v-if="choose!=0" @click="changetag('a2'),choose=0">
					<u-icon name="account" size="33" style="margin-right: 5px;"  ></u-icon><text>推荐用户</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px;color:#8bc863" v-if="choose==1">
					<u-icon name="file-text" size="33" style="margin-right: 5px;" color="#8bc863"></u-icon><text>热门文章</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 30px;font-size: 18px;" v-if="choose!=1" @click="changetag('a1'),choose=1">
					<u-icon name="file-text" size="33" style="margin-right: 5px;"></u-icon><text>热门文章</text>
				</view>
				<view  style="width:100%;height:100px;margin-top:50px; background-color:white;padding-left:2%;padding-right: 5%;margin-left:-3%;padding-bottom: 12%;border-radius: 10px;padding-top:10px;"
				>
				<view style="display:flex;flex-direction: row">
					<view>
						<image src="../../static/二维码.png" style="width:100px;height:100px"></image>
					</view>
					<view style="margin-left: 5px;display:flex;justify-content: center;flex-direction: column;">
						<view>
						<text style="font-size: 22px;font-weight:800;">扫描下载移动端</text>
						</view>
						<view style="display: flex;align-items:center">
						<text style="font-size: 22px;font-weight:500;color:#707070;">Laplace Begonia</text>
						<image style="height:22px;width:22px;margin-left:5px;margin-top:3px" src="../../static/book.png"></image>
						</view>
					</view>
				</view>
					<view @click="beian()" >
						<text   style="font-size: 12px;color:#707070;">吉ICP备2021004411号</text>
					</view>
				</view>
			</view>
			</u-transition>
		</view>
		
		<view>
				<u-toast ref="uToast"></u-toast>
				
		</view>
	</view>
</template>

<script>
	import titles from './title.vue'
	import XsuuSwiper from "@/components/Xss-swiper/Xsuu-swiper.vue"
	export default {
		components:{
			titles,XsuuSwiper
		},
		data() {
			return {
				a1:0,
				a2:0,
				a3:0,
				choose:2,
				tag:"",
				userbackgroundimage:"",
				username:"",
				userheadimage:"",
				themelist:[],
				attentionlist:[],
				op:1,
				type:0,
				admin:0,
				mylike:[],
				taglist:[],
				fanss:[],
				icon:"/static/icon.png",
				top:0,
				show1:false,
				jugetops:[],
				imagewidth:0,
				replymessageshow:false,
				list:[
					{
						type: 'error',
						title: "失败",
						message:"请登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
				],
				chartData2:{
					series:[
						
					]
				}
			}
		},
		onShow() {
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
			this.admin=getApp().globalData.admin;
		},
		onLoad(){
			this.imagewidth = uni.getSystemInfoSync().windowWidth*0.96*0.4*0.8*0.8*0.33;
			this.attentionlist=getApp().globalData.attentionlist;
			this.admin=getApp().globalData.admin;
			this.mylike=getApp().globalData.mylike;
			//console.log(getApp().globalData.mylike);
			this.op=getApp().globalData.op;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.top=1;
			var id=0;
			if(getApp().globalData.userid>0)
				id=getApp().globalData.userid;
			uni.request({
				url:getApp().globalData.http+"/api/userhotlist",
				data:{
					"userid":id
				},
				method:"POST",
				success: (res) => {
					for(var i=0;i<res.data.length;i++){
					
						if(res.data[i].userid!=getApp().globalData.userid){
							if(res.data[i]?.shenfen&&res.data[i].shenfen.length>0){
								var str="";
								for(var j=0;j<res.data[i].shenfen.length-1;j++){
									str+=res.data[i].shenfen[j].name+='、';
								}
								str+=res.data[i].shenfen[res.data[i].shenfen.length-1].name;
								res.data[i].shenfen1=str;
								
							}
							this.fanss.push(res.data[i]);
						}
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/gethottag",
				data:{
				},
				method:"POST",
				success: (res) => {
					if(res.data.code==1001){
						this.taglist=res.data.data;
						
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getthemelistby_hot",
				data:{
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
					this.themelist=res.data.data;
					this.show1=true;
					if(this.themelist.length>0)
					this.jugetop1();
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/gethottagschart",
				method:"POST",
				data:{},
				success: (res) => {
					
					{
						var temp=[];
						for(var i=0,j=30;i<res.data.hottagschart.length;i++,j--){
							temp.push({
								"name":res.data.hottagschart[i].name,
								"textSize":j,
								"data":undefined
							})
						}
						setTimeout(()=>{
							this.chartData2.series=temp;	
						},300)			
					}
											
				}
			})
		},
		
		methods: {
		jugelogin(){
			if(getApp().globalData.cookie!='')return true;return false;
		},
		jugetop1:function(){
			let that=this;
			setTimeout(() => {  
			for(var i=0;i<5;i++){
				let info = uni.createSelectorQuery().select("#t"+i);
				　　　  　info.boundingClientRect(function(data) { 
						
						//data - 各种参数
				　　　  　if(data.height>500)that.jugetops[i]=true;  // 获取元素宽度
				}).exec()
			}this.$set(this.jugetops,this.jugetops);
			} , 1)
		},
			linktag:function(e){
				uni.navigateTo({
					url:"tag?tagname="+e
				})
			},
			linktag1:function(e){
				uni.navigateTo({
					url:"tag?tagname="+this.chartData2.series[e.currentIndex].name
				})
			},
			heself:function(e){
				uni.navigateTo({
					url:"heself?userid="+e
				})
			},
			addattention:function(e){
				if(getApp().globalData.cookie==""){
					this.$refs.uToast.show({
						...this.list[0],
						complete() {
						}
					})
				}
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
			jugemyself:function(e){
				if(e==getApp().globalData.userid)return false;return true;
			},
			jugeattention:function(e){
				
				this.attentionlist=getApp().globalData.attention;
				for(var i=0;i<this.attentionlist.length;i++){
					if(e==this.attentionlist[i])
						return true;
				}
				return false;
			},

			jugetop:function(e){
				let that=this;
				uni.getSystemInfo({
				　　success: function(res) { // res - 各种参数
						let info = uni.createSelectorQuery().select("#a3");
				　　　  　info.boundingClientRect(function(data) { //data - 各种参数
						 that.a3=data.height;
				　　    }).exec()
				    }
				});
				uni.getSystemInfo({
				　　success: function(res) { // res - 各种参数
						let info = uni.createSelectorQuery().select("#a1");
				　　　  　info.boundingClientRect(function(data) { //data - 各种参数
						 that.a1=data.height;
				　　    }).exec()
				    }
				});
				uni.getSystemInfo({
				　　success: function(res) { // res - 各种参数
						let info = uni.createSelectorQuery().select("#a2");
				　　　  　info.boundingClientRect(function(data) { //data - 各种参数
						 that.a2=data.height;
				　　    }).exec()
				    }
				});
				if(e.detail.scrollTop<that.a3){
					that.choose=2;
				}
				else if(e.detail.scrollTop>that.a3&&e.detail.scrollTop<(that.a3+that.a2)){
					that.choose=0;
				}
				else if(e.detail.scrollTop>(that.a3+that.a2)){
					that.choose=1;
				}
				
			},
			changetag:function(e){
				if(e=="a1")this.tag="a1"; else if(e=="a2")this.tag="a2";else this.tag="a3";return;
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
			jugelike:function(res){
				this.mylike=getApp().globalData.mylike;
				for(var i=0;i<this.mylike.length;i++)
					if(this.mylike[i]==res)return true;return false
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
			
		
		}
	}
</script>

<style>

</style>
