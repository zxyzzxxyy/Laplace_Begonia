<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;
	display: flex;align-items: center;justify-content: center;
	" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="myusername" :userheadimage="myuserheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<view style="width:60vw;height:80vh;background-color: white;border-radius: 10px;overflow: hidden;position: relative;">
		
		<view style="width:100%;height:7%;background-color: white;display: flex;align-items: center;justify-content: center;">
			<u-icon size="25" name="arrow-left" @click="back"></u-icon>
			<view  style="width:100%;background-color: white;display: flex;align-items: center;justify-content: center;margin-left: -25px;">
			<text style="font-size: 18px;font-weight: 700;">{{username}}</text>
			</view>
		</view>
		<scroll-view style="z-index:;;height:95%;width:100%;background-color: #efefef;position:absolute" scroll-y="true" :scroll-top="top" :scroll-with-animation="show1" :scroll-into-view="bottom">
			<view v-for="(item,index) in chatlist" :class="{'a1':show==0,'a2':show==1}" style="margin-bottom: px;">
				<view style="width:100%;display: flex;align-items: center;font-size: 12px;color:#909399;justify-content: center;margin-top: 5px;" v-if="juge1(index)">{{time1(item.time)}}</view>
				<view v-if="juge(item)" style="width:98%;margin-right: 2vw;margin-top: 5px;margin-bottom: 5px;display: flex;flex-direction: row; align-items: flex-start">
				
					<view style="width:auto;border-radius: 5px;background-color: #99dc6d;overflow: hidden;max-width: 50vw;padding:7px;padding-left:10px ;margin-left: auto;margin-top: 10px;">
						<text style="border-radius: 5px;font-size: 16px;color:#ffffff;">{{item.message}}</text>
						<view v-if="jugeimage(item.image)" style="margin-top: 5px;">
							<image :src="item.image" style="min-width: 50px;height:auto;max-width: 100%;border-radius: 5px;"  mode="widthFix"></image>
						</view>
					</view>
				<view style="width:50px;margin-left: 8px;">
					<u-avatar mode="aspectFill":src="myuserheadimage" v-if="myuserheadimage!=null" size="45"></u-avatar>
					<u-avatar :text="myuserheadimage.substr(0,1)"  randomBgColor v-if="myuserheadimage==null" size="45"></u-avatar>
		
				</view>
				</view>
				<view v-if="!juge(item)" style="width:98%;margin-right: 2vw;margin-top: 5px;margin-bottom: 5px;display: flex;flex-direction: row; align-items: flex-start">
				
				<view style="width:50px;margin-left: 8px;">
					<u-avatar mode="aspectFill":src="userheadimage" v-if="userheadimage!=null" size="45"></u-avatar>
					<u-avatar :text="username.substr(0,1)"  randomBgColor v-if="userheadimage==null" size="45"></u-avatar>
				
				</view>
				<view style="width:auto;border-radius: 5px;background-color: #ffffff;overflow: hidden;max-width: 50vw;padding:7px;padding-right:10px ;margin-top: 10px;">
					<text style="border-radius: 5px;font-size: 16px;color:#000000;">{{item.message}}</text>
					<view v-if="jugeimage(item.image)" style="margin-top: 5px;">
						<image :src="item.image" style="min-width: 50px;height:auto;max-width: 100%;border-radius: 5px;"  mode="widthFix"></image>
					</view>
				</view>
				</view>
			</view>
			<view style="padding-top:70px;;width:100%;height:1px;opacity: 0;" id="bottom"></view>
		</scroll-view>
			<view style="width:98%;padding-left:2%;bottom:0;height:6.5%;display: flex;flex-direction: row;align-items: center;justify-content: center;z-index: 2;position:absolute;background-color:white">
				<view style="width:75%;border:1px solid #d0d5dd;padding-top:5px;padding-bottom:5px;padding-left:10px;padding-right:10px;border-radius:100px">
				<input
				   placeholder="请输入内容"
				   border="surround"
				   v-model="message"
				   @confirm="send()"
				   @blur="again"
				   ref="input"
				   :focus="f"	
					style="font-size:16px;background-color: white;width:75%;margin-left:5px;"
				 ></input>
				 </view>
				 <u-button type="primary" text="发送" style="width:15%;border-radius: 100px;" @click="send()"></u-button>
				 <view style="margin-right: 1%;"  v-if="image!=''" @click="image=''">
				 <u-icon name="photo" color="#2979ff" size="28"></u-icon>
				 </view>
				 <view style="margin-right: 1%;" v-if="image==''" @click="head()">
				 <u-icon name="photo" size="28"></u-icon>
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
				userid:0,
				username:"",
				userheadimage:"",
				userbackgroundimage:"",
				myuserheadimage:"",
				myusername:"",
				admin:0,
				replymessageshow:false,
				chatlist:[],
				message:"",
				image:"",
				show:0,
				bottom:"",
				top:999999999999999,
				show1:false,
				f:true,
				sub1:0,
			}
		},
		onLoad() {
			if(getApp().globalData.cookie==''){
				uni.reLaunch({
					url:"index"
				})
			}
			uni.getStorage({
			    key:"chat_userid",
			    success: function(res){
			        getApp().globalData.chat_userid=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			 uni.getStorage({
			     key:"chat_username",
			     success: function(res){
			         getApp().globalData.chat_username=res.data;
			 		//console.log("admin:"+getApp().globalData.admin);
			 	}
			  });
			  uni.getStorage({
			      key:"chat_userheadimage",
			      success: function(res){
			          getApp().globalData.chat_userheadimage=res.data;
			  		//console.log("admin:"+getApp().globalData.admin);
			  	}
			   });
			
			this.replymessageshow=getApp().globalData.replymessageshow;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.myuserheadimage=getApp().globalData.userheadimage;
			this.myusername=getApp().globalData.username;
			this.admin=getApp().globalData.admin;
			this.userid=getApp().globalData.chat_userid;
			this.username=getApp().globalData.chat_username;
			this.userheadimage=getApp().globalData.chat_userheadimage;
			uni.setNavigationBarTitle({
					title:this.username
			})
			for(var i=0;i<getApp().globalData.chatlist.length;i++){
				if(getApp().globalData.chatlist[i].a==this.userid||getApp().globalData.chatlist[i].b==this.userid)
				this.chatlist.push(getApp().globalData.chatlist[i]);
			}
			this.sub=getApp().globalData.chatlist.length-1;
			
			setTimeout( () => {
				this.top=Math.round(Math.random()*500000)+99999999;
			}, 5)			
				
			this.timer = setInterval( () => {
				this.show=1;
				this.show1=true;
				this.add();// 业务逻辑	
			}, 50)
		},
		methods: {
			again(){
				this.f=false;
				setTimeout( () => {
					this.f=true;
				}, 5)
			},
			jugeimage:function(e){
			
				if(e.length==0)return false;
				return true;
			},
			juge1:function(i){
				if(i==0)return true;
				if(this.chatlist[i].time-this.chatlist[i-1].time>60)return true;return false;
			},
			add:function(){
				if(this.sub!=getApp().globalData.chatlist.length-1){
					for(var i=this.sub+1;i<getApp().globalData.chatlist.length;i++){
						
						var temp=JSON.parse(JSON.stringify(getApp().globalData.chatlist[i]));
						if(temp.b==this.userid||temp.a==this.userid)
						this.chatlist.push(temp);
						
					}
					this.sub=getApp().globalData.chatlist.length-1;
					
					
					setTimeout( () => {
						
						this.top=Math.round(Math.random()*500000)+99999999;
						//this.bottom="bottom";
						//this.bottom="";
					}, 50)
				}
				for(this.sub1;this.sub1<getApp().globalData.chatlistback.length;this.sub1++){
					for(var i=0;i<this.chatlist.length;i++){
						console.log(getApp().globalData.chatlistback[this.sub1]);
						if(this.chatlist[i].id==getApp().globalData.chatlistback[this.sub1]){
						
							this.chatlist.splice(i,1);
							break;
						}
					}
				}
			},
				
			back:function(){
				uni.navigateBack({
					
				})
			},
			juge:function(res){
				if(res.a==getApp().globalData.userid)return true;return false;
			},
			send(){
				if(this.message==''&&this.image=='')return;
				(getApp().send(getApp().globalData.userid,this.userid,this.message,this.image,""));
				this.message="";
				this.image="";
				
				//this.$refs.input.focus();  
			},
			head:function(){
				let that = this;
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
									that.image=res.data.location;
			
							}
						})
					}
				});
			},
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
					var h = e.getHours();
					var m = e.getMinutes();
					if(m<10)
					m="0"+m;
					if(year!=year1)
						return year1+"-"+month1+"-"+date1+" "+h+":"+m;
					else return month1+"-"+date1+" "+h+":"+m;
				}
			},
		}
	}
</script>

<style>
.a1{
	opacity: 0;
}
.a2{
	opacity: 1;
}
</style>
