<template>
	<view @mousemove="test1" class="body1" style="
	-webkit-transform:transition3d/translateZ;
	background-size: 100%;back;
	background-color: #f8f8f8;
	width:100vw;height:100vh;
	overflow: hidden"	 
	:style="{'--weizhi':backweizhi,'--transition':backmiao+'s','--back':'url('+userbackgroundimage+')'}"
	@mouseenter="mouseenter1" @mouseleave="mouseleave1"
	>
		<u-transition duration="500" :show="!show" mode="fade" style="position:absolute;"
		
		> 
		<view style="width:100vw;height:100vh;position:absolute;background-size: cover;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}"
		v-if="!show"
		>
		</view>
		</u-transition>
		<u-transition duration="500" :show="!show" mode="fade-down" style="z-index: 3;" 
		
		> 
		<titles style="width:100vw;" :username="username" :userheadimage="userheadimage" :admin="admin"
		:replymessageshow="replymessageshow"
		></titles>
		</u-transition>
		<!--center-->
		<view style=";height:92vh;overflow-y:hidden;width:90vw;padding-left: 5%;padding-right: 5%;border-radius:5px;background-size: cover;background-attachment: fixed;;background-color: #f8f8f8;
		padding-top: 8vh;display: flex;background-color: rgba(255,255,255,0);" 
		>
		<vue-particles
		v-if="shapecolor.length>0&&!show"
		  :color="shapecolor"
		  :particleOpacity="0.7"
		  :particlesNumber="40"
		  :shapeType="shapetype"
		  :particleSize="3"
		  linesColor="#FFFFFF"
		  :linesWidth="3"
		  :lineLinked="false"
		  :lineOpacity="0.4"
		  :linesDistance="150"
		  :moveSpeed="1"
		  :hoverEffect="false"
		  hoverMode="grab"
		  :clickEffect="false"
		  clickMode="push"
		  class="cash"
		 style="width:100%;height:100%;z-index:0;top:0;position:absolute;left:0"
		>
		</vue-particles>
			<!--left-->
			<view style="width: 20%;z-index:2;overflow-y: auto;">
			<u-transition :show="transition1&&show==false" mode="fade-left" style="100%">
			<view style="width:95%;padding-left:2.5%;padding-right:2.5%;height:auto; background-color:white;border-radius: 5px;overflow-y: auto;padding-bottom: 10%;" :style="{'opacity':op}">
				<view v-for="(item,index) in leftlist" style=";" @click="getmore=true;themelist=[];limit=0;getthemelistby_fenlei(item,index),select_fenlei=index" >
					<view style="width:100%;height:25px;display: flex;flex-direction: row;padding-top: 18px;padding-bottom: 18px;color:#818181;align-items: center;">
					<text class="text-blue" style="font-size: 19px;margin-left: 10px;"
					:class="{'text_select':index==select_fenlei}"
					>{{item.title}}</text>
					<text style="margin-left: auto;font-size: 16px;margin-right: 10px;">{{item.num}}</text>
					</view>
					<view style="width:90%;height:0.5px;background-color: #e6e6e6;margin:0 auto"></view>
				</view>
			</view >
			</u-transition>
			<u-transition :show="(juge1()&&show==false)||(a111&&show==false)" mode="fade-left" style="width:100%;height:100px;">
			<view  style="width:95%;height:90px;margin-top:20px; background-color:white;padding-left:5%;padding-bottom: 12%;border-radius: 5px;padding-top:10px;"
			>
			<view style="display:flex;flex-direction: row;">
				<view>
					<image src="../../static/二维码.png" style="width:90px;height:90px"></image>
				</view>
				<view style="margin-left: 5px;display:flex;justify-content: center;flex-direction: column;">
					<view>
					<text style="font-size: 22px;font-weight:800;">下载移动端</text>
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
			</view></u-transition>
			</view>
			<!--center-->
		<u-transition duration="500" :show="transition2&&show==false" mode="fade" style="height:auto;width:56%;margin-left:2%;margin-right:2%;border-radius:5px 5px 0 0;"
		@click="con1" id="scroll2"
		>	  
		<scroll-view  id="scroll1" style="display:flex;justify-content: center;;width:100%;;height:100vh; ;margin-left: 0%;margin-right: 0%;border-radius: 5px 5px 0 0;overflow-y: auto;"
		:style="{'opacity':op}"  scroll-y :scroll-top="scroll_top" @scroll="scroll" :scroll-with-animation="true"
		@scrolltoupper="scroll_top=0" @scrolltolower="getthemelistby_fenlei()" :scroll-into-view="scrollid" class="scroll"
		>
		
				<view v-if="firstopen()" style="border-radius: 5px;overflow: hidden;" id="ida1">
				  <u-notice-bar :text="message[0]" mode="closable"></u-notice-bar>
				</view>
				<view id="ida2" style="width:96%;font-size: 19px;padding-top: 10px;padding-left: 2%;background-color:white;border-radius: 5px;padding-right: 2%;">
					<view style="display: flex;flex-wrap: row;">
						<view class="text-blue" style="padding:7px;color:#676767">
						<text :class="{'text_select':type==0}" @click="getmore=true;themelist=[];limit=0;type=0;getthemelistby_fenlei(leftlist[select_fenlei],select_fenlei)">全部</text>
						</view>
						<view class="text-blue" style="padding:7px;color:#676767">
						<text  :class="{'text_select':type==1}" @click="getmore=true;themelist=[];limit=0;type=1;getthemelistby_fenlei(leftlist[select_fenlei],select_fenlei)">精华</text>
						</view>
						<view class="text-blue" style="padding:7px;color:#676767">
						<text  :class="{'text_select':type==2}" @click="getmore=true;themelist=[];limit=0;type=2;getthemelistby_fenlei(leftlist[select_fenlei],select_fenlei)">已关注</text>
						</view>
						<view style="margin-left: auto;width:10%;margin-right: 0%;">
							<u-button type="primary"  text="发布" @click="newtheme()"></u-button>
						</view>
					</view>
					<view style="width:100%;background-color: #e6e6e6;height:1px;margin-top: 15px;margin-bottom: 15px;" id="ida3"></view>
					<!--顶置贴-->
					<view style="min-height: 10px;padding-bottom: 10px;;background-color: white;" id="ida4">
						<view v-for="(item,index) in toplist" style="display: flex;flex-wrap: row;
						padding-right:5%;font-size: 16px;margin-bottom: 5px;
						"
						@click="link(item.themeid)"
						>
							<text style="margin-right: 2%;color:#838383">置顶</text>
							<text style="margin-right: 1%;">{{item.title}}</text>
							<!--<u--text size="16" :lines="1" :text="item.data"></u--text>-->
						</view>
					</view>	
				</view>
				<!--主题-->

				<view v-for="(item,index) in themelist" :id="'id'+item.themeid" style="transition:0.3s" :style="{'opacity':op}"  v-if="themelist.length!=0"
				:class="{'class1':jugeid==item.themeid,'class2':jugeid!=item.themeid}"
				>
				
					<view  style="width:96%;height:auto;margin-top: 15px;margin-bottom: 15px;border-radius: 5px;background-color: white;padding:2%;
					padding-left:2%;padding-right: 2%;
					">
						<view style="width: 100%;padding: 0%;display: flex;flex-direction: row;;
						margin-left: 0%;
						">
							<view style="width:50px;margin-right: 5px;height:45px;display: flex;align-items: center;">
							<u-avatar mode="aspectFill":src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)"  randomBgColor v-if="item.userheadimage==null" size="45"></u-avatar>
							</view>
							<view style="height:45px;display: flex;flex-direction: column;margin-left: 0px;margin-bottom: 10px;width: 100%;">
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
										<text style="color:#838383;font-size: 15px;margin-left: 2px;">{{item.look}}</text>
									</view>
								</view>
							</view>
						</view>
						<view style="width:90%;margin-left: 5%;margin-right: 5%;display: flex;flex-direction: column;align-items: center;" @click="link(item.themeid)">
							<text style="font-size: 20px;font-weight: 700;margin-bottom:20px">
								{{item.title}}
							</text>
							
							<mp-html
							 :id="'t'+index"
							:content="item.text" 
							style="max-height: 502px;overflow-y: hidden;width:100%;margin-bottom: 25px;"
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
									height="600px"
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
						<view v-if="item.position!=undefined&&item.lal!=''" style="margin-left: 2vw;margin-top:20px;margin-bottom:20px">
						<u--text  @click="weizhi(item)" prefixIcon="map-fill" :text="item.position" size="15"  color="#8bc863" iconStyle="color:#8bc863;margin-right:5px;size:17px"></u--text>
						</view>
						<view style="display: flex;flex-direction: row;padding-left: 2vw;margin-top: 10px;">
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
							    :status="status" 
							    loading-text="努力加载中" 
							    loadmore-text="轻轻上拉" 
							    nomore-text="实在没有了" 
								v-if="index==themelist.length-1"
								style="margin-top:10px"
						/>
					</view>
				</view>
					<view style="height:100px">
						
					</view>
			
		</scroll-view>
		</u-transition>	
			<!--right-->
		<view style="width:22%;height:100%;z-index:2;overflow-y: auto;">
			
			<view
			 v-if="activity.length>0"
			 style="width:100%;margin-top:0px;margin-bottom: 20px;border-radius: 5px;overflow: hidden;">
			 <u-transition :show="activity.length>0&&show==false" mode="fade-right" style="width:100%">
			<Xsuu-swiper :swiperItems="activity"
			style="width:100%;"
			:height="300"
			 ></Xsuu-swiper>
			</u-transition>
			 </view>
			<u-transition :show="tuijianlist.length>0&&show==false" mode="fade-right" style="width:100%;height:400px;max-height: 60vh;padding-bottom:0%;">
			<view  style="width:90%;height:350px;max-height: 60vh; background-color:white;padding-left:5%;padding-right: 5%;border-radius: 5px;padding-top:10px;padding-bottom: 5%;"
			:style="{'opacity':op}"
			>
				<view style="margin-top: 10px;margin-left: 0%;">
					<text style="font-weight: 700;font-size: 18px;">推荐内容</text>
					<view style="width:100%;background-color: #e6e6e6;height:1px;margin-top: 15px;margin-bottom: 15px;"></view>
				</view>
				<view style="max-height: 70%;margin-bottom:0%;">
				<view v-for="(item,index) in tuijianlist" style="display: flex;padding:2%;flex-direction: column;margin-bottom: 10px;" v-if="index<3" @click="link(item.themeid)">
					<view style="display: flex;flex-direction: row;align-items: center;">
					<text style="font-size: 12;color:#ee5b20;margin-right: 5%;">{{index+1}}</text>
					<u--text v-if="item.title!=null&&item.title.length>0" :text="item.title"  :lines="1"></u--text>
					<u--text v-if="item.title==null||item.title.length==0" :text="item.data"  :lines="1"></u--text>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-top: 5px;;margin-left: -3px;">
					<u-icon name="eye" size="14"></u-icon>
					<text style="color:#838383;font-size: 14px;margin-left: 3px;">{{item.look}}</text>
					<u-tag text="精华"   size="medium" type="error"  style="margin-left: auto;" :plainFill="true" :plain="true" ></u-tag>
					</view>
					<view style="width:100%;background-color: #e6e6e6;height:1px;margin-top: 8px;margin-bottom: 0px;"></view>
				</view>
				</view>
				<view style="width: 100%;bottom:0px;display: flex;align-items: center;flex-direction: row;justify-content: center;">
					<u-icon name="reload" size="16"></u-icon>
					<text style="font-size: 16px;color:#838383;margin-left: 5px;" @click="suijituijian()">换一批</text>
				</view>
			</view>
			</u-transition>
			<u-transition :show="(juge1()&&show==false)&&activity.length>0||(a111&&show==false)&&activity.length>0" mode="fade-right" style="width:100%;height:auto;;max-height: 60vh;;margin-top:0px">
			<view  style="width:90%;margin-bottom: 0px;;height:auto;max-height: 60vh; background-color:white;padding-left:5%;padding-right: 5%;border-radius: 5px;"
			
			>
			<swiper style="width:100%;height:300px" circular autoplay>
				<swiper-item>
					<view style="font-family: zhuzi;width:100%;height:90%;padding-top:5%;padding-bottom:5%;display: flex;flex-direction: column;align-items: center;">
					<text style="
					
					font-size: 18px;font-weight: 700;margin-bottom: 10px;margin-top: 5px;margin-left:-18px">🌐 域名链接</text>
					<view>
					<view style="
					display: flex;justify-content: center;
					font-size: 17px;font-weight: 700;margin-bottom: 5px;margin-top: 5px;margin-left:-18px">
					🔗 主域名
					</view>
					
					<view style="margin-bottom: 5px;display: flex;;flex-direction:column;width:100%;;align-items: center;
					">
						<text style=";color:#8bc863;"
						@click="linkto('begonia.cafe')"
						class="text1"
						>
							mingqian.begonia.cafe
						</text>
						<text style=";color:#8bc863;"
						@click="linkto('begonia.cafe')"
						class="text1"
						>
							www.begonia.cafe
						 </text>
						 <text style=";color:#8bc863;"
						 @click="linkto('begonia.cafe')"
						 class="text1"
						 >
						 	begonia.cafe
						  </text>
					</view>
					<view style="
					display: flex;justify-content: center;
					font-size: 17px;font-weight: 700;margin-bottom: 5px;margin-top: 5px;margin-left:-18px">
					🔗 备用域名
					</view>
					<view style="margin-bottom: 5px;display: flex;flex-direction:row;width:100%;justify-content: center;
					">
						<text style=";color:#8bc863;"
						@click="linkto('blog.txtz.link')"
						class="text1"
						>
							blog.txtz.link
						 </text>
					</view>
					<view style="
					display: flex;justify-content: center;
					font-size: 18px;font-weight: 700;margin-bottom: 5px;margin-top: 5px;margin-left:-18px">
					📝 备案号
					</view>
					<view style="margin-bottom: 5px;display: flex;flex-direction:row;width:100%;justify-content: center;
					">
						<text style=";color:#8bc863;"
					 @click="beian()"
						class="text1"
						>
							吉ICP备2021004411号
						 </text>
					</view>
					</view>
				</view>
				</swiper-item>
				<swiper-item>
					<view style="width:100%;display: flex;flex-direction: column;">
						<view style="width:100%;display: flex;align-items: center;justify-content: center;margin-bottom: 5px;">
							<text style=";font-size: 18px;font-weight: 700;margin-bottom: 15px;margin-top: 15px;;margin-left:-18px;font-family: zhuzi;">🖧 技术使用</text>
						</view>
					<view style="width:100%;display: flex;flex-direction: row;flex-wrap: wrap;">
						<view>
							<view style="margin-top:10px;height:20px;width:auto;margin-right:10px;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>📑 Web</text>
								</view>
								<view style="background-color: #00aa00;color:white;display: flex;align-items: center;justify-content: center;padding:10px;border-radius:0px 3px 3px 0px">
									<a class="a3" title="Uniapp 框架" style="text-decoration:none;color:white" href="https://uniapp.dcloud.net.cn/" >Uni-app</a>
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>📟 Frame</text>
								</view>
								<view style="background-color: #5555ff;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="Spring Boot框架" style="text-decoration:none;color:white" href="https://spring.io/" >Spring Boot</a>
								</view>
							</view>
						</view>
						
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🗏 Editor</text>
								</view>
								<view style="background-color: #75b000;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="Tinymce HTML可视化编辑器" style="text-decoration:none;color:white" href="http://tinymce.ax-z.cn/" >Tinymce</a>
								</view>
							</view>
						</view>
						
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🗄 DataBase</text>
								</view>
								<view style="background-color: #bc0000;color:white;display: flex;align-items: center;justify-content: center;padding:10px;border-radius:0px 3px 3px 0px">
									<a class="a3" title="关系型数据库 MySQL" style="text-decoration:none;color:white" href="https://www.mysql.com/" >MySQL</a>
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🧾 NoSQL</text>
								</view>
								<view style="background-color: #aa5500;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="非关系型数据库 Redis" style="text-decoration:none;color:white" href="https://redis.io/" >Redis</a>
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🛢️ Bucket</text>
								</view>
								<view style="background-color: #00aaff;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="COS 腾讯云对象存储" style="text-decoration:none;color:white" href="https://cloud.tencent.com/product/cos" >COS</a>
									
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🖧 CDN</text>
								</view>
								<view style="background-color: #ffaa7f;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="CDN 腾讯云内容分发网络" style="text-decoration:none;color:white" href="https://cloud.tencent.com/product/cdn" >Tencent CDN</a>
									
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🖧 System</text>
								</view>
								<view style="background-color: #aaaaff;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="CentOS" style="text-decoration:none;color:white" >Centos</a>
									
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>📈 DataAnalysis</text>
								</view>
								<view style="background-color: #df4adf;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="数据分析存储引擎 ElasticSearch" style="text-decoration:none;color:white" href="https://github.com/elastic/elasticsearch" >Elastic Search</a>
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🔗 DNS</text>
								</view>
								<view style="background-color: #aaaa7f;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="DNSPod 腾讯云域名管理" style="text-decoration:none;color:white" href="https://dnspod.cloud.tencent.com/" >Tencent DNS</a>
									
								</view>
							</view>
						</view>
						<view style="margin-right:10px;margin-top: 10px;">
							<view style="height:20px;width:auto;border-radius:5px;font-size: 12px;flex-direction: row;display: flex;overflow: hidden;">
								<view style="background-color: #595b5f;color:white;display: flex;align-items: center;justify-content: center;padding:10px">
									<text>🖥 Server</text>
								</view>
								<view style="background-color: #85c7c7;color:white;display: flex;align-items: center;justify-content: center;padding:10px;;border-radius:0px 3px 3px 0px">
									<a class="a3" title="轻量应用服务器" style="text-decoration:none;color:white" href="https://cloud.tencent.com/product/lighthouse" >Lighthouse</a>
									
								</view>
							</view>
						</view>
				</view>
					</view>
				</swiper-item>
				<swiper-item>
				<view style="width:100%;display: flex;flex-direction: column;;align-items: center;font-family: zhuzi;">
					<text style=";font-size: 18px;font-weight: 700;margin-bottom: 10px;margin-top: 15px;;margin-left:-18px">📈 网站统计</text>
					<view style="width:100%;display:flex;flex-direction: row;align-items: center;justify-content: center;margin-top:5px">
						<view style="margin-left:3%;font-size:18px;width:50%">今日访客：{{siteinfo.todayfangke}}</view>
						<view style="margin-left:auto;font-size: 18px;width:50%">文章总数：{{siteinfo.totlethemenum}}</view>
					</view>
					<view style="width:100%;display:flex;flex-direction: row;align-items: center;justify-content: center;margin-top:10px">
						<view style="margin-left:3%;font-size: 18px;;width:50%">今日文章：{{siteinfo.todaynewtheme}}</view>
						<view style="margin-left:auto;font-size: 18px;;width:50%;">今日评论：{{siteinfo.todaynewreply}}</view>
					</view>
					<view style="width:100%;display:flex;flex-direction: row;align-items: center;justify-content: center;margin-top:10px">
						<view style="margin-left:3%;font-size: 18px;;width:50%">今日活跃：{{siteinfo.todayhotuser}}</view>
						<view style="margin-left:auto;font-size: 18px;;width:50%">精华文章：{{siteinfo.jingnum}}</view>
					</view>
					<text style="margin-top:15px;margin-bottom:10px;font-size: 17px;margin-top:20px">{{nowDate}} </text>
					<text style="font-size: 17px;">论坛已运行{{rundata}}</text>
					<view style="display: flex;justify-content: center;">
					<text style=";color:#8bc863;margin-top:15px;margin-right: 15px;"  class="text1" @click="about1=true,show=true,backmiao1()">💻关于网站 </text>
					<text style=";color:#8bc863;margin-top:15px;margin-left:15px"  class="text1" @click="about2=true,show=true,backmiao1()"> 🍵一点闲话</text>
					</view>
				</view>
				</swiper-item>
				<swiper-item>
					<view style="width:100%;display: flex;flex-direction: column;;align-items: center;font-family: zhuzi;">
						<text style=";font-size: 18px;font-weight: 700;margin-bottom: 25px;margin-top: 15px;;margin-left:-18px;">📓 更新日志</text>
						<view style="width:100%;display: flex;flex-direction: column;" v-for="(item,index) in notes">
							<view style="margin-bottom:15px">
								<view style="width:100%;display: flex;flex-direction: row;align-items: center;">
									<view style="background-color: #8bc863;border-radius: 50%;width:10px;height:10px;">
										
									</view>
									<view style="margin-left: 10px;height:15px">
										{{item.time.split("T")[0]}}
									</view>
								</view>
								<view style="margin-top: 15px;margin-left:20px">
										{{item.note}}
								</view>
							</view>
						</view>
					</view>
				</swiper-item>
				<swiper-item>
					<view style="width:100%;display: flex;flex-direction: column;;align-items: center;font-family: zhuzi;">
						<view style=";font-size: 18px;font-weight: 700;margin-bottom: 10px;margin-top: 15px;display: flex;flex-direction: row;align-items: center;justify-content: center;">
							<image style="width:30px;height:30px;margin-right:2px" src="../../static/book.png"></image>
							<text style="margin-left: 2px;">明前奶绿</text>
						</view>
						<view style="margin-top:15px;width:100%;height:200px;background-color:#e6ffe6;border-radius: 15px;display: flex;flex-direction: row;"
						@click="tonailv()"
						>
							<view style="width:50%;height:100%;display: flex;align-items:flex-end;">
								<image src="https://txtzz-1301452902.file.myqcloud.com/nailv.webp" style="width:100%" mode="widthFix"></image>
							</view>
							<view style="width:50%;height:100%;display: flex;flex-direction: column;align-items: flex-start;justify-content: center;margin-top:15px">
								<text>姓名:明前奶绿</text>
								<text style="margin-top:5px;font-family: zhuzi;">年龄:19</text>
								<text style="margin-top:5px;font-family: zhuzi;">身高:163cm</text>
								<text style="margin-top:5px;font-family: zhuzi;">生日:??年3月4日</text>
								<text style="margin-top:5px;font-family: zhuzi;">星座:双鱼座</text>
								<text style="margin-top:5px;font-family: zhuzi;">职业:拉普拉斯花店主</text>
							</view>
						</view>
					</view>
				</swiper-item>
			</swiper>
			</view>
			</u-transition>
			</view>
			
		</view>
		<u-toast ref="uToast"></u-toast>
		<u-transition duration="500" :show="transition4&&!show" mode="fade-up" style="position: fixed;bottom:20px;z-index: 5;right:20px">
		<view style="width:40px;height:40px;border-radius: 50%;display: flex;align-items: center;justify-content: center;background-color: #8bc863;
		;transition-duration:0.3s ;transition:0.3s;opacity: 1;"
		 @click="backtop()"  v-show="scroll_top2==0"
		>
			<u-icon name="arrow-up-fill" size="30" style="" color="#ffffff"></u-icon>
		</view>
		<view style="width:40px;height:40px;border-radius: 50%;display: flex;align-items: center;justify-content: center;background-color: #8bc863;
		;transition-duration:0.3s ;transition:0.3s;opacity: 1;"
		@click="showopen()"   v-if="scroll_top2>0"
		>
			<u-icon name="arrow-up-fill" size="30" style="" color="#ffffff"></u-icon>
		</view>
		</u-transition>
		<!-------->
		<view style="width:100%;height:100vh;position: absolute;transition-duration:1.25s;top:0
		background: linear-gradient(to bottom right, rgb(124, 163, 255) , rgb(16, 216, 255));
		"
		:style="{zIndex:zindex}"
		@click.prevent="showclose()"
		>
		<u-transition :show="show&&!about2&&!about1" mode="fade">
		<view  referrer="no-referrer|origin|unsafe-url"  :style="{'backgroundImage':'url(' + showback.url+ ')'}" style="	background-size: cover;width:100vw;height:100vh;background-position:center;z-index:1"
		>
		<vue-particles
			v-if="shapecolor.length>0&&show"
		      :color="shapecolor"
		      :particleOpacity="0.7"
		      :particlesNumber="40"
		      :shapeType="shapetype"
		      :particleSize="3"
		      linesColor="#FFFFFF"
		      :linesWidth="3"
		      :lineLinked="false"
		      :lineOpacity="0.4"
		      :linesDistance="150"
		      :moveSpeed="1"
		      :hoverEffect="false"
		      hoverMode="grab"
		      :clickEffect="false"
		      clickMode="push"
		      class="cash"
			 style="width:100%;height:100%"
		    >
		    </vue-particles>
			<view style="position:absolute;z-index:2;background-color: rgba(0,0,0,0.45);display: flex;flex-direction: row;justify-content:center;align-items: center;height:100vh;width:100vw;position:absolute;top:0;left:0;z-index:3">

		  <view style="display: flex;flex-direction:column;align-items:center;transition: 0.3s;
		  font-family:zhuzi
		  "
		   >
		    <!---->
			<view style="width:100vw;height:100vh;position: absolute;top:0;left:0;display: flex;align-items: center;transition: 1s;"
			 v-if="!showpixivbg"
			>
						<img style="height:80vh;width:80vw;margin-bottom:0;width:100%;object-fit: contain;margin-top: 20vh;
						
						" 
						:src="musiclist[musicid].charaimage" 
						>
							
						</img>
						<img style="margin-top: 20vh;;height:80vh;width:80vw;margin-bottom:0;width:100%;object-fit: contain;;position: absolute;z-index: 2;transition: 0.5s;" 
						:class="{'charaop1':charaop1==1,'charaop2':charaop1==0}"
						src="https://txtzz-1301452902.file.myqcloud.com/milky-green-v1.webp">
							
						</img>
						<view style="position: absolute;width:100vw;height:100vh;display: flex;align-items: center;justify-content: center;">
						<view
						class="beiguang3" 
						style=";bottom:0;filter:blur(100px);position:absolute;z-index:-1;transition: 2s;"
						:class="{'charaop1':charaop2==0,'charaop2':charaop2==1}"
						 >
							
						</view>
						<view :class="{'heise3':charaop1==0,'heise4':charaop1==1}" style="transition-delay: .25s;position: absolute;z-index: -2;width:100vw;height:100vh;top:0;left:0;transition: 1s;">
							
						</view>
				</view>
			</view>
			<view style="width:100vw;height:100vh;position: absolute;top:0;left:0;display: flex;align-items: center;z-index:5;justify-content:center;
			"
			class="yangguang"
			:style="{'--weizhi':backweizhi1,}"
			v-if="!showpixivbg"
			>
					<view style="height:300vh;width:50px;background: linear-gradient(to right ,rgba(255, 254, 205, 0.05) 5%,  rgba(255, 254, 205, 0.06) 50%,rgba(255,254,205,0.05) 95%);margin-left:-800px;
					transform:rotate(-60deg);filter: blur(8px);transition: 0.5s;
					" 
					:class="{'charaop1':charaop1==0,'charaop2':charaop1==1}"
					>
						
					</view>
					<view style="height:300vh;width:70px;background: linear-gradient(to right ,rgba(255, 254, 205, 0.05) 5%,  rgba(255, 254, 205, 0.06) 50%,rgba(255,254,205,0.05) 95%);margin-left: 1000px;
					transform:rotate(-60deg);filter: blur(5px);transition: 0.5s;
					"
					:class="{'charaop1':charaop1==0,'charaop2':charaop1==1}"
					>
						
					</view>

				</view>
				<view style="width:100vw;height:100vh;position: absolute;top:0;left:0;display: flex;align-items: center;z-index:5;justify-content:center;
				translate(0,0,-50px)
				"
				class="yangguang"
				:style="{'--weizhi':backweizhi2,}"
				v-if="!showpixivbg"
				>
					<view style="height:300vh;width:100px;background: linear-gradient(to right ,rgba(255, 254, 205, 0.05) 5%,  rgba(255, 254, 205, 0.07) 50%,rgba(255,254,205,0.05) 95%);margin-left: 400px;
					transform:rotate(-60deg);filter: blur(8px);transition: 0.5s;
					"
					:class="{'charaop1':charaop1==0,'charaop2':charaop1==1}"
					>
						
					</view>
					
					<view style="height:300vh;width:250px;background: linear-gradient(to right ,rgba(255, 254, 205, 0.05) 5%,  rgba(255, 254, 205, 0.1) 50%,rgba(255,254,205,0.05) 95%);margin-left: 1000px;
					transform:rotate(-60deg);filter: blur(10px);transition: 0.5s;
					"
					:class="{'charaop1':charaop1==0,'charaop2':charaop1==1}"
					
					>
				</view>
			</view>

			<!---->
			<view style="display: flex;flex-direction: column;position: absolute;align-items: center;top:0;z-index: 6;margin-top:5vh"
			v-if="!showpixivbg"
			>
				<text style="font-size:80px;font-weight:700;color:#b5bdab;font-family:a
				">LAPLACE BEGONIA</text>
				<text style="font-size:25px;font-weight:700;color:#b5bdab;font-family:zhuzi">{{title1}}</text>
			</view>
			<view style="display: flex;flex-direction: column;position: absolute;align-items: center;;z-index: 6;height:100vh;width:100vw;margin-top:-20vh;flex-direction:column"
			v-if="showpixivbg"
			>
				<text style="font-size:80px;font-weight:700;color:white;font-family:a
				">LAPLACE BEGONIA</text>
				<view style="margin-top:20px;color:white;font-size:30px;font-weight:700;display:flex;
				height:200px;width:100vw;justify-content:center;
				"><text style="font-family:zhuzi">{{showlist1}}<text style="
				" class="re1">|</text></text></view>
			</view>
				

			  <view  style="width:100vw;position: absolute;top:0;display: flex;flex-direction: row;z-index: 5;color:#b5bdab
			  
			  ">
					
					<view style="margin-left: auto;width:30vw;display: flex;flex-direction: row;transition: 0.3s;"
					:class="{'showtool1':showtool1}"
					>
					<view @click.stop="showtool1=!showtool1"  style="margin-left:-2vw;display: flex;flex-direction: row;align-items: center;" >
						<text >设置</text>
					</view>
					<view style=";margin-left:auto;margin-left:2vw">
						<text @click.stop="changel2d()">切换人物</text>
					</view>
					<view style=";;margin-left:2vw">
						<text v-if="!showjuge()" @click.stop="changeshowjuge()">开启自动显示</text>
						<text v-if="showjuge()"  @click.stop="changeshowjuge()">关闭自动显示</text>
					</view>
					<view style=";;margin-left:2vw;">
						<text v-if="!showpixivbg" @click.stop="changepixivbg()">显示Pixiv封面</text>
						<text v-if="showpixivbg"  @click.stop="changepixivbg()">关闭Pixiv封面</text>
					</view>
					<view style=";;margin-left:2vw;margin-right:0vw">
						<text v-if="!showl2d" @click.stop="flush()">开启L2D显示</text>
						<text v-if="showl2d"  @click.stop="flush()">关闭L2D显示</text>
					</view>
					</view>
			  </view>
			  
			  <view style="position:absolute;bottom:10px;color:white;" v-if="showpixivbg">
			  <view  v-if="showback.pid!=null">{{showback.title}} | pid:{{showback.pid}} | 画师：{{showback.username}}</view>
			  </view>
		  </view>
		</view>
		</view>
		</u-transition>
		</view>
		<!--关于-->
		<u-popup :show="about1" @close="about1=false,show=false,backmiao2()" mode="center" round="15">
			<view style="width:50vw;height:90vh;;">
				<about style="margin-top: -50px;"></about>
			</view>
		</u-popup>
		<!--碎念-->
		<u-popup :show="about2" @close="about2=false,show=false,backmiao2()" mode="center" round="15">
			<view style="width:400px;height:300px;font-family: zhuzi;padding-left: 10px;padding-right: 10px;padding-bottom: 50px;">
				<view style="margin-top: 20px;width:100%;display: flex;justify-content: center;">
				<text style="text-align: center;font-size: 24px;font-weight: 700;">一点碎碎念</text>
				</view>
				<view style="margin-top:-20px;font-size: 20px;height:100%;display:flex;flex-direction:column;align-items: center;justify-content: center;">
					
					<text>
						<p style="font-size: 19px;margin-top: 0px;text-align: center;">想了好多还是就这样过去吧。</p>
						<p style="font-size: 19px;margin-top: 10px;text-align: center;">只是拉普拉斯花店一支普普通通的海棠。</p>
					</text>
					
				</view>
			</view>
			
		</u-popup>
		<u-transition style="position: absolute;
		right:20px;top:50%;margin-top:-25px;z-index:3px;" mode="fade" :show="!show">
		<view style="width:45px;height:45px;border-radius: 10px;background-color: white;display:flex;align-items: center;justify-content: center;
		">
			<uni-icons type="headphones" size="35" color="#8bc863"
			@click="show=!show,tools=!tools,backmiao1()"
			v-if="!musictype"
			></uni-icons>
			<view style="width:100%;height:90%;margin-bottom:10%;display: flex;align-items: flex-end;justify-content: center;" v-if="musictype"
			@click="show=!show,tools=!tools,backmiao1()"
			
			>
				<view style="width:8px;height:10px;border:5px 5px 0 0;background-color: #8bc863;"
				:style=""
				:class="{'m1':musictype==1,'m111':musictype!=1}"
				>
					
				</view>
				<view style="width:8px;height:30px;margin-left:3px;margin-right:3px;border:5px 5px 0 0;background-color: #8bc863;"
				:class="{'m3':musictype==1,'m333':musictype!=1}"
				>
					
				</view>
				<view style="width:8px;height:20px;border:5px 5px 0 0;background-color: #8bc863;"
				:class="{'m2':musictype==1,'m222':musictype!=1}"
				>
					
				</view>
			</view>
		</view>
		</u-transition>
		<!--音乐-->
		<u-popup :show="tools" @close="tools=false,show=false,charaop=0,backmiao2()" mode="center" round="20">
			<view style="width:80vw;height:80vh;background-color: white;border-radius: 15px;
			display:flex;flex-direction:row
			" v-if="musiclist.length>0" @mouseenter="mouseenter" @mouseleave="mouseleave">
				<view style="position:relative;width:25%;height:100%;border-radius: 15px;overflow:hidden;display: flex; flex-direction: column;align-items: center;z-index:2;background-color: white;">
					<view style="position: absolute;width:100%;height:100%;z-index: 1;filter:blur(8px);border-radius: 15px;"
					 v-bind:style="{backgroundImage:'url(' + musiclist[musicid].bg + ')'}"
					v-if="musicid>=0"
					>
						
					</view>
					<view style="background-;;position: absolute;z-index: 2;width:100%;height:100%;display: flex; flex-direction: column;align-items: center;">
					<view 
					 v-bind:style="{backgroundImage:'url(' + musichead + ')'}"
					style="background-position:center;background-size: cover;;background-repeat: no-repeat;height:300px;width:300px;border-radius:20px;margin-top:10%" :src="musichead" mode="aspectFill">
			
					</view>
					<text style="font-family: zhuzi
					;;margin-top:100px;color:white;font-size:40px">{{musiclist[musicid].musicname}}</text>
					<text style="font-family: zhuzi;margin-top:20px;color:white;font-size:30px">{{musiclist[musicid].name}}</text>
					<view style="display: flex;align-items: center;justify-content: center;flex-direction: column;margin-top:60px">
					<view style="font-family: zhuzi;margin-top:10px;color:white;font-size:25px"
					v-for="(item1,index1) in musiclist[musicid].notes">{{item1}}</view>
					</view>

					</view>
				</view>
				<view style="width:80%;height:100%;position: relative;border-radius:0 15px 15px 0;overflow:hidden;margin-left:-2%;z-index:1"
				
				>
					<view style="width:100%;height:100%;position:absolute;z-index:1">
						<image style="width:100%;height:100%;" mode="aspectFill"
						:src="musiclist[musicid].bg"
						>
							
						</image>
					</view>
					<view style="width:100%;height:100%;position:absolute;z-index:2;display: flex;align-items: center;justify-content: center;">
						<view 
						:class="{'beiguang1':charaop==1,'beiguang2':charaop==0}" 
						style=";bottom:0;filter:blur(100px);position:absolute;z-index:-1;transition: 1s;"
						 >
							
						</view>
						<img style="height:100%;width:100%;margin-bottom:0;width:100%;object-fit: contain;
						
						" 
						:src="musiclist[musicid].charaimage" 
						>
							
						</img>
						<img style="height:100%;width:100%;margin-bottom:0;width:100%;object-fit: contain;;position: absolute;z-index: 2;transition: 0.5s;" :class="{'charaop1':charaop==1,'charaop2':charaop==0}"   src="https://txtzz-1301452902.file.myqcloud.com/milky-green-v1.webp">
							
						</img>
					</view>
					<view style="width:100%;height:100%;top:0;position:absolute;z-index:3;
					display: flex;align-items: center;flex-direction:column;font-size:22px;transition: 0.5s;
					" :class="{'heise1':charaop==0,'heise2':charaop==1}">
						<view style="width:75%;height:70%;border-radius: 15px;background-color: rgba(0,0,0,0.4);padding:5%;padding-top:2%;;margin-top:5%;overflow-y: auto;margin-right:-3%;">
							<view v-for="(item,index) in musiclist" style="height:50px;width:100%;color:white;display:flex;flex-direction: row;margin-bottom:50px">
								<view style="width:30px;">
									<text>{{(index+1)+'.'}}</text>
								</view>
								<view style="width:500px;display:flex;flex-direction:column">
								<view>
								<text>{{item.musicname}}</text>
								<text v-if="item.qiepian==1">(切片)</text>
								</view>
								<text style="margin-top:10px">{{'原唱:'+item.yuanchang+'  翻唱:'+item.name}}</text>
								</view>
								<view style="margin-left:auto;margin-right:-50px">
									<text>
										{{item.length}}
									</text>
									<view style="width:75px;height:30px;display:flex;flex-direction:row;align-items: flex-end;margin-top:10px"
									v-if="index==musicid" @click="musicjuge()"
									>
										<view style="width:8px;height:10px;border:5px 5px 0 0;background-color: white;" 
										:style=""
										:class="{'m1':musictype==1,'m111':musictype!=1}"
										>
											
										</view>
										<view style="width:8px;height:30px;margin-left:3px;margin-right:3px;border:5px 5px 0 0;background-color: white;"
										:class="{'m3':musictype==1,'m333':musictype!=1}"
										>
											
										</view>
										<view style="width:8px;height:20px;border:5px 5px 0 0;background-color: white;"
										:class="{'m2':musictype==1,'m222':musictype!=1}"
										>
											
										</view>
									</view>
									<view style="width:75px;height:30px;display:flex;flex-direction:row;align-items: flex-end;margin-top:10px"
									v-if="musicid!=index" @click="musicid=index,musictype=1, changemusic(index)"
									>
										<u-icon size="30" color="white" name="play-right-fill"></u-icon>
									</view>
								</view>
							</view>
						</view>
						<view style="width:100%;height:18%;background-color: rgba(0,0,0,0.4);margin-top:8%;display:flex;flex-direction:column;;
						">
							<view  v-if="musicid!=-1" style="width:100%;display: flex;flex-direction: row;align-items: center;justify-content: center;color:white;font-size:18px;margin-top:15px"
							
							>
								<text>{{musictime() +' -- '+ musiclist[musicid].length}}</text>
							</view>
							<view  v-if="musicid==-1" style="width:100%;display: flex;flex-direction: row;align-items: center;justify-content: center;color:white;font-size:18px;opacity: 0;"
							
							>
								<text>1</text>
							</view>
							<view style="width:90%;margin-left:6.5%;margin-top:-10px;display: flex;flex-direction: row;align-items: center;">
								<slider style="width:100%;" :value="now_time/total_time*100" block-size="10" block-color="white" activeColor="white" @change="sliderChange"></slider>

							</view>
							<view style="width:100%;display: flex;flex-direction: row;align-items: center;justify-content: center;margin-top:-5px">
								<view  style=""  v-if="playtype==2" @click="playtype=1">
									<u-icon  name="list" size="30" color="white"></u-icon>
								</view>
								<view  style=""  v-if="playtype==1" @click="playtype=2">
									<u-icon name="reload"   size="30" color="white" ></u-icon>
								</view>
								<u-icon name="rewind-left" color="white" size="32" @click="last()" style="margin-left:15px"></u-icon>
								<u-icon v-if="!musictype" name="play-circle" color="white" size="32" style="margin-left:15px;margin-right:15px"
								@click="playmusic()"
								></u-icon>
								<u-icon v-if="musictype" name="pause-circle" color="white" size="32" style="margin-left:15px;margin-right:15px"
								@click="pausemusic()"
								></u-icon>
								<u-icon name="rewind-right" color="white" size="32" @click="next()" style="margin-right:15px"></u-icon>
								<view  style="width:30px;position: relative;display: flex;flex-direction: row;align-items: center;">
									<u-icon   name="volume" size="30" color="white" @click="audionum=!audionum"></u-icon>
									<u-transition mode="fade" style="width:150px;position: absolute;margin-left:30px;top:0;margin-top:-3px;height:1px" duration="150" :show="!audionum">
									<slider  :value="musicaudio" block-size="10" block-color="white" activeColor="white" @change="musicaudiochange"></slider>
									</u-transition>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</u-popup>
	</view>
