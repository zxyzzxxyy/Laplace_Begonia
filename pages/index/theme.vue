<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin1" :replymessageshow="replymessageshow"></titles>
		<!--center-->
		<scroll-view :scroll-y="true" style="height:100vh;z-index: -1;" 
		 :scroll-into-view="scroll_top1" scroll-with-animation=""   @scrolltolower="getmorereply()" >
			<view style="height:5vh">
			</view>
		<view style="width:100%;display: flex;flex-direction: column;align-items: center;background-color: #f8f8f8;z-index: 1;padding-bottom: 0%;background-size: cover;background-attachment: fixed;height:100vh" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}"
		
		>
		<view style="min-height:5vh;width: 100%;"></view>
		<view style="width:60rem;background-color: white;height:auto;padding-left:5%;padding-right: 5%;padding-bottom: 1%;padding-top: 2%;border-radius: 10px;"
		 :style="{'opacity':op}"
		>
			<view style="display: flex;flex-direction: row;margin-bottom: 20px;align-items: center;"  v-if="createtime!=0">
				<u-tag :text="fenlei"  plain plainFill    style="margin-right: 10px;" v-if="themeid!=0"></u-tag>
				<u-tag text="精华"  plain plainFill  v-if="jing==1" type="error" style="margin-right: 10px;"></u-tag>
				<view style="">
				<u-tag text="原创"  plain plainFill  v-if="type==0" type="warning"></u-tag>
				<u-tag text="搬运"  plain plainFill  v-if="type==1" type="warning"></u-tag>
				</view>
				<view style="margin-left: auto;display: flex;flex-direction: row;">
				<view style="display: flex;flex-direction: row;align-items: center;" v-if="juge_admin()">
					
					<u-icon name="file-text"  size="23" style="margin-left: 10px;" ></u-icon>
					<text style="font-size: 18px;color:#8f8f8f" @click="changetheme()" >编辑</text>
				
				</view>
				<view style="display: flex;flex-direction: row;margin-left: 10px;align-items: center;" v-if="juge_admin()">
					
					<u-icon name="integral"  size="23" style="margin-left: 10px;" ></u-icon>
					<text style="font-size: 18px;color:#8f8f8f" @click="setjing()" v-if="jing!=1" >设置精华</text>
					<text style="font-size: 18px;color:#8f8f8f" @click="deletejing()" v-if="jing==1">取消精华</text>
					
				</view>
				
				<view style="display: flex;flex-direction: row;margin-left: 10px;align-items: center;" v-if="juge_admin()">
					
					<u-icon name="arrow-up-fill"  size="23" style="margin-left: 10px;" ></u-icon>
					<text style="font-size: 18px;color:#8f8f8f" @click="settop()" v-if="top!=1" >设置置顶</text>
					<text style="font-size: 18px;color:#8f8f8f" @click="deletetop()" v-if="top==1">取消置顶</text>
					
				</view>
				
				<view style="display: flex;flex-direction: row;align-items: center;">
				<u-icon name="trash"  size="23" style="margin-left: 20px;" v-show="juge()"></u-icon>
				<text style="font-size: 18px;color:#8f8f8f" @click="show=true" v-show="juge()">删除</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;">
				<u-icon name="arrow-left"  size="23" style="margin-left: 15px;" ></u-icon>
				<text style="font-size: 18px;color:#8f8f8f"  @click="back()">返回</text>
				</view>
				</view>
			</view>
			<view style="width:100%;display: flex;justify-content: center;margin: 30px;">
			<text style="user-select:text;font-size: 25px;font-weight: 700;">{{title}}</text>
			</view>
			<view v-if="this.createtime!=0" >
				<view style="width: 100%;padding: 0%;display: flex;flex-direction: row;;
				margin-left: 0%;
				" >
				
					
					<view style="height:50px;display: flex;flex-direction: column;margin-left: 0px;margin-bottom: 10px;width: 100%;flex-direction: row;align-items: center;">
						<view style="width:50px;margin-right: 5px;height:50px;display: flex;"  @click="heself()">
						<u-avatar mode="aspectFill" :src="userheadimage1" v-if="userheadimage1!=null" size="50"></u-avatar>
						<u-avatar :text="username1.substring(0,1)"  randomBgColor v-if="userheadimage1==null" size="50"></u-avatar>
						</view>
						<view style="width:100%">
							<view style="display: flex;flex-direction: row; min-width:100%;margin-left: 5px;">
								<view style="display: flex;align-items:center;width:100%">
										<text style="margin-right: 10px;font-size:16px">{{username1}}</text>
										<text style="font-size: 16px;" v-if="shenfen.length!=0">·</text><text style="margin-left:10px;font-size: 16px;color:#909399" v-if="shenfen.length!=0">{{shenfen}}</text>
										<view style="margin-left: auto;margin-right: 10px;" v-if="changetime!=0">
											<text style="color:#909399;font-size: 16px;">编辑于{{time1(changetime)}}</text>
										</view>
							</view>
							<view style="margin-left: auto;" v-if="jugemyself()">
							<view style="width:55px;height:32px;background-color: #8bc863;border-radius: 3px;display: flex;align-items: center;justify-content: center;color:white;border-radius: 3px;"  v-show="jugelogin()&&!jugeattention()" @click="addattention()">关注</view>
							<view style="width:53px;height:30px;background-color: #ecf5ff;border: 1px solid #8bc863;color:#8bc863;display: flex;align-items: center;justify-content: center;border-radius: 3px;"  v-show="jugelogin()&&jugeattention()" @click="deleteattention()">已关注</view>
							</view>
							</view>
							<view style="display: flex;flex-direction: row;align-items: center;margin-left: 5px;">
								<text style="font-size: 14px;color:#838383;margin-right: 5px;">{{time(createtime)}}</text>
								<u-icon name="eye"></u-icon>
								<text style="color:#838383;font-size: 14px;margin-left: 2px;">{{look}}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view style="width:100%;display: flex;align-items: center;justify-content: center;">
			<!--	<text style="user-select:text;font-size: 25px;font-weight: 700;">{{title}}</text>-->
				</view><br>

			<mp-html 
			:content="jugeemoji(article)" 
			:lazy-load="true"
			style="width:100%"
			class="mp"
			/>
			<br>
			<u--text  @click="weizhi" style="margin-left: 0vw;" prefixIcon="map-fill" :text="position" size="17" v-if="position!=undefined&&lal!=''" color="#8bc863" iconStyle="color:#8bc863;margin-right:5px;size:17px"></u--text>
			<view style="display: flex;flex-direction: row;padding-left: 0%;">
			<u-tag v-for="(item,index) in tags" :text="item"  icon="tags-fill" style="width: auto;margin-right:10px" plain plainFill @click="linktag(item)"></u-tag>
			</view>
			<view style="margin-top: 20px;" v-if="type==1">
				<hr>
				<br>
				<text style="font-size:18px;font-weight: 700;">原文链接:</text><text @click="linkto()" style="font-weight: 700;color:#4a95df;">{{ycsrc}}</text>
			</view>
			<view style="display: flex;flex-direction: row;align-items: center;padding:15px;justify-content: center;width:100%;margin-top: 20px;"
			 v-if="this.createtime!=0"
			>
				<view style="display: flex;flex-direction: row;align-items: center;color:#838383" v-show="!jugelike(themeid)"
				@click="setlike(themeid)"
				>
					<u-icon name="thumb-up"  size="25" style="margin-right: 3px;"></u-icon>
					<text v-if="like==0" style="font-size:17px;">赞</text>
					<text v-if="like!=0" style="font-size: 17px;">{{like}}</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;color:#838383" v-show="jugelike(themeid)"
				@click="deletelike(themeid)"
				>
					<u-icon name="thumb-up-fill" color="#8bc863"  size="25" style="margin-right: 3px;" ></u-icon>
					<text  style="font-size: 17px;">{{like}}</text>
				</view>
				<view v-show="saveshow==0" style="display: flex;flex-direction: row;align-items: center;color:#838383;margin-left: 30%;margin-right: 30%;" @click="saveadd()">
					<u-icon name="star"  size="25" style="margin-right: 3px;"></u-icon>
					<text  style="font-size: 17px;">收藏</text>
				</view>
				<view v-show="saveshow==1" style="display: flex;flex-direction: row;align-items: center;color:#838383;margin-left: 30%;margin-right: 30%;" @click="savedelete()">
					<u-icon name="star"  size="25" style="margin-right: 3px;" color="#f9ae3d"></u-icon>
					<text  style="font-size: 17px;">已收藏</text>
				</view>
				<view style="display: flex;flex-direction: row;align-items: center;color:#838383">
					<u-icon name="share"  size="25" style="margin-right: 3px;"></u-icon>
					<text  style="font-size: 17px;">分享</text>
				</view>
			</view>
		</view>
		
		<view style="width:60rem;background-color: white;height:auto;padding-left:5%;padding-right: 5%;padding-bottom: 1%;padding-top: 1%;border-radius: 10px;margin-top: 20px;"
		 :style="{'opacity':op}" v-show="tuijianlist.length>0"
		>
			<view style="width:100%">
				<text style="font-size: 20px;font-weight: 700;">推荐文章</text>
			</view>
			<u-divider></u-divider>
			<view v-for="(item,index) in tuijianlist"  @click="totheme(item.themeid)">
				<view style="display: flex;flex-direction: row;align-items: center;justify-content: center;">
					<view style="width:100%">
						<view style="">
							<text style="font-size: 16px;font-weight: 700;">{{item.title}}</text>
						</view>
						<view style="margin-top: 10px;">
							<u--text :text="item.data" size="14" lines="1" color="#909399"></u--text>
						</view>
						<view style="width: 100%;display: flex;flex-direction: row;margin-top: 10px;align-items: center;">
							
							<u-avatar mode="aspectFill" :src="item.userheadimage" v-if="item.userheadimage!=null" size="30"></u-avatar>
							<u-avatar :text="item.username.substring(0,1)"  randomBgColor v-if="item.userheadimage==null" size="30"></u-avatar>					
							<text style="font-size: 14px;margin-left: 10px;color:#909399" >
								{{item.username}}
							</text>
							<text style="font-size: 14px;width:100px;margin-left: 30px;color:#909399" >
								{{item.fenlei}}
							</text>
							<u-icon name="eye-fill" size="14"></u-icon>
							<text style="font-size: 14px;width:50px;margin-left: 5px;color:#909399" >
								{{item.look}}
							</text>
							<u-icon name="thumb-up-fill" size="14"></u-icon>
							<text style="font-size: 14px;width:50px;margin-left: 5px;color:#909399" >
								{{item.likes}}
							</text>
							<u-icon name="chat-fill" size="14"></u-icon>
							<text style="font-size: 14px;width:50px;margin-left: 5px;color:#909399" >
								{{item.num}}
							</text>
						</view>
					</view>
				
					<view v-if="item.image!=null&&item.image.length>0" style="margin-left: 10px;">
						<u--image mode="aspectFill" :showLoading="true" :src="imagesrc1(item.image)" width="100px" height="100px" shape="square" radius="10px"></u--image>
					</view>
				
				</view>
				
				
			<u-divider></u-divider>
			</view>
		</view>
		
		<view style="width:60rem;background-color: white;height:auto;padding-left:5%;padding-right: 5%;padding-bottom: 1%;padding-top: 1%;border-radius: 10px;margin-top: 20px;"
		 :style="{'opacity':op}" v-show="createtime>0"
		>
		
			<view style="width:100%;margin-bottom: 20px;">
				<view style="width:100%;display: flex;flex-direction: row;align-items: center;">
				
				<text>{{num}}条评论</text>
				<text style="font-size: 14px;margin-left: 10px;" v-if="replytype==2" @click="getreply(1)">查看全部</text>
				<text style="font-size: 14px;margin-left: 10px;font-weight: 700;" v-if="replytype==1"  @click="getreply(1)">查看全部</text>
				<text style="font-size: 14px;margin-left: 10px"  v-if="replytype!=2"  @click="getreply(2)">只看作者</text>
				<text style="font-size: 14px;margin-left: 10px;font-weight: 700"  v-if="replytype!=1" @click="getreply(2)">只看作者</text>
				
				<u-button type="primary" text="切换编辑器" style="width:80px;margin-right:5px;margin-left: auto;" v-if="jugelogin()" @click="show2=!show2"></u-button>
				
				</view>
				<view style="border-radius: 5px;border:solid 1px #eaeaeb;margin-top: 10px;" v-if="jugelogin()&&show2">
					<textarea v-model="reply" placeholder="写下我的评论..." style="background-color: #f4f4f5;width:98%;height:170px;padding-left:1%;padding-right:1%;padding-top: 0.5%;border-radius: 0;" maxlength="1500" @input="changenum" ></textarea>
					<view style="width:97%;height:20px;background-color: #f4f4f5;padding-left:1.5%;padding-right: 1.5%;font-size: 10px;color:grey;border-bottom: solid 1px #eaeaeb;">
						<text style="margin-left: 95%;">{{replynum}}/1500</text>
					</view>
					<view v-show="chooseimage" style="padding:2%;border-bottom: solid 1px #eaeaeb;display: flex;flex-direction: row;flex-wrap: wrap;">
						<u--image v-for="(item,index) in uploadimages" :showLoading="true" :src="item" width="120px" height="120px" @click="deleteimage(index)" mode="aspectFill" style="margin-right:10px;border-radius: 5px;margin-bottom: 5px;"></u--image>
						<view style="width:120px;height:120px;border-radius: 5px;background-color: #f4f4f5;display: flex;flex-direction: column;align-items: center;justify-content: center;" @click="uploadimage()" v-if="uploadimages.length<9">
							<u-icon name="plus" color="#909399" size="28"></u-icon>
							<text style="color:#909399">上传图片</text>
						</view>
					</view>
					<view v-if="height2&&jugelogin()" style="width:100%;height:300px;position: relative;background-color: #f6f7f8;margin-top: 0px;"
					>
						<swiper class="swiper" style="height:250px;;margin-bottom: 50px;"
						:current="emojiselect" circular
						@change="changeemojiselect"
						>
							<swiper-item v-for="(item,index2) in emoji">
								<view
									style="height:240px;overflow-y: auto;display: flex;flex-direction: row;flex-wrap: wrap;"
								>
									<image :src="item"  v-for="(item,index) in emojilist[index2]"
										style="width:50px;height:50px;margin-top:10px;margin-bottom:10px;margin-left:10px;margin-right: 10px;" 
										mode="aspectFill"
										@click="addemoji(index,0)"
									>
												
									</image>
								</view>
							</swiper-item>								
						</swiper>
					
						<view style="position: absolute;width:100%;bottom: 0;height:60px;background-color: white;display:flex;flex-direction:row;overflow-x:auto">
							<view
							 :class="{'selectemoji':emojiselect==index}"
							 v-for="(item,index) in emoji" style="width:60px;height:60px;margin-right:10px;display: flex;align-items: center;justify-content: center;">
							<image @click="emojiselect=index"
							
							 style="width:40px;height:40px;margin-top:0px;margin-bottom:0px;" mode="aspectFill"
								:src="emoji[index].icon"
							>
								
							</image>
							</view>
						</view>
					</view>
					<view style="padding-top:10px;padding-bottom: 10px;padding-left:5px ;display: flex;flex-direction: row;">
						<u-icon name="photo" :color="chooseimagecolor" size="35" @click="chooseimage=!chooseimage,height2=0,changeimagecolor(),chooseemojicolor='#c4c6c9'"></u-icon>
						<u-icon name="more-circle" :color="chooseemojicolor" size="30" @click="height2=!height2,chooseimage=0,changeemojicolor()"></u-icon>
						<u-button type="primary" text="回复" style="width:80px;margin-right:5px;margin-left: auto;" @click="huifu(1)"></u-button>
					</view>
					
				</view>
				
				
				
				<view style="margin-top: 10px;" v-if="jugelogin()&&!show2">
					<lRichTextEditor
						@text="con"
						v-model="reply"
						style="width:100%;"
						>
					</lRichTextEditor>
					<view style="border:solid 1px #ccc;border-top:none;padding-top:10px;padding-bottom: 10px;padding-left:5px ;display: flex;flex-direction: row;">

						<u-button type="primary" text="回复" style="width:80px;margin-right:5px;margin-left: auto;" @click="huifu(1)"></u-button>
					</view>
				</view>
			</view>
			
			<view v-for="(item,index) in replys" style="width:100%;margin-top: 10px;margin-bottom: 10px;" :id="'a'+item.id">
				<view style="display: flex;flex-direction: row;width:100%">
					<u-avatar mode="aspectFill" :src="item.userheadimage"   @click="" size="50" @click="heself1(item.userid)"
					v-if="item.userheadimage!=''"
					></u-avatar>
					<u-avatar :text="item.username.substring(0,1)"  randomBgColor  size="50" v-if="item.userheadimage==''"  @click="heself1(item.userid)"></u-avatar>
					<view style="display: flex;flex-direction: column;justify-content: center;margin-left: 10px;width: 100%;">
						<view style="height:50px;display: flex;flex-direction: column;justify-content: center;">
						<view style="display: flex;flex-direction: row;align-items: center;">
						<view style="display: flex;align-items:center;justify-content: center;">
							<text style="margin-right: 10px;font-size:15px">{{item.username}}</text><text style="font-size: 15px;" v-if="item.shenfen.length!=0">·</text>
							<text style="margin-left:10px;font-size: 15px;color:#909399" v-if="item.shenfen.length!=0">{{item.shenfen}}</text>
						</view>
						<u-tag text="作者"   size="mini" style="margin-left: 5px;" v-if="zuozhe(item.userid)"></u-tag>
						
						
						<u-icon name="trash"  size="23" style="margin-left: auto;margin-right: 2%;" v-show="jugeuserid(item.userid)"
						@click="deletereply(item.louceng)"
						></u-icon>
						</view>
						<view style="display: flex;flex-direction: row;">
						<text  style="font-size: 15px;color:#909399;margin-right: 10px;">{{item.louceng}}楼</text>							
						<text style="font-size: 15px;" v-if="item.shenfen.length!=0">·</text>
						<text style="font-size: 15px;color:#909399;margin-left: 10px;">{{time1(item.time)}}</text>						
						</view>
						</view>
						<view style="border-radius: 5px;width:100%;;padding-top: 10px;padding-bottom: 10px;" v-if="item.data!=''">
							<!--<text style="font-size: 16px;user-select:text">{{item.data}}</text>-->
							<mp-html
							:content="jugeemoji(item.data)" 
							:lazy-load="true"
							style="width:100%"
							class="mp"
							/>
						</view>
						<view style="margin-top: 10px;">
							<u-album  :urls="imagesrc(item.image)" multipleSize="280" singleSize="400" space="10"></u-album>
						</view>
						<view style="margin-left: 0px;font-size: 15px;color:#909399;margin-top: 10px;">
						<u--text :lines="2" :text="item.sign" color="#909399" v-if="item.sign!=''&&item.sign!=''"></u--text>
						<view style="height:5px" v-if="item.sign!=null&&item.sign!=''">
							
						</view>
						<u-divider v-if="item.loucengreply.length>0"></u-divider>
						<view style=";width:96%;
							margin-top: 0px;border-radius: 5px;
						"	:class="{'color1':item.loucengreply.length==0,'color2':item.loucengreply.length>0}"
						
						>
							<view v-for="(item1,index) in item.loucengreply" v-show="jugeshowall(index,item.sub)">
								<view style="height:45px;display: flex;flex-direction: row;">
									<view style="margin-right:5px">
										<u-avatar mode="aspectFill" :src="item1.userheadimage"   @click="" size="45" @click="heself1(item1.userid)"
										v-if="item1.userheadimage!=''"
										></u-avatar>
										<u-avatar :text="item1.username.substring(0,1)"  randomBgColor  size="45" v-if="item1.userheadimage==''"  @click="heself1(item1.userid)"></u-avatar>
										
									</view>
									<view style="margin-left: 5px;width:100%">
										<view style="display: flex;align-items:center;width:100%">
											<text style="margin-right: 10px;font-size:15px;color:#66686c">{{item1.username}}</text>
											<text style="font-size: 15px;;color:#66686c" v-if="item1.shenfen.length!=0">·</text>
											<text style="margin-left:10px;font-size: 15px;color:#66686c" v-if="item1.shenfen.length!=0">{{item1.shenfen}}</text>
											<u-tag text="作者"   size="mini" style="margin-left: 5px;" v-if="zuozhe(item1.userid)"></u-tag>
											<view  style="margin-left: auto" v-show="jugeuserid(item1.userid)"
											@click="deletereplylouceng(item1.id)">
												<u-icon name="trash"  size="23"
												></u-icon>
											</view>		
										</view>
										<view >
											<text style="font-size: 15px;color:#66686c;">{{time1(item1.time)}}</text>
										</view>
									</view>
									
								</view>
								<view style="margin-left: 55px;margin-top: 5px;;color:#66686c">
								
									<mp-html
									:content="jugeemoji(item1.data)" 
									:lazy-load="true"
									style="width:100%"
									class="mp"
									/>
								</view>
								<u-divider></u-divider>
							</view>
							<view style="border-radius: 5px;border:solid 1px #eaeaeb;margin-top: 10px;margin-bottom: 10px;" v-if="jugelogin()&&item.sub1==1">
								<textarea v-model="item.loucengreplydata" placeholder="写下我的评论..." style="background-color: #f4f4f5;width:98%;height:170px;padding-left:1%;padding-right:1%;padding-top: 0.5%;border-radius: 0;" maxlength="1500" @input="changenum" ></textarea>
								<view style="width:97%;height:20px;background-color: #f4f4f5;padding-left:1.5%;padding-right: 1.5%;font-size: 10px;color:grey;border-bottom: solid 1px #eaeaeb;">
									<text style="margin-left: 95%;">{{item.loucengreplydata.length}}/1500</text>
								</view>
								<view style="padding-top:10px;padding-bottom: 10px;padding-left:5px ;display: flex;flex-direction: row;">
									
									<u-button type="primary" text="回复" style="width:80px;margin-right:5px;margin-left: auto;" @click="replylouceng(item)"></u-button>
								</view>
							</view>
							<view style="width:100%;color:#8bc863;display: flex;flex-direction: row;">
							
								<view v-if="item.loucengreply.length>3&&item.sub==0" @click="item.sub=1">
									<text>查看全部{{item.loucengreply.length}}条回复</text>
								</view>
								<view v-if="item.loucengreply.length>3&&item.sub==1" @click="item.sub=0">
									<text>收起</text>
								</view>
								<view style="margin-left: auto;" @click="item.sub1=!item.sub1" v-if="jugelogin()">
									<text>回复</text>
								</view>
							</view>
						</view>
					
						</view>
					</view>
					
				</view>
				<u-divider text="" style="width: 100%;"></u-divider>
			</view>
			<view style="width: 100%;display: flex;flex-direction: row;align-items: center;justify-content: center;font-size: 15px;color:#787878">
				<text>没有更多了</text>
			</view>
			
		</view>
		<view style="height:5vh;width:100vw;opacity: 0;">&empt</view>
		</view>
		
		</scroll-view>
		
		<view >
				<u-modal :show="show" title="删除主题" content="是否删除主题?" showCancelButton
				@cancel="show=false" @confirm="delete_theme()" :closeOnClickOverlay="true"
				@close="show=false" 
				></u-modal>
		</view>
		<view >
				<u-modal :show="show1" title="删除主题" content="是否删除回复?" showCancelButton
				@cancel="show1=false" @confirm="deletereply1()" :closeOnClickOverlay="true"
				@close="show1=false" 
				></u-modal>
		</view>
		<view>
				<u-toast ref="uToast"></u-toast>
				
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
				emojiselect:0,
				height2:0,
				index:0,
				key:"",
				fangdou1:0,
				fangdou2:true,
				icon:"../../static/icon.png",
				chooseimage:false,
				backgroundimage:"",
				attentionlist:[],
				chooseimagecolor:"#c4c6c9",
				chooseemojicolor:"#c4c6c9",
				userheadimage:"",
				ycsrc:"",
				show:false,
				op:0.95,
				reply:"",
				replys:[],
				article:"",
				username:"",
				title:"",
				tags:[],
				themeid:0,
				admin1:0,
				show1:false,
				replytype:1,
				userid:0,
				jing:0,
				type:0,
				fangdou:1,
				changetime:0,
				look:0,
				saveshow:0,
				top:0,
				like:0,
				scroll_top:0,
				scroll_top1:"",
				createtime:0,
				username1:"",
				userheadimage1:"",
				mylike:[],
				num:0,
				sign:"",
				uploadimages:[],
				replynum:0,
				admin:0,
				savelist:[],
				shenfen:"",
				show2:true,
				loucenglist:[],
				list:[
					{
						type: 'success',
						title: '成功',
						message: "删除成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png',
						url:"index"
					},
					{
						type: 'error',
						title: "失败",
						message:"删除失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'success',
						title: "成功",
						message:"设置成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"设置失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"其它设备登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"请登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'success',
						title: "成功",
						message:"回复成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"回复失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"删除失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'success',
						title: "成功",
						message:"删除成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"内容不能为空",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
					{
						type: 'error',
						title: "失败",
						message:"账号禁用中",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					},
				],
				replymessageshow:false,
				loucengreplydata:"",
				tuijianlist:[],
			emoji:[
				{
					"text":"Monaka",
					"icon":"/static/mnk/mnk_16.png",
					"type":"png"
				},
				{
					"text":"Monaka",
					"icon":"/static/mnk/mnk_32.jpg",
					"type":"jpg"
				},
				{
					"text":"Taffy",
					"icon":"/static/taffy/taffy_3.png",
					"type":"png"
				},
				{
					"text":"明前奶绿",
					"icon":"/static/mqnl/mqnl_9.png"
				}
			],
			emojilist:[
				[
				"/static/mnk/mnk_1.png",
				"/static/mnk/mnk_2.png",
				"/static/mnk/mnk_3.png",
				"/static/mnk/mnk_4.png",
				"/static/mnk/mnk_5.png",
				"/static/mnk/mnk_6.png",
				"/static/mnk/mnk_7.png",
				"/static/mnk/mnk_8.png",
				"/static/mnk/mnk_9.png",
				"/static/mnk/mnk_10.png",
				"/static/mnk/mnk_11.png",
				"/static/mnk/mnk_12.png",
				"/static/mnk/mnk_13.png",
				"/static/mnk/mnk_14.png",
				"/static/mnk/mnk_15.png",
				"/static/mnk/mnk_16.png",
				"/static/mnk/mnk_17.png",
				"/static/mnk/mnk_18.png",
				"/static/mnk/mnk_19.png",
				"/static/mnk/mnk_20.png",
				"/static/mnk/mnk_21.png",
				"/static/mnk/mnk_22.png",
				"/static/mnk/mnk_23.png",
				"/static/mnk/mnk_24.png",
				"/static/mnk/mnk_25.png",
				"/static/mnk/mnk_26.png",
				"/static/mnk/mnk_27.png",
				"/static/mnk/mnk_28.png",
				"/static/mnk/mnk_29.png",
				"/static/mnk/mnk_40.png",
				"/static/mnk/mnk_41.png",
				"/static/mnk/mnk_42.png",
				"/static/mnk/mnk_43.png",
				"/static/mnk/mnk_44.png",
				"/static/mnk/mnk_45.png",
				"/static/mnk/mnk_46.png",
				"/static/mnk/mnk_47.png",
				"/static/mnk/mnk_48.png",
				],
				[
				"/static/mnk/mnk_30.jpg",
				"/static/mnk/mnk_31.jpg",
				"/static/mnk/mnk_32.jpg",
				"/static/mnk/mnk_33.jpg",
				"/static/mnk/mnk_34.jpg",
				"/static/mnk/mnk_35.jpg",
				"/static/mnk/mnk_36.jpg",
				"/static/mnk/mnk_37.jpg",
				"/static/mnk/mnk_38.jpg",
				"/static/mnk/mnk_39.jpg",
				],
				[
				"/static/taffy/taffy_1.png",
				"/static/taffy/taffy_2.png",
				"/static/taffy/taffy_3.png",
				"/static/taffy/taffy_4.png",
				"/static/taffy/taffy_5.png",
				"/static/taffy/taffy_6.png",
				"/static/taffy/taffy_7.png",
				"/static/taffy/taffy_8.png",
				"/static/taffy/taffy_9.png",
				"/static/taffy/taffy_10.png",
				"/static/taffy/taffy_11.png",
				"/static/taffy/taffy_12.png",
				"/static/taffy/taffy_13.png",
				"/static/taffy/taffy_14.png",
				"/static/taffy/taffy_15.png",
				"/static/taffy/taffy_16.png",
				"/static/taffy/taffy_17.png",
				"/static/taffy/taffy_18.png",
				"/static/taffy/taffy_19.png",
				"/static/taffy/taffy_20.png",
				"/static/taffy/taffy_21.png",
				"/static/taffy/taffy_22.png",
				"/static/taffy/taffy_23.png",
				"/static/taffy/taffy_24.png",
				"/static/taffy/taffy_25.png",
				"/static/taffy/taffy_26.png",
				"/static/taffy/taffy_27.png",
				"/static/taffy/taffy_28.png",
				"/static/taffy/taffy_29.png",
				"/static/taffy/taffy_30.png",
				"/static/taffy/taffy_31.png",
				"/static/taffy/taffy_32.png",
				"/static/taffy/taffy_33.png",
				"/static/taffy/taffy_34.png",
				"/static/taffy/taffy_35.png",
				],
				[
					"/static/mqnl/mqnl_1.png",
					"/static/mqnl/mqnl_2.png",
					"/static/mqnl/mqnl_3.png",
					"/static/mqnl/mqnl_4.png",
					"/static/mqnl/mqnl_5.png",
					"/static/mqnl/mqnl_6.png",
					"/static/mqnl/mqnl_7.png",
					"/static/mqnl/mqnl_8.png",
					"/static/mqnl/mqnl_9.png",
					"/static/mqnl/mqnl_10.png",
					"/static/mqnl/mqnl_11.png",
					"/static/mqnl/mqnl_12.png",
					"/static/mqnl/mqnl_13.png",
					"/static/mqnl/mqnl_14.png",
					"/static/mqnl/mqnl_15.png",
					"/static/mqnl/mqnl_16.png",
					"/static/mqnl/mqnl_17.png",
					"/static/mqnl/mqnl_18.png",
					"/static/mqnl/mqnl_19.png",
					"/static/mqnl/mqnl_20.png",
					"/static/mqnl/mqnl_21.png",
					"/static/mqnl/mqnl_22.png",
					"/static/mqnl/mqnl_23.png",
					"/static/mqnl/mqnl_24.png",
					"/static/mqnl/mqnl_25.png",
				]
			],
			emojijugelist:[
				
					[
					"[mnk_早上花]",
					"[mnk_下午花]",
					"[mnk_晚安喵]",
					"[mnk_问号]",
					"[mnk_嘻嘻喵]",
					"[mnk_mua]",
					"[mnk_不吃三吨]",
					"[mnk_好耶1]",
					"[mnk_溜了]",
					"[mnk_美梦]",
					"[mnk_喵]",
					"[mnk_撬棍]",
					"[mnk_嘻嘻]",
					"[mnk_笑嘻了]",
					"[mnk_致命打击]",
					"[mnk_好耶2]",
					"[mnk_+3]",
					"[mnk_+100]",
					"[mnk_DAY0]",
					"[mnk_爱的力量]",
					"[mnk_发点能回的]",
					"[mnk_复读机]",
					"[mnk_姑奶喵]",
					"[mnk_尼奥喵]",
					"[mnk_思考考]",
					"[mnk_小趴菜]",
					"[mnk_总督1]",
					"[mnk_总督2]",
					"[mnk_mona卡]",
					"[mnk_no喵]",
					"[mnk_呃]",
					"[mnk_哼]",
					"[mnk_rua头]",
					"[mnk_上勾拳]",
					"[mnk_手摇奶茶]",
					"[mnk_呜呜喵]",
					"[mnk_雌小鬼]",
					"[mnk_主人主人]",
					],
					[
					"[mnk_mnk]",
					"[mnk_噔噔咚]",
					"[mnk_来了来了]",
					"[mnk_流鼻血]",
					"[mnk_嗯了]",
					"[mnk_你好]",
					"[mnk_菩萨包邮]",
					"[mnk_请]",
					"[mnk_停止响应]",
					"[mnk_再来一局]",
					],
					[
					"[taffy_散步]","[taffy_主人]","[taffy_耶咿]","[taffy_呜呜呜]","[taffy_晚上花]","[taffy_投降]","[taffy_收收味]","[taffy_切割]","[taffy_嗯打游戏]","[taffy_K48]","[taffy_好听]","[taffy_珍珠奶茶灌我嘴里]","[taffy_回私信]","[taffy_活下去]","[taffy_加油喵]","[taffy_好似喵]","[taffy_留条命]","[taffy_啊啊]"
					,"[taffy_绿豆嫌弃]","[taffy_嗯]","[taffy_857]","[taffy_mua喵]","[taffy_白白喵]","[taffy_爆金币咯]","[taffy_别急]","[taffy_不敢想了]","[taffy_不玩了]","[taffy_草喵]","[taffy_超塔菲]","[taffy_吹爆]","[taffy_大骗子]","[taffy_疑惑]","[taffy_单推塔菲]","[taffy_愤怒愤怒!]","[taffy_高伤害]"
					],
					[
						"[明前奶绿_在吗]","[明前奶绿_?]","[明前奶绿_fa店]","[明前奶绿_MA]","[明前奶绿_抱歉]",
						"[明前奶绿_别急]","[明前奶绿_呃呃]","[明前奶绿_放弃]","[明前奶绿_嘿嘿]","[明前奶绿_哼哼]",
						"[明前奶绿_花花]","[明前奶绿_灰头土脸]","[明前奶绿_加油]","[明前奶绿_惊讶]","[明前奶绿_懒]",
						"[明前奶绿_嗯嗯]","[明前奶绿_起床]","[明前奶绿_亲亲]","[明前奶绿_确实]","[明前奶绿_生气]",
						"[明前奶绿_我会了]","[明前奶绿_蟹蟹!]","[明前奶绿_早安]","[明前奶绿_这是什么]","[明前奶绿_真的假的?]",
					]
				
			],
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
			let that = this;
		
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			//console.log(this.userbackgroundimage);
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.op=getApp().globalData.op;
		},
		methods: {
			addemoji(e,i){
				if(i==0){
				this.reply+=this.emojijugelist[this.emojiselect][e];
				this.replynum+=this.emojijugelist[this.emojiselect][e].length;
				}
				if(i==1){
					this.replydata+=this.emojijugelist[this.emojiselect][e];
					this.replynum+=this.emojijugelist[this.emojiselect][e].length;
				}
			},
			getreply:function(e){
				this.replys=[];
				this.replytype=e;
				this.status="loading";
				uni.request({
					url:getApp().globalData.http+"/api/getreply",
					method:"POST",
					data:{
						"themeid":this.themeid,
						"id":1,
						"replytype":this.replytype
					},
					success: (res) => {
						//console.log(res.data);
						if(res.data.code==1001){
							for(var i=0;i<res.data.data.length;i++){
							
								this.replys.push(res.data.data[i]);
								this.$set(this.replys[i],"sub",0);
								this.$set(this.replys[i],"sub1",0);
								this.$set(this.replys[i],"loucengreplydata",'');
							}
						if(res.data.data.length==0){
							this.status="nomore";
						}
						else if(res.data.data.length<10){
							this.replys[this.replys.length-1].id=
							this.replys[this.replys.length-1].id+1;
							this.status="loadmore"
						}
						else {
							this.getmore=1;
							this.status="loadmore"
						}
						}
						this.fangdou2=false;
					},fail: (res) => {
						this.fangdou2=false;
					}
				})
			},
			getmorereply:function(){
				uni.$u.throttle(this.getmorereply1, 500)
			},
			getmorereply1:function(){
				if(this.fangdou2==true)return;this.fangdou2==true;
			
				this.status="loading";
				var id=0;
				if(this.replys.length==0)id=0;else id=this.replys[this.replys.length-1].id-1;
				uni.request({
					url:getApp().globalData.http+"/api/getreply",
					method:"POST",
					data:{
						"themeid":this.themeid,
						"id":id,
						"replytype":this.replytype
					},
					success: (res) => {
						//console.log(res.data);
						if(res.data.code==1001){
							console.log(res.data.data);
							for(var i=0;i<res.data.data.length;i++){
								res.data.data[i].sub=0;
								res.data.data[i].sub1=0;
								res.data.data[i].loucengreplydata="";
								this.replys.push(res.data.data[i]);
							
							}
							if(res.data.data.length==0){
								this.status="nomore";
							}
							else if(res.data.data.length<10){
								this.replys[this.replys.length-1].id=
								this.replys[this.replys.length-1].id+1;
								this.status="loadmore"
							}
							else {
								this.getmore=1;
								this.status="loadmore"
							}
							setTimeout(() => {
								this.fangdou2=false;
							}, 1000)
						}
					},fail: (res) => {
						this.fangdou2=false;
					}
				})
			},
			con(e){
				this.reply=e;
			},
			jugeshowall(index,sub){
				
				if(sub==1)return true;
				if(index>=3&&sub==0)return false;
				if(index<3&&sub==0)return true;
			},
			deletereplylouceng:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/deleteloucengreply",
					method:"POST",
					data:{
						id:e
					},
					success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<this.replys.length;i++){
								for(var j=0;j<this.replys[i].loucengreply.length;j++)
									if(this.replys[i].loucengreply[j].id==e){
										this.replys[i].loucengreply.splice(j,1)
										uni.showToast({
											title:"删除成功",
											icon:"success"
										})
										return;
									}
							}
						
						}else{
							uni.showToast({
								title:"删除失败",
								icon:"error"
							})
						}
					},
					fail:(res)=>{
						uni.showToast({
							title:"删除失败",
							icon:"error"
						})
					}
				})
			},
			replylouceng:function(e){

				if(this.fangdou1==1)return;this.fangdou1=1;
				if(e.loucengreplydata.length==0)return;
				uni.request({
					url:getApp().globalData.http+"/api/newloucengreply",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						themeid:this.themeid,
						louceng:e.louceng,
						data:e.loucengreplydata
					},
					success: (res) => {
						if(res.data.code==1001){
							var temp={
								"username":getApp().globalData.username,
								"userheadimage":getApp().globalData.userheadimage,
								"userid":getApp().globalData.userid,
								"data":e.loucengreplydata,
								"time":res.data.time,
								"sign":getApp().globalData.sign,
								"shenfen":res.data.shenfen,
								"id":res.data.id
							}
							for(var i=0;i<this.replys.length;i++){
								if(this.replys[i].louceng==e.louceng){
									this.replys[i].loucengreply.push(temp);
									this.replys[i].loucengreplydata='';
									break;
								}
							}

							
							uni.showToast({
								title:"回复成功",
								icon:"success"
							})
							this.fangdou1=0;
						}else{
							uni.showToast({
								title:"回复失败",
								icon:"error"
							})
							this.fangdou1=0;
						}
					},
					fail:(res)=>{
						uni.showToast({
							title:"回复失败",
							icon:"error"
						})
						this.fangdou1=0;
					}
				})
			},
			scroll:function(e){
			
			},
			jugemyself:function(){
				if(this.userid==getApp().globalData.userid)return false;return true;
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
			changetheme:function(){
				uni.navigateTo({
					url:"edit?themeid="+this.themeid
				})
			},
			jugeemoji(e){
				if(e==undefined||e==null)return "";
				for(var j=0;j<this.emojijugelist.length;j++)
				for(var i=0;i<this.emojijugelist[j].length;i++){
					var sub=e.indexOf(this.emojijugelist[j][i]);
					while(sub!=-1){
						var imagename=this.emojilist[j][i];
						imagename="<img src='"+imagename+"' style='width:60px;height:60px;margin-left:5px;margin-right:5px'/>";
						e=e.substring(0,sub)
						+imagename+
						e.substring(sub+this.emojijugelist[j][i].length,e.length);
						sub=e.indexOf(this.emojijugelist[j][i]);
					}
				}
				
				return e;
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
			yulan:function(e){
				uni.previewImage({
							url: e,
							longPressActions: {
								itemList: [],
								success: function(data) {
									
								},
								fail: function(err) {
									
								}
							}
				});
			},
			jugelogin:function(){
				if(getApp().globalData.cookie!='')return true;return false;
			},
			linktag:function(e){
				uni.navigateTo({
					url:"tag?tagname="+e
				})
			},
			saveadd:function(){
				if(getApp().globalData.cookie==''){
					this.$refs.uToast.show({
						...this.list[5],
						complete() {
							return;
						}
					})
					return;
				}
				uni.request({
					url:getApp().globalData.http+"/api/addsave",
					method:"POST",
					data:{
						"themeid":this.themeid,
						"userid":getApp().globalData.userid
					},
					success: (res) => {
						//console.log(res.data.code);
						if(res.data.code==1001){
							this.saveshow=1;
						}
						
					}
				})
			},
			savedelete:function(){
				uni.request({
					url:getApp().globalData.http+"/api/savedelete",
					method:"POST",
					data:{
						"themeid":this.themeid,
						"userid":getApp().globalData.userid,
					},
					success: (res) => {
						//console.log(res.data.code);
						if(res.data.code==1001){
							this.saveshow=0;
						}
					}
				})
			},
			deletereply:function(e){
				this.deletelouceng=parseInt(e);
				this.show1=true;
			},
			totheme:function(e){
				uni.navigateTo({
					url:"theme?themeid="+e
				})
			},
			deletereply1:function(e){
				let themeid=this.themeid;
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/deletereply",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"louceng":this.deletelouceng,
						"themeid":this.themeid
					},
					success: (res) => {
						if(res.data.code==1001){
							
						this.$refs.uToast.show({
							...this.list[9],
							complete() {
							for(var i=0;i<that.replys.length;i++){
								if(that.replys[i].louceng==that.deletelouceng){
									that.replys.splice(i,1);
								}
							}
							}
						})
						}
						else{
							this.$refs.uToast.show({
								...this.list[8],
								complete() {
								
								}
							})
						}
						this.show1=false;
					},
					fail:(res)=>{
						this.$refs.uToast.show({
							...this.list[8],
							complete() {
							
							}
						})
						this.show1=false;
					}
				})
			},
			jugeuserid:function(e){
				if(getApp().globalData.admin==1||e==getApp().globalData.userid)return true;
				return false;
			},
			zuozhe:function(e){
				//console.log(e+" "+this.userid);
				if(e==this.userid)return true;
				return false;
			},
			search:function(){
				
				if(this.key=="")
				uni.navigateTo({
					url:"search"
				})
				else
				uni.navigateTo({
					url:"search?key="+this.key
				})
				this.key="";
			},
			imagesrc:function(e){
				if(e==null)return;
				var arr=e.split(';');
				arr.pop();
				return  arr;
			},
			imagesrc1:function(e){
				if(e==null)return;
				var arr=e.split(';');
				arr.pop();
				return  arr[0];
			},
			jugelogin:function(){
				if(getApp().globalData.cookie=="")return false;return true;
			},
			huifu:function(e){
			
				let themeid=this.themeid;
				let that=this;
				if(this.reply.length==0&&this.uploadimages.length==0){
					this.$refs.uToast.show({
						...this.list[10],
						complete() {
						
						}
					})
					return;
				}
				var image="";
				for(var i=0;i<this.uploadimages.length;i++){
					image=image+this.uploadimages[i]+";";
				}
				uni.request({
					url:getApp().globalData.http+"/api/reply",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"data":this.reply,
						"image":image,
						"themeid":this.themeid,
						"type":e
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.list[6],
								complete() {
									
								}
							})
							this.reply="";
						}
						else if(res.data.code==1002){
							this.$refs.uToast.show({
								...this.list[11],
								complete() {
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.list[7],
								complete() {
								}
							})
						}
					},
					fail:(res)=>{
						this.$refs.uToast.show({
							...this.list[7],
							complete() {
							}
						})
					}
				});
			},
			changenum:function(e){
				this.replynum=e.detail.value.length;
			},
			changeimagecolor:function(){
				if(this.chooseimagecolor=="#c4c6c9"||this.uploadimages.length>0){
					this.chooseimagecolor="#8bc863";
				}
				else this.chooseimagecolor="#c4c6c9";
			},
			changeemojiselect(e){
				this.emojiselect=e.detail.current;
			
			},
			changeemojicolor(){
				if(this.chooseemojicolor=="#c4c6c9"){
					this.chooseemojicolor="#8bc863";
					this.chooseimagecolor="#c4c6c9";
				}
				else this.chooseemojicolor="#c4c6c9";
			},
			deleteimage:function(e){
				this.uploadimages.splice(e,1);
			},
			uploadimage:function(){
				let that=this;
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
								that.uploadimages.push((JSON.parse(res.data)).location);
							}
						})
					}
				});
			},
			linkmyself:function(){
				uni.navigateTo({
					url:"myself"
				})
			},
			weizhi(){
				var latitude=this.lal.split(";")[0];
				var longitude=this.lal.split(";")[1];
				uni.openLocation({
					latitude: parseFloat(latitude),
					longitude: parseFloat(longitude),
				});
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
			
			time1:function(e){
				var g = new Date().getTime()/1000;
				var time=parseInt(g)-parseInt(e);
				if(time<60)
					return parseInt(time)+"秒前";
				else if(time>=60&&time<60*60)
					return parseInt(time/60)+"分钟前";
				else if(time>=60*60&&time<60*60*24)
					return parseInt(time/60/60)+"小时前"
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
			setjing:function(){
				
				if(this.fangdou!=1)return;
				this.fangdou=0;
				uni.request({
					url:getApp().globalData.http+"/api/setjing",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"themeid":this.themeid
					},
					success: (res) => {
						if(res.data.code==1001){
						this.jing=1;
						this.$refs.uToast.show({
							...this.list[2],
							complete() {
								this.jing=1;
							}
						})}
						else if(res.data.code==1003)
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
							}
						})
						else
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					}
				})
				this.fangdou=1;
			},
			deletejing:function(){
				if(this.fangdou!=1)return;
				this.fangdou=0;
				uni.request({
					url:getApp().globalData.http+"/api/deletejing",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"themeid":this.themeid
					},
					success: (res) => {
						if(res.data.code==1001){
							this.jing=0;
						this.$refs.uToast.show({
							...this.list[2],
							complete() {
								this.jing=0;
							}
						})}
						else if(res.data.code==1003)
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
							}
						})
						else
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					}
				})
				this.fangdou=1;
		},
		

		
		settop:function(){
			if(this.fangdou!=1)return;
			this.fangdou=0;
			uni.request({
				url:getApp().globalData.http+"/api/settop",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie,
					"userid":getApp().globalData.userid,
					"themeid":this.themeid
				},
				success: (res) => {
					if(res.data.code==1001){
						this.top=1;
					this.$refs.uToast.show({
						...this.list[2],
						complete() {
							this.top=1;
						}
					})
					}
					else if(res.data.code==1003)
					this.$refs.uToast.show({
						...this.list[4],
						complete() {
						}
					})
					else
					this.$refs.uToast.show({
						...this.list[3],
						complete() {
						}
					})
				},
				fail: () => {
					this.$refs.uToast.show({
						...this.list[3],
						complete() {
						}
					})
				}
			})	
			this.fangdou=1;
		},
			deletetop:function(){
				if(this.fangdou!=1)return;
				this.fangdou=0;
				uni.request({
					url:getApp().globalData.http+"/api/deletetop",
					method:"POST",
					data:{
						"cookie":getApp().globalData.cookie,
						"userid":getApp().globalData.userid,
						"themeid":this.themeid
					},
					success: (res) => {
						if(res.data.code==1001){
							this.top=0;
						this.$refs.uToast.show({
							...this.list[2],
							complete() {
								this.top=0;
							}
						})}
						else if(res.data.code==1003)
						this.$refs.uToast.show({
							...this.list[4],
							complete() {
							}
						});
						else
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.list[3],
							complete() {
							}
						})
					}
				})
				this.fangdou=1;
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
			find:function(){
				uni.navigateTo({
					url:"find"
				})
			},
			linkto:function(){
				window.location.href=this.ycsrc;
			},
			showToast(params) {
				this.$refs.uToast.show({
					...params,
					complete() {
					params.url && uni.navigateTo({
						url: params.url
					})
					}
				})
			},
			delete_theme:function(){
				this.show=false;
				uni.request({
					url:getApp().globalData.http+"/api/deletetheme",
					method:"POST",
					data:{
						"themeid":this.themeid
					},
					success: (res) => {
						this.showToast(this.list[0]);
					},
					fail: (res) => {
						this.showToast(this.list[1]);
					}
				})
			},
			heself:function(){
				uni.navigateTo({
					url:"heself?userid="+this.userid
				})
			},
			heself1:function(e){
				uni.navigateTo({
					url:"heself?userid="+e
				})
			},
			back:function(){
				uni.navigateBack({
					
				})
			},
			setlike:function(e){
				if(getApp().globalData.cookie==''){
					this.$refs.uToast.show({
						...this.list[5],
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
							this.like=this.like+1;
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
									this.like=this.like-1;
									break;
								}
							}
							
						}
					}
				})
			},
			jugelike:function(res){

				for(var i=0;i<this.mylike.length;i++){
					if(this.mylike[i]==parseInt(res))return true;
				};return false
			},
			juge(){
				////console.log(getApp().globalData.userid==this.userid);
				if(getApp().globalData.userid==this.userid||getApp().globalData.admin==1)return true;else return false;
			},
			juge_admin(){
			
				if(getApp().globalData.admin==1)return true;else return false;
			},
			
		},
		onLoad(res) {
			uni.getStorage({
			    key:"admin",
			    success: function(res){
			        getApp().globalData.admin=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			this.backgroundimage=getApp().globalData.backgroundimage;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.op=getApp().globalData.op;
			this.themeid=parseInt(res.themeid);
			this.admin1=getApp().globalData.admin;
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
							getApp().globalData.mylike=res.data.mylike;
							this.mylike=res.data.mylike;
							
						}
					}
				})
			
			uni.request({
				url:getApp().globalData.http+"/api/getmysave",
				method:"POST",
				data:{
					"userid":getApp().globalData.userid
				},
				success: (res) => {
					this.savelist=res.data;
					for(var i=0;i<res.data.length;i++){
						if(this.themeid==res.data[i]){
							this.saveshow=1;
							break;
						}
					}
					
				}
			})
			}
			var id1=1;
			let id2=res.id;
			if(res.id!=null)id1=res.id;
			this.status="loading";
			let that=this;
			uni.request({
				url:getApp().globalData.http+"/api/getreply",
				method:"POST",
				data:{
					"themeid":this.themeid,
					"id":id1,
					"replytype":3
					
				},
				success: (res) => {
					//console.log(res.data);
					if(res.data.code==1001){
					
						this.replys=res.data.data;
						for(var i=0;i<this.replys.length;i++){;
							this.$set(this.replys[i],"sub",0);
							this.$set(this.replys[i],"sub1",0);
							this.$set(this.replys[i],"loucengreplydata",'');
						}
					if(res.data.data.length==0){
						this.status="nomore";
					}
					else if(res.data.data.length<10){
						this.replys[this.replys.length-1].id=
						this.replys[this.replys.length-1].id+1;
						this.status="loadmore"
					}
					else {
						this.getmore=1;
						this.status="loadmore"
					}
						if(id2!=null){
							setTimeout(() => {  
								this.scroll_top1='a'+id2
								
							}, 500)
						}
					}
					this.fangdou2=false;
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/tuijianlist",
				method:"POST",
				data:{
					"themeid":this.themeid
				},success: (res) => {
					for(var i=0;i<res.data.data.length;i++){
					res.data.data[i].data = res.data.data[i].data.replace(/&nbsp;/ig, '');
					res.data.data[i].data = res.data.data[i].data.replace(/&rdquo;/ig, '');
					res.data.data[i].data = res.data.data[i].data.replace(/&middot;/ig, '');
					res.data.data[i].data = res.data.data[i].data.replace(/&mdash;/ig, '');
					res.data.data[i].data = res.data.data[i].data.replace(/&ldquo;/ig, '');
					res.data.data[i].data = res.data.data[i].data.replace(/\s/ig, '');
					}
					this.tuijianlist=res.data.data;
				}
			})
			uni.request({
				url:getApp().globalData.http+"/api/gettheme",
				method:"POST",
				data:{
					"themeid":parseInt(res.themeid)
				},
				success: (res) => {
					this.jing=res.data.data.jing;
					this.top=res.data.data.top;
					this.type=res.data.data.type;
					this.ycsrc=res.data.data.ycsrc;
					this.article=res.data.data.text;
					this.title=res.data.data.title;
					this.changetime=res.data.data.changetime;
					this.num=res.data.data.num;
					if(res.data.data.position!=null&&res.data.data.position!=undefined){
						this.position=res.data.data.position;
						this.lal=res.data.data.lal;
					}
					if(this.title!='')
					uni.setNavigationBarTitle({
							title:this.title
					})
					else
					uni.setNavigationBarTitle({
							title:"主题"
					})
					if(this.tags.length>0){
						this.tags=res.data.tags.split(";");
						this.tags.pop();
					}
					this.userid=res.data.data.userid;
					this.fenlei=res.data.data.fenlei
					this.username1=res.data.data.username;
					this.look=res.data.data.look;
					this.userheadimage1=res.data.data.userheadimage;
					this.createtime=res.data.data.createtime;
					this.like=res.data.data.likes;
					this.num=res.data.data.num;
					this.sign=res.data.data.sign;
					this.admin=res.data.data.admin;
					this.shenfen=res.data.data.shenfen;
					////console.log(this.article);
				}
			})
		}
		
	}
</script>

<style>
.op{
	opacity: 0.95; 
}
.mp{
	user-select:text;
}
.color2{
	background-color: #f5f5f6;
	padding:2%
}
.color1{
	background-color: white;
	padding-left:2%;
	padding-right: 2%;
}
.selectemoji{
	background-color: #f6f7f8;
}
</style>
