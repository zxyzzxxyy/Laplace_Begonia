<template>
	<view style="width: 100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}">
		<titles style="width:100vw;" :username="username" :userheadimage="userheadimage" :admin="admin" ></titles>
		<!--*************************-->
		<view style="display: flex;flex-direction: row;display: flex;justify-content: center;padding-top: 10vh;">
			<view style="background-color: white;border-radius: 10px;width:50vw;height:auto;min-height: 10vh;overflow-y: auto;padding:20px">
				<view style="font-size: 20px;font-weight: 700" @click="getreplymessage()">
					<text>消息通知</text>
				</view>
				<u-divider ></u-divider>
				<view style="display: flex;flex-direction: row;">
					<view style="margin-left: 10px;" v-if="choose!=0" @click="choose=0">
						<text>回复我的</text>
					</view>
					<view style="display: flex;flex-direction: column;margin-left: 10px;" v-if="choose==0">
						<text style="color:#8bc863">回复我的</text>
						<view style="background-color:#8bc863;width:auto;height:2px;border-radius: 1px;margin-top: 5px;"></view>
					</view>
					<view style="margin-left: 30px;" v-if="choose!=1" @click="choose=1">
						<text>系统消息</text>
					</view>
					<view style="display: flex;flex-direction: column;margin-left: 30px;" v-if="choose==1">
						<text style="color:#8bc863">系统消息</text>
						<view style="background-color:#8bc863;width:auto;height:2px;border-radius: 1px;margin-top: 5px;"></view>
					</view>
					<view style="margin-left: 30px;" v-if="choose!=2" @click="choose=2">
						<text>私信</text>
					</view>
					<view style="display: flex;flex-direction: column;margin-left: 30px;" v-if="choose==2">
						<text style="color:#8bc863">私信</text>
						<view style="background-color:#8bc863;width:auto;height:2px;border-radius: 1px;margin-top: 5px;"></view>
					</view>
					<!--
					<view style="margin-left: 10px;" v-if="choose!=1"  @click="choose=1">
						<text>点赞我的</text>
					</view>
					<view style=";display: flex;flex-direction: column;margin-left: 10px;" v-if="choose==1">
						<text style="color:#8bc863">点赞我的</text>
						<view style="background-color:#8bc863;width:auto;height:2px;border-radius: 1px;margin-top: 5px;"></view>
					</view>
					-->
				</view>
				<scroll-view style="margin-top:20px;max-height:70vh;height:70vh" :scroll-y="true" @scrolltolower="getmore()" v-if="choose==0">
					<view v-for="(item,index) in themelist" style="margin-bottom: 15px;" @click="theme(item.themeid,item.id)">
						<view style="display: flex;flex-direction:row;margin-right: 5px;align-items: center;">
							<u-avatar mode="aspectFill":src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)"  randomBgColor v-if="item.userheadimage==null" size="45"></u-avatar>
							<view style="display: flex;flex-direction:column;margin-left:8px">
								<text>{{item.username}}</text>
								<text style="font-size: 14px;color:#909399;">{{time(item.time)}} 回复了我</text>
							</view>
						</view>
						<view style="width:90%;height:auto;background-color:#f4f4f5;margin-top:20px;border-radius:10px;padding:1vh;padding-top:20px">
							<u--text :lines="2" :text="item.louceng+'楼:'+item.data" size="16"></u--text>
							<text>{{image1(item.image)}}</text>
							<view style="width:80%;height:auto;background-color:white;border-radius:5px;padding:15px;margin-top:10px;display:flex;flex-direction:row;align-self: center;">
								<view v-if="item.type==1&&item.themeimage.length>0" style="margin-right: 10px;">
									<image :src="image(item.themeimage)" style="width:80px;height:80px;border-radius:5px"  mode="aspectFill"></image>
								</view>
								<u--text v-if="item.type==1" :lines="2" :text="username+':'+item.title+'\n'+item.themedata+image1(item.themeimage)" size="14"></u--text>
								<u--text v-if="item.type==2" :lines="2" :text="username+' '+item.louceng+'楼:'+item.title+'\n'+item.themedata+image1(item.themeimage)" size="14"></u--text>
								</view>
						</view>
					</view>
					<u-loadmore
							style="margin-top: 30px;"
					        :status="status" 
					        loading-text="努力加载中" 
					        loadmore-text="轻轻上拉" 
					        nomore-text="实在没有了" 
					    />
				</scroll-view>
				<scroll-view v-if="choose==1" style="margin-top:20px;max-height:70vh;height:70vh" :scroll-y="true"
				@scrolltolower="getmy_systemmessage()"
				>
					<view v-for="(item,index) in systemmessage" style="margin-bottom: 15px;" >
						<view style="display: flex;flex-direction:row;align-items: center;">
							<u-avatar mode="aspectFill":src="item.systemheadimage"size="45"></u-avatar>
							
							<view style="display: flex;flex-direction:column;margin-left:8px">
								<text>Laplace</text>
								<text style="font-size: 14px;color:#909399;">{{time(item.createtime)}}</text>
							</view>
						</view>
						<view style="width:90%;height:auto;background-color:#f4f4f5;margin-top:20px;border-radius:10px;padding:1vh;padding-top:15px">
							<u--text :text="item.message" size="16"></u--text>
							<u--text :lines="1" text="地址链接" size="16" color="#8bc863" style="margin-top: 5px;" @click="linkto1(item.src)" v-if="item.src!=''"></u--text>
							<u--image :src="item.images" style="margin-right:1rem;margin-top: 5px;"
							height="10rem"
							radius="5px"
							v-if="item.images!=''"></u--image>
						</view>
					</view>
					<u-loadmore
							style="margin-top: 30px;"
					        status="nomore" 
					        loading-text="努力加载中" 
					        loadmore-text="轻轻上拉" 
					        nomore-text="实在没有了" 
					    />
				</scroll-view>
				<scroll-view style="margin-top:20px;max-height:70vh;background-color: white;width:100%;height:70vh;" :scroll-y="true" @scrolltolower="getmore()"
				v-if="choose==2"
				>
					<view v-for="(item,index) in chat" @click="tochat(item)">
						<view style="display: flex;flex-direction:row;align-items: center;width:100%">
							<u-avatar mode="aspectFill":src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)"  randomBgColor v-if="item.userheadimage==null" size="45"></u-avatar>
							<view style="display: flex;flex-direction: column;width:100%">
							<view style="display: flex;flex-direction:row;margin-left:8px;align-items: center;">
								<text style="font-size: 16px;">{{item.username}}</text>
								<view style="margin-left:auto">
								<text style="font-size: 14px;color:#909399;">{{time1(item.time)}} </text>
								</view>
							</view>
							<view style="display: flex;flex-direction:row;margin-left:8px;align-items: center;color:#909399">
								<u--text :lines="1" color="#909399" :text="item.message" ></u--text>
							</view>
							</view>
						</view>
						<u-divider ></u-divider>
					</view>
					
					<u-loadmore
							style="margin-top: 30px;"
					        status="nomore" 
					        loading-text="努力加载中" 
					        loadmore-text="轻轻上拉" 
					        nomore-text="实在没有了" 
					    />
				
					
				</scroll-view>
			</view>
			<!---->
			<view style="background-color: white;border-radius: 10px;width:15vw;height:10vh;min-height: 10vh;margin-left: 5vw;padding:20px">
				<text style="font-size: 20px;font-weight: 700;">消息通知</text>
				<u-divider text="分割线" :dot="true" style="width:95%;margin: 0 auto;margin-top: 10px;margin-bottom: 10px;"></u-divider>
				<view style="display: flex;flex-direction: row;align-items: center;margin-top: 10px;font-size: 18px;color:#8bc863" >
					<u-icon name="chat" size="33" style="margin-right: 5px;" color="#8bc863"></u-icon><text>全部消息</text>
				</view>
			</view>
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
				status:"loadmore",
				username:"",
				userheadimage:"",
				admin:0,
				userbackgroundimage:"",
				choose:0,
				themelist:[],
				juge:1,
				limit:0,
				fangdou1:0,
				page:0,
				systemmessage:[
					
				]
			}
		},
		onLoad() {
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.admin=getApp().globalData.admin;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			
			this.getmore();
			getApp().globalData.replymessageshow=false;
			this.timer = setInterval( () => {
				this.getmychat();// 业务逻辑	
			}, 500)
			uni.setStorage({
			    key: "replymessageshow",
			    data: getApp().globalData.replymessageshow,
			    success: function () {
			    }
			});
		},
		onShow(){
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.admin=getApp().globalData.admin;
			//this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.getmy_systemmessage();
			getApp().globalData.replymessagenum=0;
			if(this.themelist.length==0)this.status="nomore";
		},
		methods: {
			time1:function(e){
				var g = new Date();
				var e1 = new Date(e*1000);
				var data=g.getDate();
				var data1=e1.getDate();
				if(data==data1){
					let date = new Date(e*1000);
					let h = date.getHours();
					let m = date.getMinutes();
					if(m<10)
					m="0"+m;
					return h+":"+m
				}
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
				}},
			tochat:function(res){
				getApp().globalData.chat_userid=res.userid;
				getApp().globalData.chat_username=res.username;
				getApp().globalData.chat_userheadimage=res.userheadimage
				uni.setStorage({
				    key: "chat_userid",
				    data: res.userid,
				    success: function () {
				    }
				});
				uni.setStorage({
				    key: "chat_username",
				    data: res.username,
				    success: function () {
				    }
				});
				uni.setStorage({
				    key: "chat_userheadimage",
				    data:res.userheadimage,
				    success: function () {
				    }
				});
				var url="chat";
				uni.navigateTo({
					url:url
				})
			},
			getmychat(){
				
				for(var i=0;i<getApp().globalData.chat.length-1;i++){
					for(var j=i+1;j<getApp().globalData.chat.length;j++)
					if(getApp().globalData.chat[i].time<getApp().globalData.chat[j].time){
						var temp=getApp().globalData.chat[i];
						getApp().globalData.chat[i]=getApp().globalData.chat[j];
						getApp().globalData.chat[j]=temp;
					}
				}
				this.chat=getApp().globalData.chat;
			},
			getmy_systemmessage:function(){
				if(this.fangdou1==1)return this.fangdou1=1;
				uni.request({
					url:getApp().globalData.http+"/api/getmy_systemmessage",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						cookie:getApp().globalData.cookie,
						page:this.page
					},
					success:(res)=>{
						for(var i=0;i<res.data.length;i++)
							this.systemmessage.push(res.data[i]);
						if(res.data.length<5){
							this.fangdou1=1;
							this.page=-1;
						}else {
							this.page++;this.fangdou1=0;
						}
					},
					fail:(res)=>{
						
					},
				})
			},
			choose2:function(){
				
			},
			linkto1:function(e){
				window.location.href=e;
			},
			getmore:function(){
				if(this.juge==0)return;
				this.juge=0;
				this.status="loading";
				uni.request({
					url:getApp().globalData.http+"/api/getreplymessage",
					method:"POST",
					data:{
						"limit":this.limit,
						"userid":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie
					},
					success:(res)=>{
						this.juge=1;
						if(res.data.reply.length==0||res.data.reply.length<10
						){
							this.juge=0;
							this.status="nomore";
							
						}
						for(var i=0;i<res.data.reply.length;i++){
							var msg = res.data.reply[i].data;
							while(msg.indexOf("<pre")!=-1){
								var x=msg.indexOf("<pre");
								var y=msg.indexOf("</pre>");
								msg=msg.substring(0,x)+"[代码块]"+msg.substring(y+6,msg.length);
							}
							var re2 = new RegExp("\n","g");//匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
							msg = msg.replace(re2,'');//执行替换成空字符
							var re1 = new RegExp("<.+?>","g");//匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
							msg = msg.replace(re1,'');//执行替换成空字符
							res.data.reply[i].data=msg.toString();
							res.data.reply[i].text='';
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/&nbsp;/ig, '');
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/&rdquo;/ig, '');
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/&middot;/ig, '');
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/&mdash;/ig, '');
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/&ldquo;/ig, '');
							res.data.reply[i].themedata = res.data.reply[i].themedata.replace(/\s/ig, '');
							getApp().globalData.replymessage.push(res.data.reply[i]);
							this.themelist.push(res.data.reply[i]);
						}
						this.limit++;
						if(this.themelist.length>0){
							getApp().globalData.replymessageid=this.themelist[0].id;
							uni.setStorage({
								key: "replymessageid",
								data: getApp().globalData.replymessageid,
								success: function () {
								}
							});
						}
					}
				})
			},
			theme:function(e,i){
				uni.navigateTo({
					url:"./theme?themeid="+e+"&id="+i
				})
			},
			image1:function(e){
				if(e==null||e==undefined||e.length==0)return '';
				var arr= e.split(";")
				arr.pop();
				var str="";
				for(var i=0;i<arr.length;i++)
					str+="[图片]";
				return str;
			},
			image:function(e){
				return e.split(";")[0];
			},
			time:function(e){
				var g = new Date().getTime()/1000;
				var time=parseInt(g)-parseInt(e);
				if(time<60)
					return ""+parseInt(time)+"秒前";
				else if(time>=60&&time<60*60)
					return ""+parseInt(time/60)+"分钟前";
				else if(time>=60*60&&time<60*60*24)
					return ""+parseInt(time/60/60)+"小时前"
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
		}
	}
</script>

<style>

</style>