</template>

<script>
	import titles from './title.vue'
	import XsuuSwiper from "../../components/Xss-swiper/Xsuu-swiper.vue"
	import { L2Dwidget } from 'live2d-widget'
	import  un  from "@/universe.js"
	import about from '@/pages/index/about.vue'
	import WzsAudio from '@/components/WZS-Audio/WZS-Audio.vue'
	const innerAudioContext = uni.createInnerAudioContext();
	export default {
		
		components:{
			titles,XsuuSwiper,about,WzsAudio
		},
		data() {
			return {
				notes:[],
				musicaudio:100,
				playtype:2,
				total_time:300,
				now_time:0,
				timeupdata:'',
				interval:'',
				musicid:0,
				percent:0,
				musictype:false,
				musiclist:[],
				about1:false,
				about2:false,
				showl2d:true,
				transition1:false,
				transition2:false,
				transition3:false,
				transition4:true,
				show:false,
				showimg:[],
				showpixivbg:true,
				showback:[],
				replymessagenum:0,
				showlist:[
					"我并不羡慕别人的人生，这就是所谓幸福。——《幸运星》",
					"“如果下雨了，你愿意留下吗？” \n“即使不下雨，我也在这里啊。”\n——《言叶之庭》",
					"只要记住你的名字，不管你在世界的哪个地方，我一定会，去见你。——《你的名字》",
					"曾经发生过的事情不可能忘记，只不过是想不起而已。——《千与千寻》",
					"有思念你的人在的地方,就是你的归处。——《火影忍者》",
					"真正重要的东西，总是没有的人比拥有的人清楚。——《银魂》",
					"真正重要的东西，永远都是非常简单的。——《Clannad》",
					"就算被打得遍体鳞伤，内心也不会简单屈服。——《妖精的尾巴》",
				],
				show1:false,
				showlist1:"",
				showlist1sub:0,
				showsub:0,
				showtool1:true,
				imagewidth:0,
				jugeid:0,
				timer:null,
				  swiperItems:[
				                 
				                ],
				jugetops:[],
				op:0.95,
				open:0,
				scrollid:"",
				musichead:"",
				key:"",
				replymessageshow:false,
				status:"loadmore",
				shadowStyle:{
					  // #ifndef APP-NVUE
					    backgroundImage: "linear-gradient(-180deg, rgba(255, 255, 255, 0) 0%, #fff 80%)",
					    // #endif
					    // #ifdef APP-NVUE
					    // nvue上不支持设置复杂的backgroundImage属性
					    backgroundImage: "linear-gradient(to top, #fff, rgba(255, 255, 255, 0.5))",
					    // #endif
					    paddingTop: "100px",
					    marginTop: "-100px",
				},
				title: 'Hello',
				userbackgroundimage:"",
				icon:"/static/icon.png",
				src:"/static/aa.jpg",
				scroll_top:-1,
				audionum:100,
				scroll_top1:0,
				music_play:true,
				zindex:10,
				scroll_top2:1,
				username:"",
				select_fenlei:0,
				
				fenlei:"全部分类",
				activity:[],
				type:0,
				item:"全部分类",
				a111:false,
				index:0,
				limit:0,
				getmore:true,
				admin:0,
				siteinfo:{},
				charaop1:0,
				sub2:0,
				backmiao:0,
				mylike:[
					
				],
				tuijian:[

				],
				tuijianlist:[
					
				],
				themelist:[
				],
				toplist:[

				],
				leftlist:[

				],
				nowDate:"",
				rundata:"",
				title1:"",
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
				message:[],
                shapetype:"",
				shapecolor:"",
				tools:false,
				innerAudioContext:"",
				charaop:0,
				charaop2:0,
				backweizhi:'translate3d(0px, 0px, 0px) scale(1.05)',
				backweizhi1:'translate3d(0px, 0px, 0px)',
				backweizhi2:'translate3d(0px, 0px, 0px)'
			}
		},
		created() {

		},
		
		onLoad() {
			let templist1=['奶绿只是想偷睡一会','奶绿只是想偷个懒罢了','奶绿只是想挣个钱罢了','奶绿能有什么坏心眼呢'];
			this.title1=templist1[uni.$u.random(0, 3)];
			uni.request({
				url:getApp().globalData.http+"/api/updatenotes",
				method:"POST",
				data:{},
				success: (res) => {
					this.notes=res.data.data;
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/fangke",
				method:"POST",
				data:{},
				success: (res) => {
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/musiclist",
				method:"POST",
				data:{},
				success: (res) => {
					this.musiclist=res.data.musiclist;
					innerAudioContext.src=this.musiclist[0].src;
					for(var i=0;i<this.musiclist.length;i++){
						
						this.musiclist[i].notes=this.musiclist[i].notes.split(";");
					}
					this.musichead=this.musiclist[this.musicid].head;
				}
			})
			this.playAudio();
			
			//innerAudioContext.autoplay = true;
			//innerAudioContext.src = 'https://txtzz-1301452902.file.myqcloud.com/最长的电影-明前奶绿.mp3';

			var arr=["circle","edge","triangle", "polygon","star"];
			var sub = uni.$u.random(0, 4);
			this.shapetype=arr[sub];
			var rgb="rgb("+uni.$u.random(0, 255)+","+uni.$u.random(170, 255)+","+uni.$u.random(0, 255)+")";
			this.shapecolor=uni.$u.rgbToHex(rgb);

			uni.request({
				url:getApp().globalData.http+"/api/getStatistics",
				method:"POST",
				data:{},
				success: (res) => {
					this.siteinfo=res.data.today;
					this.siteinfo.todayfangke=res.data.todayfangke;
					//console.log(this.siteinfo);
				}
			})
			//this.formatDate();
			let a=this;
			//this.dark();
			this.currentTime();
			this.showl2d=getApp().globalData.showl2d;
			this.l2d=getApp().globalData.l2d;
			this.showpixivbg=getApp().globalData.showpixivbg;
			this.imagewidth = uni.getSystemInfoSync().windowWidth*0.45*0.8*0.8*0.33;
			let that = this;
			if(this.showl2d){
			//	this.l2dinit();
			}
			if(!getApp().globalData.showjuge){
				this.show=false;
				this.zindex=-1;
				this.backmiao=0.3;
			}
			else{ this.show=true;this.backmiao=0;}
			//console.log(this.show);
			this.showsub=uni.$u.random(0, 7);
			this.tests();
			uni.request({
				url:getApp().globalData.http+"/api/getmessage",
				data:{
				},
				method:"POST",
				success: (res) => {
					var temp=[];
					
					for(var i=0;i<getApp().globalData.firstopen.length;i++)
						temp.push(getApp().globalData.firstopen[i]);
					for(var i=0;i<res.data.length;i++){
						if(temp.indexOf(res.data[i].message)==-1){
							var str=res.data[i].message.toString();
							this.message.push(str);
							
						}
					}
					for(var i=0;i<this.message.length;i++)
						getApp().globalData.firstopen.push(this.message[i]);
					uni.setStorage({
					    key: "firstopen",
					    data: getApp().globalData.firstopen,
					    success: function () {
					    }
					});
					
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getthemelist",
				data:{
					type:this.type
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
							res.data.data[i].image[j]+="";
						}
						res.data.data[i].text=this.test(res.data.data[i].text);
					}
					if(res.data.activity.length>0){
						this.activity=[];
										
						for(var i=0;i<res.data.activity.length;i++){
						var temp={
							"img":res.data.activity[i].image.split(";")[0]+'?imageMogr2/thumbnail/1000000@',
							"url":"./theme?themeid="+res.data.activity[i].themeid,
							"title":res.data.activity[i].title,
							"Subtitle":res.data.activity[i].data
						}
						this.activity.push(temp);
						}
					}
					else{
						this.activity=[];
					}
					this.themelist=res.data.data;
					this.jugeid=this.themelist[0].themeid;
					this.limit++;
					if(!this.show)
					this.jugetop();
					this.transition2=true;
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/getthemelist_jing",
				data:{
					
				},
				method:"POST",
				success: (res) => {
					for(var i=0;i<res.data.data.length;i++){
						var src=res.data.data[i].image.toString();
						res.data.data[i].image=src.split(";");
						res.data.data[i].image=res.data.data[i].image.slice(0,-1);
						if(res.data.data[i].tags!=undefined){
						res.data.data[i].tags=res.data.data[i].tags.split(";");
						res.data.data[i].tags.pop();
						}
						res.data.data[i].text=this.test(res.data.data[i].text);
					}
					this.tuijian=res.data.data;
					this.suijituijian();
					////console.log(this.themelist);
				}
			})
			if(getApp().globalData.showjuge)
			this.timer = setInterval( () => {
			    this.showlist2();		
			}, 300)
		},
		onShow() {
			
			this.replymessageshow=getApp().globalData.replymessageshow;
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
			if(getApp().globalData.cookie!=''){
				uni.request({
					url:getApp().globalData.http+"/api/getmylike",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid
					},
					success: (res) => {
						if(res.data.code==1001){
							getApp().globalData.mylike=res.data.mylike
							this.mylike=res.data.mylike;
							
						}
					}
				})
				if(getApp().globalData.cookie!=''){
					uni.request({
						url:getApp().globalData.http+"/api/getmyself",
						method:"POST",
						data:{
							"cookie":getApp().globalData.cookie
						},
						success: (res) => {
							getApp().globalData.admin=res.data.data.admin;
							getApp().globalData.ban=res.data.data.ban;	
							this.admin=getApp().globalData.admin;
							uni.setStorage({
							    key: "admin",
							    data: getApp().globalData.admin,
							    success: function () {
							    }
							});
							uni.setStorage({
							    key: "ban",
							    data: getApp().globalData.ban,
							    success: function () {
							    }
							});
							uni.getStorage({
							    key:"admin",
							    success: function(res){
							        getApp().globalData.admin=res.data;
									//console.log("admin:"+getApp().globalData.admin);
								}
							 });
						}
					})
				}
			}
			uni.request({
				url:getApp().globalData.http+"/api/getfenlei",
				method:"POST",
				data:{
				},
				success: (res) => {
					//console.log(res);
					this.leftlist=[];
					for(var i=0;i<res.data[0].length;i++){
						var temp={
							"title":res.data[0][i],
							"num":res.data[1][i]
						}
						//console.log(temp);
						this.leftlist.push(temp);
					}
					var temp=this.leftlist[this.leftlist.length-1];
					this.leftlist.pop();
					this.leftlist.unshift(temp);
					this.transition1=true;
				}
			})
			this.scroll_top1=getApp().globalData.scroll_top1;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			if(this.userbackgroundimage==""||this.userbackgroundimage==null||this.userbackgroundimage==undefined){
			getApp().globalData.userbackgroundimage='https://txtzz-1301452902.file.myqcloud.com/bg.webp';
			this.userbackgroundimage='https://txtzz-1301452902.file.myqcloud.com/bg.webp'
			}
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.op=getApp().globalData.op;
			uni.request({
				url:getApp().globalData.http+"/api/getthemelist_top",
				data:{
					
				},
				method:"POST",
				success: (res) => {
					//console.log(res.data);
					for(var i=0;i<res.data.data.length;i++){
						var src=res.data.data[i].image.toString();
						res.data.data[i].image=src.split(";");
						res.data.data[i].image=res.data.data[i].image.slice(0,-1);
						if(res.data.data[i].tags!=undefined){
						res.data.data[i].tags=res.data.data[i].tags.split(";");
						res.data.data[i].tags.pop();
						}
						res.data.data[i].text=this.test(res.data.data[i].text);
					}
					this.toplist=res.data.data;
					////console.log(this.themelist);
				}
			})
		},
		computed:{

		},
		methods: {
			backmiao1(){
				this.backmiao=0.3;
				setTimeout(()=>{
					this.backmiao=0;	
				},300);
			},
			backmiao2(){
				this.backmiao=0.3;
			},
			test1(e){
				if(this.show)
				uni.$u.throttle(this.test2(e), 1)
				else
				this.backweizhi="translate3d(0px , 0px , 0px) scale(1.05)";
			},
			test2(e){
				var x=(uni.getSystemInfoSync().windowWidth/2-e.clientX)/uni.getSystemInfoSync().windowWidth*2;
				var y= (uni.getSystemInfoSync().windowHeight/2-e.clientY)/uni.getSystemInfoSync().windowHeight*2;
				this.backweizhi="translate3d("+x*-20+"px , "+y*10+"px , 0px) scale(1.05)";
				this.backweizhi1="translate3d("+x*80+"px , "+y*80+"px , 0px)";
				this.backweizhi2="translate3d("+x*30+"px , "+y*30+"px , 0px)"
			},
			mouseleave(i){
				this.charaop=0;
			},
			mouseenter(i){
				this.charaop=1;
			},
			mouseenter1(i){
				this.charaop1=1;
				setTimeout(()=>{
					this.charaop2=1;
				},300);
			},
			mouseleave1(i){
				this.charaop1=0;
				setTimeout(()=>{
					this.charaop2=0;
				},300);
			},
			musicaudiochange(i){
				//console.log(i);
				if(i.detail.value<=10)i.detail.value=0;
				this.musicaudio=i.detail.value;
				//console.log(i/100);
				innerAudioContext.volume=(i.detail.value)/100;
			},
			tonailv(){
				window.location.href="https://space.bilibili.com/2132180406"
			},
			next(){
				this.musicid=this.musicid+1;
				if(this.musicid==this.musiclist.length)this.musicid=0;
				this.changemusic(this.musicid);
			},
			last(){
				this.musicid=this.musicid-1;
				if(this.musicid<0)this.musicid=this.musiclist.length-1;
				this.changemusic(this.musicid);
			},
			musictime(){
				var min=parseInt(this.now_time/60);
				var sec=parseInt(this.now_time%60);
				if(sec<10) 
					return min+":0"+sec;
				else return min+":"+sec;
			},
			changemusic(i){
				console.log(i);
				if(i==this.musiclist.length){
					i=0;
				}
				else if(i<0){
					i=0;
				}
				this.musicid=i;
				this.musictype=false;
				this.now_time=0;
				this.total_time=this.musiclist[this.musicid].time;
				innerAudioContext.src = this.musiclist[this.musicid].src;
				this.musichead=this.musiclist[this.musicid].head;
				this.$forceUpdate(this.musichead);
				console.log(this.musichead);
				innerAudioContext.stop();
				innerAudioContext.play();
				this.musictype=true;
			},
			musicjuge(){
				this.musictype=!this.musictype;
				if(this.musictype){
					this.playmusic();
				}else{
					this.pausemusic();
				}
			},
			pausemusic(){
				this.musictype=false;
				innerAudioContext.pause();
			},
			playmusic(){
				this.musichead=this.musiclist[this.musicid].head;
				//this.$set(this.musichead,this.musiclist[this.musicid].head);
				//console.log(this.musichead);
				this.musictype=1;
				innerAudioContext.autoplay = true;
				innerAudioContext.play(() => {
				});
			},
			playAudio(){
				this.now_time=0;
				let that=this;
				this.timeupdata = setInterval(()=>{
					//console.log(this.now_time/this.total_time*100)
					if(this.musictype){
						this.now_time ++
					
						if(this.now_time >= this.total_time){
							this.musictype = false
							this.now_time = 0
							if(this.playtype==1){
								this.changemusic(this.musicid);
							}else{
								this.changemusic(this.musicid+1);
							}
						}
					}
				},1000)
			},
			
			// 拖动进度条
			sliderChange(e){
				let second = e.detail.value / 100 * this.total_time
				this.now_time = this.total_time/100 *  e.detail.value;
				innerAudioContext.seek(this.now_time);
			},
			linkto(i){
				//console.log(i);
				window.location.href="https://"+i;
			},
			l2dinit(){
				let that=this;
				if(this.showl2d){
				var url=''+that.l2dlist[that.l2d];
	
				L2Dwidget.init({
				  display: { position: 'left' },	
				  model: {
				    jsonPath:url
				  },
				  dialog:{
				    enable: true,
				            script: {
				  			'hover .star': '星星在天上而你在我心里 (*/ω＼*)',
				  			//触摸到身体
				  			'tap body': '害羞⁄(⁄ ⁄•⁄ω⁄•⁄ ⁄)⁄',
				  			//触摸到头部
				  			'tap face': '~~'
				            }
				    },
				
				});}
			},
			currentTime() {
			     setInterval(this.formatDate, 1000);
			},
			formatDate() {
			     let date = new Date();
			     let year = date.getFullYear(); // 年
			     let month = date.getMonth() + 1; // 月
			     let day = date.getDate(); // 日
			     let week = date.getDay(); // 星期
			     let weekArr = [ "星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六" ];
			     let hour = date.getHours(); // 时
			     hour = hour < 10 ? "0" + hour : hour; // 如果只有一位，则前面补零
			     let minute = date.getMinutes(); // 分
			     minute = minute < 10 ? "0" + minute : minute; // 如果只有一位，则前面补零
			     let second = date.getSeconds(); // 秒
			     second = second < 10 ? "0" + second : second; // 如果只有一位，则前面补零
			     this.nowDate = `${year}年${month}月${day}日 ${hour}:${minute}:${second} `+weekArr[week];
				 this.rundata="";
				 let startime=1648080000;
				 let g = new Date().getTime()/1000;
				 var temp=(g-startime)
				 let nowday=0;
				 if(temp>60*60*24){
					nowday=parseInt(temp/(60*60*24));
					temp=temp%(60*60*24);
				 }
				 let nowhour=0;
				 if(temp>60*60){
					nowhour=parseInt(temp/(60*60));
					temp=temp%(60*60);
				 }
				 let nowmin=0;
				 if(temp>60){
					nowmin=parseInt(temp/60);
					temp=temp%60;
				 }
				 let nows=0;
				 nows=parseInt(temp%60);
				 if(nowday>0)this.rundata+=nowday+"天";
				 if(nowhour>0)this.rundata+=nowhour+"小时";
				 if(nowmin>0)this.rundata+=nowmin+"分";
				 this.rundata+=nows+"秒";
			},
			changepixivbg(){
				this.showpixivbg=!this.showpixivbg;
				if(!this.showpixivbg){
					this.showback.url1=this.showback.url;
					this.showback.url="";
				}else{
					this.showback.url=this.showback.url1;
				}
			
				getApp().globalData.showpixivbg=this.showpixivbg;
				uni.setStorage({
				    key: "showpixivbg",
				    data: getApp().globalData.showpixivbg,
				    success: function () {
				    }
				});
			},
			changel2d(){
				this.l2d++;
				if(this.l2d==getApp().globalData.l2dlist.length)this.l2d=0;
				getApp().globalData.l2d=this.l2d;
				uni.setStorage({
				    key: "l2d",
				    data: getApp().globalData.l2d,
				    success: function () {
				    }
				});
				this.$router.go(0)
			},
			weizhi(e){
				var latitude=e.lal.split(";")[0];
				var longitude=e.lal.split(";")[1];
				uni.openLocation({
					latitude: parseFloat(latitude),
					longitude: parseFloat(longitude),
				});
			},
			flush(){
				getApp().globalData.showl2d=!getApp().globalData.showl2d;
				uni.setStorage({
				    key: "showl2d",
				    data: getApp().globalData.showl2d,
				    success: function () {
				    }
				});
				this.$router.go(0);
			},
			tests(){
				uni.request({
					url:getApp().globalData.http+"/api/getpixiv",
					method:"POST",
					data:{
						"type":1
					},
					success: (res) => {
						this.showback=res.data[0];
						if(!this.showpixivbg){
							this.showback.url1=this.showback.url;
							this.showback.url="";
						}else{
							
						}
						//console.log(this.showback);
					}
				})
			},
			showopen(){
		
				this.show=true;
				this.showsub=uni.$u.random(0, 7);
				this.showlist1sub=0;
				this.zindex=10;
				this.timer = setInterval( () => {
				    this.showlist2()
					this.backmiao=0;
				}, 300)
				
			},
			juge1(){
				let that=this;
				if(this.activity.length>0&&this.tuijianlist.length>0){
					setTimeout(()=>{
						that.a111=true;
						return true;
					},80)
				
				}else return false;
			},
			showclose(){
				this.backweizhi="translate3d(0px , 0px , 0px) scale(1.05)";
			    this.show=false,
				this.backmiao=0.3;
				clearTimeout(this.timer);
				this.timer = null;  
				setTimeout(() => {
					clearTimeout(this.timer);
					this.timer = null;  
					this.showlist1="";
					this.zindex=-1;
				}, 500)
				this.sub2=0;
				this.jugetop();
			},
			showjuge(){
				return getApp().globalData.showjuge;
			},
			changeshowjuge(){
				getApp().globalData.showjuge=!getApp().globalData.showjuge;
				uni.setStorage({
				    key: "showjuge",
				    data: getApp().globalData.showjuge,
				    success: function () {
				    }
				});
			},
		    showlist2(){
				if(this.showlist1sub>=this.showlist[this.showsub].length){
					clearTimeout(this.timer);  
					this.timer = null;  
					setTimeout(() => {   
						this.timer = setInterval( () => {
						     this.showlist3()
						}, 150)
					}, 2500)
				}
				else{
					setTimeout(() => {
						this.showlist1+=this.showlist[this.showsub][this.showlist1sub];
						this.showlist1sub++;
					}, Math.round(Math.random()*100)+150)
				}
			},
			showlist3(){
				if(this.showlist1sub<0){
					this.showlist1="";
					var showsub1=uni.$u.random(0, this.showlist.length-1);
					while(showsub1==this.showsub){
						showsub1=uni.$u.random(0, this.showlist.length-1);
					}
					this.showsub=showsub1;
					this.showlist1sub=0;
					clearTimeout(this.timer);  
					this.timer = null;  
					this.timer = setInterval( () => {
					    this.showlist2()
					}, 300)
				}
				else{
					this.showlist1=this.showlist1.substring(0,this.showlist1sub);
					this.showlist1sub--;
				}
			},
			close1(){
				this.showtime=300;
				this.show=false;
			},
			beian:function(){
				window.location.href="https://beian.miit.gov.cn/"
				
			},
			con1(){
				console.log(1);
			},
			firstopen(){
				if(this.message.length>0)return true;return false;
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
				uni.reLaunch({
					url:"index"
				})
			},
			jugetop:function(){
				let that=this;
				setTimeout(() => {  
				for(var i=0;i<that.themelist.length;i++){
					let info = uni.createSelectorQuery().select("#t"+i);
					　　　  　info.boundingClientRect(function(data) { //data - 各种参数
						
					　　　  　if(data.height>50)
								that.jugetops[i]=true;  // 获取元素宽度
					}).exec()
				}that.$set(that.jugetops,that.jugetops);
				} , 1)
			
			},

			linktag:function(e){
				uni.navigateTo({
					url:"tag?tagname="+e
				})
			},
			backtop:function(){
				this.scroll_top=Math.random()-1;
				this.jugeid=this.themelist[0].themeid;
				this.sub2=0;
			},

			getthemelistby_fenlei:function(item,index){
				if(item!=undefined)this.item=item.title;
				if(index!=undefined)this.index=index;
				if(this.getmore==false)return ;
				this.getmore=false;
				if(getApp().globalData.cookie==''&&this.type==2){
					this.type=0;
				this.$refs.uToast.show({
					...this.upload[3],
					complete() {
						
					}
				})
				}
				this.status="loading";
				{
					uni.request({
						url:getApp().globalData.http+"/api/getthemelistby_fenlei",
						method:"POST",
						data:{
							"fenlei":this.item,
							"type":this.type,
							"limit":this.limit,
							"userid":getApp().globalData.userid,
							"cookie":getApp().globalData.cookie
						},
						success: (res) => {
							this.limit++;
							this.jugetops=[];
							for(var i=0;i<res.data.data.length;i++){
								var src=res.data.data[i].image.toString();
								res.data.data[i].image=src.split(";");
								res.data.data[i].image=res.data.data[i].image.slice(0,-1);
								if(res.data.data[i].tags!=undefined){
								res.data.data[i].tags=res.data.data[i].tags.split(";");
								res.data.data[i].tags.pop();
								}
								res.data.data[i].text=this.test(res.data.data[i].text);
								this.themelist.push(res.data.data[i]);
								this.jugetops.push(false);
							}
							this.jugeid=this.themelist[0].themeid;
							this.getmore=true;
							this.jugetop();
							setTimeout(() => {
								if(!this.show){
									console.log("x");
									this.jugetop();
								}
							}, 10)
							if(res.data.data.length<10){
								this.getmore=false;
								this.limit=0;
								this.status="nomore";
								return;
							}
							
						}
					})
				}
			},
			jugelike:function(res){
				//console.log(this.mylike.length);
				for(var i=0;i<this.mylike.length;i++)
					if(this.mylike[i]==res)return true;return false
			},
			suijituijian:function(){
				var temp=[];
				this.tuijianlist=[];
				if(this.tuijian.length==0)return;
				while(1){

					var i=uni.$u.random(0,this.tuijian.length-1);
					if(temp.indexOf(i)==-1){
						temp.push(i);
					}
					if(temp.length==3||this.tuijian.length==temp.length)
					break;
				}
				for(var i=0;i<temp.length;i++)
				this.tuijianlist.push(this.tuijian[temp[i]]);
				//console.log(this.tuijianlist);
				this.transition3=true;
			},
			scroll:function(e){
				let that=this;
				var sub=1;
				var i=this.sub2-3;
				if(i<0)i=0;
				for(i;i<this.themelist.length;i++){
					if(sub==0)break;
					uni.getSystemInfo({
					　　success: function(res) { // res - 各种参数
							let info = uni.createSelectorQuery().select("#id"+that.themelist[i].themeid);
					　　　  　info.boundingClientRect(function(data) { //data - 各种参数
								//console.log(data.top);
								if(data.top<=330&&data.top>=130){
									that.jugeid=that.themelist[i].themeid;
									that.sub2=i;
									sub=0;
								}	
									
					　　    }).exec()
						}
					});
				}
				if(e.detail.scrollTop>20){
					this.open=1;
					setTimeout(() => {this.scroll_top2=0;this.scroll_top1=1, 20})
					
					
				}
				else {this.scroll_top1=0;this.scroll_top2=1}
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


			test:function(e){
				
				var imgReg = /<img.*?(?:>|\/>)/gi;//定义正则表达式（拿到img标签所有值）
				      
				var deArray  = e.match(imgReg);//使用正则表达式，拿到的是数组
				//["<img src="images/01.gif">", "<img src="images/02.gif">"]
				if (deArray != null && deArray != undefined) {
				        for (var i=0;i<deArray.length;i++) {
				            e = e.replace(deArray[i], "") //放在循环中剔除img标签
				        }
				    }
				var re1 = new RegExp("<br>","g");//匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
				//e = e.replace(re1,'');//执行替换成空字符
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
			newtheme:function(){
				if(getApp().globalData.cookie==''){
					uni.showToast({
						title:"请登录",
						icon:"error"
					})
					return;
				}
				uni.navigateTo({
					url:"newtheme"
				})
			},
			
		}
	}
</script>

<style>
	body{
		height:100vh;
		width:100vw;
		overflow: hidden;
	}
	.m1{
		animation:m11 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear
	}
	.m2{
		animation:m22 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear
	}
	.m3{
		animation:m33 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear
	}
	.m111{
		animation:m11 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear;
		animation-play-state:paused;
	}
	.m222{
		animation:m22 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear;
		animation-play-state:paused;
	}
	.m333{
		animation:m33 1s;
		animation-iteration-count:infinite;
		/*animation-direction:alternate-reverse;*/
		animation-timing-function:linear;
		animation-play-state:paused;
	}
	@keyframes m11
	{
	    0%   {height:10px}
	    16.6%  {height:20px}
	    33.2%  {height:30px}
	    50%  {height:20px}
		66.6%  {height:10px}
	    83.2% {height:0px}
		100% {height:10px}
	}
	@keyframes m22
	{
	    0%   {height:20px}
	    16.6%  {height:30px}
	    33.2%  {height:20px}
	    50%  {height:10px}
		66.6%  {height:0px}
	    83.2% {height:10px}
		100% {height:20px}
	}
	@keyframes m33
	{
	    0%   {height:30px}
	    16.6%  {height:20px}
	    33.2%  {height:10px}
	    50%  {height:0px}
		66.6%  {height:10px}
	    83.2% {height:20px}
		100% {height:30px}
	}
	.showtool1{
		transform: translateX(30vw);
	}
	.text-blue :hover{
		color:#8bc863
	}
	.text1 :hover{
		font-weight: 700;
	}
	.op{
		opacity: 0.95;
	}
	.op1{
		background:rgba(255, 255, 255, 0);
	}
	.text_select {
		color:#8bc863;
		font-weight: 800;
	}
	.re1{
		animation: myfirst 1s linear  normal infinite;
	}
	@keyframes myfirst
	{
		0%{
			opacity: 0;
		}
		50%{
			opacity: 1;
		}
		100%{
			opacity: 0;
		}
	}
.class1{

}

.class2{
	
}
.showtool11{
	transfrom:rotate(180deg)
}

a :visited {
    color: white
}

 @keyframes rotate {
        0%{
            transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(0);
        }
        100%{
            transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(-360deg);
        }
    }
    .stars{
        transform: perspective(500px);
        transform-style: preserve-3d;
        position: absolute;
        perspective-origin: 50% 100%;
        left:50%;
        animation: rotate 90s infinite linear ;
        bottom: 0;
    }
    .star{
        width: 2px;
        height: 2px;
        background: #f7f7b8;
        position: absolute;
        top: 0;
        left: 0;
        backface-visibility: hidden;
    }
	.charaop1{
		opacity: 0;
	}
	.charaop2{
		opacity: 1;
	}
	.beiguang1{
		width:30px;
		border-top: 0px solid transparent;  
		border-left: 350px solid transparent;          
		border-right: 350px solid transparent;      
		border-bottom: 100vh solid  rgba(255, 254, 205, 0.8);
		
	}
	.beiguang2{
		width:0px;
		border-top: 0px solid transparent;  
		border-left: 0px solid transparent;          
		border-right: 0px solid transparent;      
		border-bottom: 0vh solid  rgba(255, 254, 205, 0.8);
	}
	.beiguang3{
		width:30px;
		border-top: 0px solid transparent;  
		border-left: 350px solid transparent;          
		border-right: 350px solid transparent;      
		border-bottom: 100vh solid  rgba(255, 254, 205, 0.8);
		
	}
	.heise1{
		background-color: rgba(0,0,0,0.3);
	}
	.heise2{
		background-color: rgba(0,0,0,0);
	}
	.heise4{
		background-color: rgba(0,0,0,0.25);
	}
	.heise3{
		background-color: rgba(0,0,0,0.8);
	}	
	.body1::before {
		content:"";
		background-image: url('https://txtzz-1301452902.file.myqcloud.com/bg.webp');
		width:100vw;
		height:100vh;  
		background-size: cover;
		transform: var(--weizhi);
		position:absolute;
		transition: var(--transition);
	}
	.yangguang{
		transform: var(--weizhi);
	}
</style>
