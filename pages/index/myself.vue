<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<u-transition mode="fade-down" style="100%" show>
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		</u-transition>
		<!--center-->
		
		<scroll-view :scroll-with-animation="true" style="height:100vh;;width:100%;;background-size: cover;background-attachment: fixed;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" scroll-y @scrolltolower="getmoretheme" @scroll="scroll" :scroll-top="scroll_top">
		<view style="height:0px;width:100%;opacity: 0;">	
		</view>
		<view style="width: 100vw;height:100px;">
			
		</view>
		<u-transition  mode="fade-down" show >
		<view style="width:60%;;height:auto;background-color: white;margin: 0 auto;border-radius: 15px;overflow: hidden;" :style="{'opacity':op}"
		
		>
			<view style="width:100%;height:150px">
				<image :src="toutu" style="width:100%;height:100%" mode="aspectFill" >
					
				<image>
			</view>
			<view style="display: flex;flex-direction: row;position: relative;">
				<view  style="width:170px" v-show="show">
				<view style="z-index: 3;position: absolute;z-index: 2;width:110px;height:110px">
				<u-avatar mode="aspectFill" size="110" :src="userheadimage" v-if="username!=''&&userheadimage!=null" style="margin-top: -50px;margin-left: 50px;" ></u-avatar>
				<u-avatar  size="110" :text="username.substr(0,1)"  randomBgColor v-if="username!=''&&userheadimage==null" style="margin-top: -50px;margin-left: 50px;"></u-avatar>
				</view>
				<view style="margin-left: auto;width:28px;display: flex;justify-content: center;align-items: center;background-color: #2469f6;border-radius: 50%;z-index: 2;position: absolute;margin-left: 135px;margin-top: 30px;border:2px solid white;height:28px">
				<u-icon name="camera-fill" size="24" color="#ffffff" style="background-color: #2469f6;border-radius: 50%;" @click="head()"></u-icon>
				</view>
				</view>
				<view style="height:100px;margin-top: -50px;display: flex;flex-direction: column;width:80%;justify-content: center;margin-left: 0px;
				padding-bottom: 20px;"
				
				>
					<view style="z-index: 2;display: flex;flex-direction: row;">
					<text style="font-size: 30px;font-weight: 700;margin-right: auto;">{{username}}</text>
					<view style="margin-left: auto;margin-top: 20px;">
					<u--text style=";" iconStyle="18px" prefixIcon="photo" type="info" text="上传封面图片" @click="settoutu()"></u--text>
					</view>
					</view>
					
					<u--text :text="sign" type="info" v-if="sign!=null" ></u--text>
					<u--text text="暂无签名" type="info" v-if="sign==null"></u--text>
					
					
				</view>
			</view>
		</view>
		</u-transition>
		
		<view style="width:60%;height:100vh;;margin: 0 auto;margin-top: 20px;display: flex;flex-direction: row;" :style="{'opacity':op}">
			
			<view style="width:65%;">
			<u-transition  mode="fade-left" show >
				<view style="width:100%;height:80px;border-radius: 15px;background-color: white;margin-bottom: 20px;display: flex;align-items: center;">
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:80px;font-size: 14px;color:#606266" @click="linktomessage()">
						<u-icon name="email" color="#606266" size="28"></u-icon><text>我的消息</text>
					</view>
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:80px;font-size: 14px;color:#606266" @click="show3=true">
						<u-icon name="star" color="#606266" size="28" ></u-icon><text>我的收藏</text>
					</view>
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:80px;font-size: 14px;color:#606266" @click="show4=true">
						<u-icon name="thumb-up" color="#606266" size="28"></u-icon><text>我的点赞</text>
					</view>
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:80px;font-size: 14px;color:red" v-if="ban==1" @click="show8=true">
						<u-icon name="warning" color="red" size="28"></u-icon><text>账号停用中</text>
					</view>
				</view>
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;"  v-if="themelist.length==0">
					<text style="margin-top: 20px;margin-left: 20px;font-size: 20px;font-weight: 700;">我的主题</text>
					<text style="font-size: 15px;color:#999999;margin-left: 10px;">暂无主题</text>
				</view>
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;" v-for="(item,index) in themelist " :id="'t'+index">
					
					<view v-if="index==0">
					<text style="margin-top: 20px;margin-left: 20px;font-size: 20px;font-weight: 700;">我的主题</text>
					<text style="font-size: 15px;color:#999999;margin-left: 10px;" v-if="themelistnum!=0">{{themelistnum}}个主题</text>
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
						<view style="width:100%;height:30px">
							
						</view>
					</view>
				</view>
				<view style="height:50px;opacity: 0;"></view>
				</u-transition>
			</view>
			
			
			<view style="display: flex;flex-direction: column;width:33%;">
			<u-transition  mode="fade-right" show >	
			<view style="width:100%;margin-left: 5%;border-radius: 15px;background-color: white;height:100%">
				<view style="margin-top: 20px;margin-left: 10px;margin-right: 10px;">
					<view style="width:100%;display:flex;flex-direction:row">
						
					<text style="font-size: 20px;font-weight: 700;">个人资料</text>								<view style="margin-left:auto;margin-right:1%">
					<text style="font-size: 15px;color:#8bc863" @click="changepassword()">修改密码</text>
					</view>
					</view>
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					
				<view style="display: flex;flex-direction: row;align-items: center;width:100;justify-content: center;font-size: 18px;color:black;font-weight: 500;">
					
					<text >关注:{{attention}}</text>
					<text style="margin-left: 20%;margin-right: 20%;">粉丝:{{fans}}</text>
					<text>获赞:{{bylike}}</text>
				</view>
					<view style="color:#787878">
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					<text style="margin-right:5px;">邮箱 :</text><text>{{email}}</text>
					<br><br>
					<text style="margin-right:5px;">UID :</text><text>{{userid}}</text>
					<br><br>
					<text style="margin-right:5px;" v-if="birthday!=null">生日 :</text><text v-if="birthday!=null">{{birthday}}</text>
					<br v-if="birthday!=null"><br v-if="birthday!=null">
					<view style="width: 100%;display:flex;flex-direction:row">
						<view style="width: 80%;">
					<text style="margin-right:5px;">身份标识 :</text><text>{{shenfen}}</text>
					</view>
					<text style="margin-left: auto;margin-right: 1%;font-size: 15px;color:#8bc863" @click="show6=true">点击修改</text>
					</view>
					<br>
					
					<text style="margin-right:5px;">注册时间 :</text><text>{{time2(registertime)}}</text><br><br>
					<view style="width:100%;display: flex;flex-direction: row;align-items: center;">
					<view style="width: 65%;display: flex;flex-direction: row;align-items: center;">
					<text style="margin-right: 5px;">签名 :</text>
					<u--text :text="sign" type="info" :lines="1" v-if="sign!=null"></u--text>
					<u--text text="暂无签名" type="info" :lines="1" v-if="sign==null"></u--text>
					</view>
					<text style="margin-left: auto;margin-right: 1%;font-size: 15px;color:#8bc863" @click="open()">点击修改</text>
					</view>
					
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
					</view>

				</view>

			</view>
			<view style="width:100%;margin-left: 5%;border-radius: 15px;background-color: white;height:auto;margin-top: 10px;">
				<view style="margin-top: 20px;margin-left: 10px;margin-right: 10px;">
					<text style="font-size: 20px;font-weight: 700;">关注</text>
				</view>
				<view>
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				</view>
				<view>
					<view v-for="(item,index) in attentions" v-if="index<=5" style="margin-left: 10px;display: flex;flex-direction: row;margin-bottom: 10px;" >
						<view style="margin-right: 10px;" @click="heself(item.userid)">
		           		<u-avatar :src="item.userheadimage" v-if="!jugetouxiang(item.userheadimage)"></u-avatar>
		           		<u-avatar randomBgColor :text="item.username.substr(0,1)" v-if="jugetouxiang(item.userheadimage)"></u-avatar>
						</view>
						<view style="display: flex;flex-direction: column;">
							<text style="font-size: 16px;">{{item.username}}</text>
							<u--text :lines="1" :text="item.sign" size="13"color="#909399"></u--text>
						</view>
						<view style="width:50px;height:30px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="!jugeattention(item.userid)" @click="addattention(item.userid)">关注</view>
						<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="jugeattention(item.userid)" @click="deleteattention(item.userid)">已关注</view>
					</view>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878" v-if="attentionlist.length==0">
					<text>没有更多了</text>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#8bc863" v-if="attentionlist.length>0" >
					<text @click="show1=true">更多</text>
				</view>
			</view>
			<view style="width:100%;margin-left: 5%;border-radius: 15px;background-color: white;height:auto;margin-top: 10px;margin-bottom: 10px;">
				<view style="margin-top: 20px;margin-left: 10px;margin-right: 10px;">
					<text style="font-size: 20px;font-weight: 700;">粉丝</text>
				</view>
				<view>
					<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				</view>
				<view>
					<view v-for="(item,index) in fanss" v-if="index<=5" style="margin-left: 10px;display: flex;flex-direction: row;margin-bottom: 10px;" >
						<view style="margin-right: 5px;" @click="heself(item.userid)">
		           		<u-avatar :src="item.userheadimage" v-if="!jugetouxiang(item.userheadimage)"></u-avatar>
		           		<u-avatar randomBgColor :text="item.username.substr(0,1)" v-if="jugetouxiang(item.userheadimage)"></u-avatar>
						</view>
						<view style="display: flex;flex-direction: column;">
							<text style="font-size: 16px;">{{item.username}}</text>
							<u--text :lines="1" :text="item.sign" size="13"color="#909399"></u--text>
						</view>
						<view style="width:50px;height:30px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="!jugeattention(item.userid)" @click="addattention(item.userid)">关注</view>
						<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="jugeattention(item.userid)" @click="deleteattention(item.userid)">已关注</view>
					</view>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878" v-if="fanss.length==0">
					<text>没有更多了</text>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#8bc863" v-if="fanss.length>0" >
					<text @click="show2=true">更多</text>
				</view>
			</view>
			
			<view  style="width:90%;height:100px;background-color:white;padding-left:5%;padding-right: 5%;padding-bottom: 12%;border-radius: 15px;padding-top:10px;margin-left:5%"
			>
			<view style="display:flex;flex-direction: row;">
				<view>
					<image src="../../static/二维码.png" style="width:100px;height:100px"></image>
				</view>
				<view style="margin-left: 15px;display:flex;justify-content: center;flex-direction: column;">
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
				<view style="width:100%;height:50px">
					
				</view>
			</view></u-transition>
			</view>		
		</view>

		</scroll-view>
		<uni-popup ref="popup" type="dialog">
		    <uni-popup-dialog mode="input" placeholder="请输入签名内容" type="success" :duration="2000" :before-close="true" @close="close" @confirm="confirm"></uni-popup-dialog>
		</uni-popup>
		<view>
				<u-toast ref="uToast"></u-toast>
		</view>
		
		<u-popup :show="show1" mode="center"  @close="show1=false">
		        <view style="width:500px;height:800px;padding-top: 20px;padding-left: 10px;padding-right: 10px;;padding-bottom: 10px;overflow-y: auto;border-radius: 10px;">
					<scroll-view scroll-y style="height:100%" @scrolltolower="getmoreattention()">
					<view style="width: 100%;display: flex;align-items: center;justify-content: center;margin-bottom: 15px;">
						<text style="font-size: 20px;font-weight: 700;">我的关注</text>
					</view>
		           <view v-for="(item,index) in attentions"  style="margin-left: 5px;display: flex;flex-direction: row;margin-bottom: 20px;">
		           	<view style="margin-right: 5px;" @click="heself(item.userid)">
		           		<u-avatar :src="item.userheadimage" v-if="!jugetouxiang(item.userheadimage)"></u-avatar>
		           		<u-avatar randomBgColor :text="item.username.substr(0,1)" v-if="jugetouxiang(item.userheadimage)"></u-avatar>
		           	</view>
		           	<view style="display: flex;flex-direction: column;">
		           		<text>{{item.username}}</text>
		           		<view style="display: flex;flex-direction: row;">
		           		<u--text :lines="1" :text="item.sign" size="13"color="#909399"></u--text>
		           		</view>
		           	</view>
		           	<view style="width:50px;height:30px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="!jugeattention(item.userid)" @click="addattention(item.userid)">关注</view>
		           	<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="jugeattention(item.userid)" @click="deleteattention(item.userid)">已关注</view>
		           </view>
				   <view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878">
				   	<text>没有更多了</text>
				   </view>
					</scroll-view>
		        </view>
		</u-popup>
		<u-popup :show="show2" mode="center"  @close="show2=false" >
		        <view style="width:500px;height:800px;padding-top: 20px;padding-left: 10px;padding-right: 10px;;padding-bottom: 10px;;border-radius: 10px;">
					<scroll-view scroll-y style="height:100%" @scrolltolower="getmorefans()">
					<view style="width: 100%;display: flex;align-items: center;justify-content: center;margin-bottom: 15px;">
						<text style="font-size: 20px;font-weight: 700;">我的粉丝</text>
					</view>
		           <view v-for="(item,index) in fanss"  style="margin-left: 5px;display: flex;flex-direction: row;margin-bottom: 20px;" >
		           	<view style="margin-right: 5px;" @click="heself(item.userid)">
		           		<u-avatar :src="item.userheadimage" v-if="item.userheadimage!=''"></u-avatar>
		           		<u-avatar :text="item.username.substr(0,1)" v-if="item.userheadimage==''"></u-avatar>
		           	</view>
		           	<view style="display: flex;flex-direction: column;">
		           		<text>{{item.username}}</text>
		           		<view style="display: flex;flex-direction: row;">
		           		<u--text :lines="1" :text="item.sign" size="13"color="#909399"></u--text>
		           		</view>
		           	</view>
		           	<view style="width:50px;height:30px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="!jugeattention(item.userid)" @click="addattention(item.userid)">关注</view>
		           	<view style="width:48px;height:28px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;font-size: 15px;margin-left: auto;margin-right: 10px;"  v-show="jugeattention(item.userid)" @click="deleteattention(item.userid)">已关注</view>
		           </view>
				   <view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878" >
				   	<text>没有更多了</text>
				   </view>
					</scroll-view>
		        </view>
		</u-popup>
		<!--收藏-->
		<u-popup :show="show3" mode="center"  @close="show3=false">
			<view style="width:40vw;height:90vh;padding-top: 20px;padding-left: 10px;padding-right: 10px;;padding-bottom: 10px;overflow-y: auto;border-radius: 10px;">
				<scroll-view style="height:100%" scroll-y @scrolltolower="getmoresave()">
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;" v-for="(item,index) in mysavelist " :id="'s'+index">
					
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
						style="max-height: 35vh;overflow-y: hidden;width:80%;margin-bottom: 25px;"
						/>
						<view style="width:100%;height:80px;background:linear-gradient(to top,rgba(255,255,255,1) 50%,rgba(255,255,255,0) );margin-top: -80px;
						z-index: 2;
						" 
						v-show="jugetops2[index]"
						>
							
						</view>
						<view style="width:100%;height:20px;background:rgba(255,255,255,1);margin-top: -20px;
						z-index: 2;display: flex;align-items: center;justify-content: center;flex-direction: row;color:#8bc863;margin-bottom: 10px;
						" 
						v-show="jugetops2[index]"
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
					
					<u-divider text="分割线" :dot="true" style="width:98%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878" >
					<text v-if="savepage==-1">没有更多了</text>
					<text v-if="savepage!=-1">加载中...</text>
				</view>
				</scroll-view>
			</view>
		</u-popup>
		<!--点赞-->
		<u-popup :show="show4" mode="center"  @close="show4=false">
			<view style="width:40vw;height:90vh;padding-top: 20px;padding-left: 10px;padding-right: 10px;;padding-bottom: 10px;overflow-y: auto;border-radius: 10px;">
				<scroll-view style="height:100%" scroll-y @scrolltolower="getmorelike()">
				<view style="width:90%;padding-left: 5%;padding-right: 5%;background-color: white;padding-top: 20px;;background-color: white;border-radius: 15px;margin-bottom: 20px;padding-bottom: 20px;" v-for="(item,index) in mylikelist "  :id="'l'+index">
					
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
						style="max-height: 35vh;overflow-y: hidden;width:80%;margin-bottom: 25px;"
						/>
						<view style="width:100%;height:80px;background:linear-gradient(to top,rgba(255,255,255,1) 50%,rgba(255,255,255,0) );margin-top: -80px;
						z-index: 2;
						" 
						v-show="jugetops1[index]"
						>
							
						</view>
						<view style="width:100%;height:20px;background:rgba(255,255,255,1);margin-top: -20px;
						z-index: 2;display: flex;align-items: center;justify-content: center;flex-direction: row;color:#8bc863;margin-bottom: 10px;
						" 
						v-show="jugetops1[index]"
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
					
					<u-divider text="分割线" :dot="true" style="width:98%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				</view>
				<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;margin-bottom: 10px;color:#787878" >
					<text v-if="likepage==-1">没有更多了</text>
					<text v-if="likepage!=-1">加载中...</text>
				</view>
				</scroll-view>
			</view>
		</u-popup>
		<u-popup mode="center" :show="show6" @close="show6=false" style="height:50vh;width:50vw;
		padding:2vh;;
		">
			<view style="background-color: white;height:4vh;margin-top:1vh;margin-bottom:1vh">
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 2vw;height:100%" >
					<view style="margin-left: auto;margin-right: 5%;">
						<u-tag style="" text="保存" @click="save1"
						> </u-tag>
					</view>
					</view>
			</view>
				<view style="padding-top: 1rem;padding-bottom: 1rem;padding-left: 0.5rem;padding-right: 0.5rem;background-color: aliceblue;margin-top: 1px solid #909399;margin-left:2vw;margin-right:2vw">
					<view>
						<text style="font-size: 14px;color:#909399">我的身份标签({{myidentity.length}}/3)</text>
					</view>
					<view style="margin-top: 1rem;font-size: 14px;">
						<text>身份标签会显示在个人主页，评论区等页面，让你更容易找到同好</text>
					</view>
					<view style="display: flex;flex-direction: row;flex-wrap: warp;">
					<view v-for="(item,index) in myidentity" style="margin-top: 1rem;">
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.identity"  
			v-if="item.identity!=null" @click="deleteshenfen1(index)"> </u-tag>
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.interest"
			v-if="item.interest!=null" @click="deleteshenfen1(index)"> </u-tag>
					</view>
					</view>
				</view>
				<view style="padding-top: 1rem;padding-bottom: 1rem;padding-left: 0.5rem;padding-right: 0.5rem;background-color: white;margin-top: 1px solid #909399;margin-left:2vw;margin-right:2vw" v-if="show7">
					<view>
						<text style="font-size: 14px;color:#909399">我的技能</text>
					</view>
					<view style="display: flex;flex-direction: row;flex-wrap: wrap;">
						<view v-for="(item,index) in identity">
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.identity" plain  v-if="!jugeidentity(index)"
						@click="addshenfen(index,1)"
						> </u-tag>
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.identity"   v-if="jugeidentity(index)"
						@click="deleteshenfen(index,1)"
						> </u-tag>
						</view>
					</view>
				</view>
				<view style="padding-top: 1rem;padding-bottom: 1rem;padding-left: 0.5rem;padding-right: 0.5rem;background-color: white;margin-top: 1px solid #909399;margin-left:2vw;margin-right:2vw" v-if="show7">
					<view>
						<text style="font-size: 14px;color:#909399">我的兴趣</text>
					</view>
					<view style="display: flex;flex-direction: row;flex-wrap: wrap;">
						<view v-for="(item,index) in interest">
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.interest" plain  v-if="!jugeinterest(index)"
						@click="addshenfen(index,2)"
						> </u-tag>
						<u-tag style="margin-top: 0.5rem;margin-right: 10px;"  :text="item.interest"   v-if="jugeinterest(index)"
						@click="deleteshenfen(index,2)"
						> </u-tag>
						</view>
					</view>
				</view>
		</u-popup>
		<u-popup :show="show8" mode="center"  @close="show8=false">
		        <view style="width:300px;height:auto;padding-top: 20px;padding-left: 10px;padding-right: 10px;;padding-bottom: 20px;overflow-y: auto;border-radius: 10px;color:rgb(120, 120, 120);">
					<text>禁用至:{{time1()}}</text><br>
					<view style="display: flex;flex-direction:row">
						<view>
							<text>禁用原因:</text>
						</view>
						<view>
							<text>{{banreason}}</text>
						</view>
					</view>
				
		        </view>
		</u-popup>
		<view style="width:40px;height:40px;border-radius: 50%;display: flex;align-items: center;justify-content: center;background-color: #8bc863;
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
				jugetops:[],
				jugetops1:[],
				jugetops2:[],
				show1:false,
				show2:false,
				themelistnum:0,
				show3:false,
				show4:false,
				birthday:0,
				show6:false,
				show7:false,
				show8:false,
				attentions:[],
				fanss:[],
				attention:0,
				mylike:[],
				attentionlist:[],
				shenfen:"",
				mysavelist:[],
				fans:0,
				userid:-1,
				username:"",
				userbackgroundimage:"",
				op:1,
				sign:"",
				save:0,
				bylike:0,
				registertime:0,
				admin:0,
				icon:"/static/icon.png",
				themelist:[],
				scroll_top:0,
				mylikelist:[],
				open1:0,
				email:"",
				toutu:"",
				key:"",
				attentionpage:0,
				fanspage:0,
				attentionfangdou:0,
				fansfangdou:0,
				limit:0,
				ban:0,
				getmore:true,
				bantime:0,
				banreason:"",
				show:false,
				userheadimage:"",
				identity:[],
				interest:[],
				myidentity:[],
				savefangdou:0,
				likefangdou:0,
				savepage:0,
				likepage:0,
				upload:[
					{
						type: 'success',
						icon: false,
						title: '成功主题',
						message: "设置成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "设置失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "其它设备登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "请登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					}
				],
				replymessageshow:false,
				imagewidth:0,
			}
		},
		onShow() {
			this.imagewidth = uni.getSystemInfoSync().windowWidth*0.4*0.8*0.8*0.33;
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
			this.mylike=getApp().globalData.mylike;
			uni.request({
				url:getApp().globalData.http+"/api/getmyself",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie
				},
				success: (res) => {
					this.birthday=res.data.data.birthday;
					if(this.birthday!=null){
						let date = new Date(parseInt(this.birthday)*1000);
						        let y = date.getFullYear();
						        let MM = date.getMonth() + 1;
						        MM = MM < 10 ? "0" + MM : MM;
						        let d = date.getDate();
						        d = d < 10 ? "0" + d : d;
						      
						        var dayDate = y + "-" + MM + "-" + d;
						this.birthday=dayDate;
					}
			
					this.userid=res.data.data.userid;
					this.username=res.data.data.username;
					this.userheadimage=res.data.data.userheadimage;
					this.userbackgroundimage=res.data.data.userbackgroundimage;
					if(this.userbackgroundimage==""||this.userbackgroundimage==null||this.userbackgroundimage==undefined){
					getApp().globalData.userbackgroundimage='../../static/bg.webp';
					this.userbackgroundimage='../../static/bg.webp'
					}
					this.sign=res.data.data.sign;
					this.save=res.data.data.save;
					this.bylike=res.data.data.bylike;
					this.registertime=res.data.data.registertime;
					this.admin=res.data.data.admin;
					this.email=res.data.data.email;
					this.toutu=res.data.data.toutu;
					this.attention=res.data.data.attention;
					this.fans=res.data.data.fans;
					this.ban=res.data.data.ban;
					this.banreason=res.data.data.banreason;
					this.bantime=res.data.data.ban_time;
					this.show=true;
					if(res.data?.shenfen&&res.data.shenfen.length>0){
						var str="";
						
						for(var j=0;j<res.data.shenfen.length-1;j++){
							str+=res.data.shenfen[j].name+='、';
						}
						str+=res.data.shenfen[res.data.shenfen.length-1].name;
						this.shenfen=str;
						
					}
					var i="";
					for(var j=0;j<this.email.indexOf("@")-4;j++)i=i+'x';
						
					this.email=this.email.substring(0,3)+i+this.email.substring(this.email.indexOf("@")-1,this.email.length);
				uni.setNavigationBarTitle({
						title:this.username
					})
				}
			})
		},
		onLoad() {
			this.mylike=getApp().globalData.mylike;
			this.admin=getApp().globalData.admin;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			uni.request({
				url:getApp().globalData.http+"/api/getmyattention",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie,
					"userid":getApp().globalData.userid
				},
				success: (res) => {
					if(res.data.code==1001){
						getApp().globalData.attention=res.data.data;
						this.attentionlist=getApp().globalData.attention;
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getmysavelist",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie,
					"userid":getApp().globalData.userid,
					"savepage":this.savepage
				},
				success: (res) => {
					if(res.data.code==1001){
						for(var i=0;i<res.data.data.length;i++){
							this.jugetops2.push(false);
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
						if(res.data.data.length==5){
							this.savefangdou=0;
							this.savepage++;
						}else this.savepage=-1;
						this.mysavelist=res.data.data;
						//this.jugetop1();
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getmylikelist",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie,
					"userid":getApp().globalData.userid,
					"likepage":this.likepage
				},
				success: (res) => {
					if(res.data.code==1001){
						for(var i=0;i<res.data.data.length;i++){
							this.jugetops1.push(false);
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
						if(res.data.data.length==5){
							this.likefangdou=0;
							this.likepage++;
						}else this.likepage=-1;
						this.mylikelist=res.data.data;
						//this.jugetop2();
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getthemelistby_user",
				data:{
					"userid":getApp().globalData.userid,
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
					this.limit++;
					this.themelist=res.data.data;
					if(res.data.data.length<10){
						this.getmore=false;
					}
					this.themelistnum=res.data.themelistnum;
					this.jugetop();
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getmyattentionlist",
				data:{
					"a":getApp().globalData.userid,
					"cookie":getApp().globalData.cookie,
					"attentionpage":this.attentionpage
				},
				method:"POST",
				success: (res) => {
					if(res.data.code==1001)
						this.attentions=res.data.data;
					if(res.data.data.length<15){
						this.attentionpage=-1;
						this.attentionfangdou=1;
					}else {
						this.attentionpage++;
						this.attentionfangdou=0;
					}
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getmyfanslist",
				data:{
					"b":getApp().globalData.userid,
					"cookie":getApp().globalData.cookie,
					"fanspage":this.fanspage
				},
				method:"POST",
				success: (res) => {
					if(res.data.code==1001)
						this.fanss=res.data.data;
						if(res.data.data.length<15){
							this.fanspage=-1;
							this.fansfangdou=1;
						}else {
							this.fansspage++;
							this.fansfangdou=0;
						}
				
				}
			})
			
			uni.request({
				url:getApp().globalData.http+"/api/getshenfenlist",
				method:"POST",
				data:{
					
				},
				success: (res) => {
					this.identity=res.data[0];
					this.interest=res.data[1];
					uni.request({
						url:getApp().globalData.http+"/api/getmyshenfen",
						method:"POST",
						data:{
							"userid":getApp().globalData.userid
						},
						success: (res) => {
							for(var i=0;i<res.data.length;i++){
								
								if(res.data[i].type==1){
									var identity="";
									for(var j=0;j<this.identity.length;j++){
									
										if(this.identity[j].id==res.data[i].id){
											identity=this.identity[j].identity;
											break;
										}
									}
									
									var temp={
										"id":res.data[i].id,
										"type":res.data[i].type,
										"identity":identity
									}
									this.myidentity.push(temp);
								}
								else{
									var interest="";
									for(var j=0;j<this.interest.length;j++){
								
										if(this.interest[j].id==res.data[i].id){
											interest=this.interest[j].interest;
											break;
										}
									}
									var temp={
										"id":res.data[i].id,
										"type":res.data[i].type,
										"interest":interest
									}
									this.myidentity.push(temp);
								}
							}
							this.show7=true;
						},
						
					})
				}
			})
		},
		methods: {
			scroll:function(e){
				
				if(e.detail.scrollTop>20){
					this.open1=1;
				}
				else {this.open1=0;}
			},
			
			getmorefans(){
				uni.request({
					url:getApp().globalData.http+"/api/getmyfanslist",
					data:{
						"b":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"fanspage":this.fanspage
					},
					method:"POST",
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<res.data.data.length;i++)
							this.fanss.push(res.data.data[i]);
							if(res.data.data.length<15){
								this.fanspage=-1;
								
							}else {
								this.fanspage++;
								this.fansfangdou=0;
								}
						}
					}
				})
			},
			getmoreattention(){
				if(this.attentionfangdou==1)return ;this.attentionfangdou=1;
				uni.request({
					url:getApp().globalData.http+"/api/getmyattentionlist",
					data:{
						"a":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"attentionpage":this.attentionpage
					},
					method:"POST",
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<res.data.data.length;i++)
							this.attentions.push(res.data.data[i]);
							if(res.data.data.length<15){
								this.attentionpage=-1;
							}else {
								this.attentionpage++;
								this.attentionfangdou=0;
							}
						}
					}
				})
			},
			getmoresave(){
				if(this.savefangdou==1)return ;this.savefangdou=1;
				uni.request({
					url:getApp().globalData.http+"/api/getmysavelist",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"savepage":this.savepage
					},
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<res.data.data.length;i++){
								this.jugetops2.push(false);
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
								this.mysavelist.push(res.data.data[i]);
							}
							if(res.data.data.length==5){
								this.savefangdou=0;
								this.savepage++;
							}else this.savepage=-1;
						}
					}
				})
			},
			backtop:function(){
				this.scroll_top=Math.random();
			},
			getmorelike(){
				if(this.likefangdou==1)return ;this.likefangdou=1;
				uni.request({
					url:getApp().globalData.http+"/api/getmylikelist",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"likepage":this.likepage
					},
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<res.data.data.length;i++){
								this.jugetops1.push(false);
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
								this.mylikelist.push(res.data.data[i]);
							}
							if(res.data.data.length==5){
								this.likefangdou=0;
								this.likepage++;
							}else this.likepage=-1;
						}
					}
				})
			},
			beian:function(){
				window.location.href="https://beian.miit.gov.cn/"
				
			},
			jugetop1:function(){
				let that=this;
				setTimeout(() => {  
				for(var i=0;i<this.mysavelist.length;i++){
					let info = uni.createSelectorQuery().select("#s"+i);
					　　　  　info.boundingClientRect(function(data) { //data - 各种参数
					
					　　　  　if(data.height>500)that.jugetops2[i]=true;  // 获取元素宽度
				
					}).exec()
					
				}this.$set(this.jugetops2,this.jugetops2);
				} , 100)
			},
			jugetop2:function(){
				let that=this;
				setTimeout(() => {  
				for(var i=0;i<this.mylikelist.length;i++){
					let info = uni.createSelectorQuery().select("#l"+i);
					　　　  　info.boundingClientRect(function(data) { //data - 各种参数
					
					　　　  　if(data.height>500)that.jugetops1[i]=true;  // 获取元素宽度
				
					}).exec()
					
				}this.$set(this.jugetops1,this.jugetops1);
				} , 100)
			},
			time1(){
				if(this.bantime!=0){
					var now = new Date(this.bantime * 1000); // 依情况进行更改 * 1
						var y = now.getFullYear();
						var m = now.getMonth() + 1;
						var d = now.getDate();
						        return y + "-" + (m < 10 ? "0" + m : m) + "-" + (d < 10 ? "0" + d : d) + " " + now.toTimeString().substr(0, 8);
					
				}
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
			changepassword:function(){
				uni.navigateTo({
					url:"changepassword"
				})
			},
			getmoretheme:function(){
				if(this.getmore==false)return;
				this.getmore=false;
				uni.request({
					url:getApp().globalData.http+"/api/getthemelistby_user",
					data:{
						"userid":getApp().globalData.userid,
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
			jugelike:function(res){
				//console.log(this.mylike.length);
				this.mylike=getApp().globalData.mylike;
				for(var i=0;i<this.mylike.length;i++)
					if(this.mylike[i]==res)return true;return false
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
			jugeattention:function(e){
				this.attentionlist=getApp().globalData.attention;
				for(var i=0;i<this.attentionlist.length;i++){
					if(e==this.attentionlist[i])
						return true;
				}
				return false;
			},
			jugecookie:function(){
				if(getApp().globalData.usercookie!='')return true;return false;
			},
			linkmyself:function(){
				uni.navigateTo({
					url:"myself"
				})
			},
			confirm:function(value) {
				this.$refs.popup.close();
				uni.request({
					url:getApp().globalData.http+"/api/setsign",
					method:'POST',
					data:{
						"sign":value,
						"cookie":getApp().globalData.cookie
					},
					success:(res) =>{
						if(res.data.code==1001){
							this.sign=value;
							this.$refs.uToast.show({
								...this.upload[0],
								complete() {
									return;
								}
							})
						}
					},
					fail: (res) => {
						this.$refs.uToast.show({
							...this.upload[1],
							complete() {
								return;
							}
						})
					}
				});
			},
			open:function() {
			         this.$refs.popup.open()
			        },
			close:function(){
					 this.$refs.popup.close()
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
			time2:function(e){
				var d = new Date(e*1000);
				var year = d.getFullYear();
				var month = d.getMonth() + 1; //取得日期中的月份，其中0表示1月，11表示12月
				var date = d.getDate(); 
				return year+"-"+month+"-"+date;
			},

			head:function(){
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: function(res) {
						//console.log(res.tempFilePaths);
						uni.uploadFile({
						    url:"https://www.txtz.club:8808/api/upload",		//post请求的地址
						    filePath:res.tempFilePaths[0],
						    name:'file',	
						    success: (res) => {
									 res.data = JSON.parse(res.data)
									
								uni.request({
									url:getApp().globalData.http+"/api/setuserheadimage",
									method:"POST",
									data:{
										"userheadimage":res.data.location,
										"userid":getApp().globalData.userid,
										"cookie":getApp().globalData.cookie
									},
									success: (res1) => {
										if(res1.data.code==1001){
											getApp().globalData.userheadimage=res.data.location;
											this.userheadimage=getApp().globalData.userheadimage;
											uni.setStorage({
											    key: "userheadimage",
											    data: getApp().globalData.userheadimage,
											    success: function () {
											    }
											});			
											uni.reLaunch({
												url:"myself"
											})
										}
										else{
											this.$refs.uToast.show({
												...this.upload[1],
												complete() {
													return;
												}
											})
										}
									},
									fail:(res)=>{
										this.$refs.uToast.show({
											...this.upload[1],
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
			linktomessage:function(){
				uni.navigateTo({
					url:"./message"
				})
			},
			settoutu:function(){
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: function(res) {
						//console.log(res.tempFilePaths);
						uni.uploadFile({
						    url:"https://www.txtz.club:8808/api/upload",		//post请求的地址
						    filePath:res.tempFilePaths[0],
						    name:'file',	
						    success: (res) => {
									 res.data = JSON.parse(res.data)
									
								uni.request({
									url:getApp().globalData.http+"/api/setusertoutu",
									method:"POST",
									data:{
										"toutu":res.data.location,
										"userid":getApp().globalData.userid,
										"cookie":getApp().globalData.cookie
									},
									success: (res1) => {
										if(res1.data.code==1001){
											this.toutu=res.data.location;		
											uni.reLaunch({
													url:"myself"
											})
										}
										else{
											this.$refs.uToast.show({
												...this.upload[1],
												complete() {
													return;
												}
											})
										}
									},
									fail:(res)=>{
										this.$refs.uToast.show({
											...this.upload[1],
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
			save1:function(){
				var temp=[];
				let that=this;
				for(var i=0;i<this.myidentity.length;i++){
					temp.push({
						"type":this.myidentity[i].type,
						"id":this.myidentity[i].id
					})
				}
				uni.request({
					url:getApp().globalData.http+"/api/setmyshenfen",
					method:"POST",
					data:{
						"data":temp,
						"userid":getApp().globalData.userid
					},
					success: (res) => {
						if(res.data.code==1001){
							that.show6=false;
							uni.showToast({
								icon:'success',
								title:"保存成功"
							})
							setTimeout(() => {
								this.$router.go(0)
							}, 400)
						}
						else{
							uni.showToast({
								icon:'error',
								title:"保存失败"
							})
						}
					},
					fail: (res) => {
						uni.showToast({
							icon:'error',
							title:"保存失败"
						})
					}
				})
			},
			back(){
				uni.navigateBack({
					delta:1
				})
			},
			deleteshenfen1:function(e){
				this.myidentity.splice(e,1);
			},
			deleteshenfen:function(index,type){
				if(type==1){
					for(var i=0;i<this.myidentity.length;i++){
						if(this.myidentity[i].type==1){
							if(this.myidentity[i].id==this.identity[index].id)
								this.myidentity.splice(i,1);
						}
					}
				}
				else{
					for(var i=0;i<this.myidentity.length;i++){
						if(this.myidentity[i].type==2){
							if(this.myidentity[i].id==this.interest[index].id)
								this.myidentity.splice(i,1);
						}
					}
				}
			},
			addshenfen:function(index,type){
				if(this.myidentity.length==3)return;
				if(type==1){
					this.identity[index].type=1;
					var temp=this.identity[index];
				
					this.myidentity.push(temp);
				}
				else{
					this.interest[index].type=2;
					var temp=this.interest[index];
					this.myidentity.push(temp);
				}
			},
			jugeidentity:function(e){
				for(var i=0;i<this.myidentity.length;i++){
					
					if(this.myidentity[i].type==1){
						if(this.myidentity[i].id==this.identity[e].id){
							
							return true;
						}
					}
				}
				return false;
			},
			jugeinterest:function(e){
				for(var i=0;i<this.myidentity.length;i++){
					if(this.myidentity[i].type==2){
						if(this.myidentity[i].id==this.interest[e].id){
						
							return true;
						}
					}
				}
					
				return false;
			}
		}
	}
</script>

<style>

</style>
