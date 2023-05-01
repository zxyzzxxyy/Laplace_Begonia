<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<!--center-->
		
		<view style="display: flex;flex-direction: column;align-items: center;width:100vw;padding-top:min-height: 90vh;height:auto;background-size: cover;background-attachment: fixed;overflow-y: hidden;margin-top:10vh"  v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" >
			<view style=";background-color: white;width:80vw;;background-color: white;padding:10px;border-radius: 15px;"  >
			<view style="display: flex;flex-direction: row;"  :style="{'opacity':op}">
				<u--input
				   placeholder="标题"
				   border="bottom"
				   clearable
				   size="19"
				   v-model="title"
				style="width:90%;margin-bottom: 10px;"
				:style="{'opacity':op}"
				 ></u--input>
				 <u-button type="primary"  text="发布" style="width:6%;margin-left: 0.5%"
				 @click="fabu()"
				 ></u-button>
			</view>
			<view style="height:520px;min-height:520px;display:flex;flex-direction:row;margin-bottom: 10px;overflow-y:auto">
			<view
				v-if="type1"
				style="overflow-y:auto;width:49.5%;;border:1px solid #ccc;border-radius:0px;padding:10px;margin-right:0.5%;height:auto">
				
			<lRichTextEditor
				v-model="data"
				@change="GetChange"
				@text="con"
				style="margin-bottom: 10px;height:500px"
				>
			</lRichTextEditor>
			</view>
			<view 
			v-if="!type1"
			style="overflow-y:auto;width:49.5%;;border:1px solid #ccc;border-radius:0px;padding:10px;padding-bottom:0px;margin-right:0.5%;height:auto">
				<textarea
				v-model="text1"
				:maxlength="-1"
				 placeholder-style="color:#ccc"
				 placeholder="正文内容" style="height:400px;width:100%;border-bottom: 1px solid #ccc;"></textarea>
				<scroll-view style="height:100px;margin-top:7px;width:100%;" scroll-y>
				<view style="width:auto;height:auto;display: flex;flex-wrap: wrap;;flex-direction: row;">
				<image v-for="(item,index) in uploadimages"
				:src="item" style="	padding:0px;width:90px;height:90px;margin-bottom:7px;margin-left:00px;border-radius: 5px;margin-right:10px ;
					border-radius: 5px;" mode="aspectFit"
				@click="deleteimage(index)"
				>
					
				</image>
				<view
				@click="add_image()"
				style="
					background-color: #f6faff;border:1px dashed #ecf0f4;
					padding:10px;width:70px;height:70px;border-radius: 5px;margin-right:10px ;
					border-radius: 5px;margin-bottom:7px;
					display: flex;flex-direction: column;align-items: center;justify-content:center">
					<u-icon name="plus" color="#909399" size="28"></u-icon>
					<text style="color:#909399;font-size:14px">上传图片</text>
				</view>
				</view>
				</scroll-view>
			</view>
			<view style="overflow-y:auto;width:49.5%;;border:1px solid #ccc;border-radius:0px;padding:10px;margin-left:0.5%;height:auto">
			<mp-html
			:content="text" 
			:lazy-load="true"
			class="mp"
			v-if="type1"
			/>
			<mp-html
			:content="mode1()" 
			:lazy-load="true"
			class="mp"
			v-if="!type1"
			/>
			</view>
			</view>
			<view style="display: flex;flex-direction: row;" :style="{'opacity':op}">
				<u-tag v-for="(item,index) in tags" :text="item"  icon="tags-fill" style="width: auto;margin:10px" @click="delete_tag(item)" plain plainFill></u-tag>
				<view style="display: flex;flex-direction: row;align-items: center;" v-if="tags.length<5">
				<u--input
				   placeholder="添加标签"
				   border="bottom"
				   size="19"
				   v-model="tag"
					style="max-width: 5vw;margin-bottom: 10px;"
					
				 />
				 <u-button type="primary" :plain="true" text="添加" @click="add_tag()" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;"></u-button>
				</view>
				<view style="margin-left: auto;display: flex;flex-direction: row;justify-content: center;align-items: center;">
					<u--input
					   placeholder="原文链接(必填)"
					   border="bottom"
					   size="19"
					   v-model="ycsrc"
						style="max-width: 20vw;margin-bottom: 10px;"
						v-if="yuanchuang==false"
					 />
					 <u-tag type="primary" :plain="true" text="原创" @click="yuanchuang=true;type=0" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;" v-if="type==2"></u-tag>
					 <u-tag type="primary"  plainFill :plain="true" text="原创" @click="yuanchuang=false;type=1" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;" v-if="type==0"></u-tag>
					 <u-tag type="primary" :plain="true" text="搬运" @click="yuanchuang=true;type=2" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;" v-if="type==1"></u-tag>
				</view>
			</view>
			<view style="display: flex;flex-direction: row;" >
			<u--text prefixIcon="map-fill" iconStyle="color:#8bc863;margin-right:5px" v-if="position==''" type="primary" :plain="true" text="添加地点" color="#8bc863" @click="setposition" style="width:auto;margin:0;height:30px;"></u--text>
			<u--text prefixIcon="map-fill" iconStyle="color:#8bc863;margin-right:5px" v-if="position!=''" type="primary" :plain="true" :text="position" @click="setposition" style="width:auto;margin:0;height:30px;"></u--text>
			</view>
			<view style="display: flex;flex-direction: row;align-items: center;flex-wrap: wrap;">
					<view v-for="(item,index) in fenlei">
					<u-tag  :text="item"  icon="tags-fill" style="width: auto;margin:10px"
					 plain plainFill v-if="index==fenleisub"></u-tag>
					<u-tag  :text="item"  icon="tags-fill" style="width: auto;margin:10px" plain v-if="index!=fenleisub"  @click="fenleisub=index,fenlei1=item"></u-tag>
					</view>
					<u-tag  style="margin-left: auto;" @click="type1=!type1" text="切换编辑器"></u-tag>
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
				type1:false,
				text1:"",
				imagelist:[],
				op:0.95,
				yuanchuang:true,
				fenleisub:0,
				ycsrc:"",
				icon:"/static/icon.png",
				userbackgroundimage:"",
				userheadimage:"",
				src:"/static/aa.jpg",
				data:"",
				username:"",
				text:"",
				position:"",
				lal:"",
				image:[],
				admin:0,
				title:"",
				tags:[],
				uploadimages:[
				
				],
				key:"",
				tag:"",
				fenlei:[],
				fenlei1:"默认分类",
				type:2,
				sub:0,
				replymessageshow:false,
				upload:[
					{
						type: 'error',
						title: "失败",
						message:"其它设备登录",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png'
					}
				]
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
			if(getApp().globalData.cookie=='')
			{
				uni.reLaunch({
					url:"index"
				})
				return;
			}
			else
			uni.request({
				url:getApp().globalData.http+"/api/jugeuser",
				method:"POST",
				data:{
					"cookie":getApp().globalData.cookie
				},
				success: (res) => {
					if(res.data.code==1002){
						this.$refs.uToast.show({
							...this.upload[0],
							complete() {
								that.loginout();
							}
						})

					}
				}
			})
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			this.admin=getApp().globalData.admin;
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					////console.log("admin:"+getApp().globalData.admin);
					this.op=res.data;
				}
			 });
			this.op=getApp().globalData.op;
			this.fenlei=[];
			uni.request({
				url:getApp().globalData.http+"/api/getfenleilist",
				method:"POST",
				data:{
				},
				success: (res) => {
					for(var i=0;i<res.data.length;i++)
					this.fenlei.push(res.data[i]);
				}
			})
		},
		methods: {
			setposition(){
				let that=this;
				uni.chooseLocation({
					success: function (res) {
						console.log(res);
						that.position=res.address;
						that.lal=res.latitude+";"+res.longitude
					}
				});
			},
			deleteimage(i){
				this.uploadimages.splice(i,1);
			},
			mode1(){
				var str="";
				str+=this.text1;
				for(var i=0;i<this.uploadimages.length;i++){
					str+="<img src='"+this.uploadimages[i]+"' style='width:1920px;height:auto;border-radius:5px;margin-top:10px' mode='aspectFit' />"
				}

				return str;
			},
			add_image(){
				let _this=this;
				uni.chooseImage({
					count: 1,
					sizeType: [ 'compressed'],
					sourceType: ['album'],
					success: function(res) {
						//console.log(res.tempFilePaths);
						uni.getImageInfo({
							src:res.tempFilePaths[0],
							success:(res)=>{
								var temp={
									width:res.width,
									height:res.height
								}
								_this.hw.push(temp);
							}
						})
						uni.uploadFile({
						    url:"https://www.txtz.club:8808/api/upload",		//post请求的地址
						    filePath:res.tempFilePaths[0],
						    name:'file',	
						    success: (res) => {
				
								res.data = JSON.parse(res.data)
								var location = res.data.location;
								_this.uploadimages.push(location);
								
							}
						})
					}
				});
				
			},
			change_fenleisub:function(e){
			
				this.fenleisub=e;
			},

			delete_tag:function(e){
				this.tags.splice(this.tags.indexOf(e),1);
			},
			add_tag:function(){
				if(this.tag.length>0)
				this.tags.push(this.tag);
				this.tag=""
			},
			GetChange:function(e){
				e=e.substr(5,e.length-11);
				this.image.push(e);
				////console.log(e);
				////console.log(this.getImgSrc(e));
				
			},
			con:function(e){
				this.text=e;
				////console.log(this.text);
			},

			fabu:function(){
				////console.log(this.text);
				////console.log(temp);
				
				if(this.sub==1)return;this.sub=1;
				if(this.type1==false)this.text=this.mode1();
				if(this.type==1&&this.ycsrc==''){
					uni.showToast({
						title:"原文链接不能为空",
						icon:"error"
					})
					this.sub=0;
					return;
				}
				if(this.title.length==0){
					uni.showToast({
						title:"标题不能为空",
						icon:"error"
					})
					this.sub=0;
					return;
				}
				else {
					var juge=0;
					for(var i=0;i<this.title.length;i++){
						if(this.title[i]!=' '){
							juge=1;
							break;
						}
					}
					if(juge==0){
						uni.showToast({
							title:"标题不能为空",
							icon:"error"
						})
						this.sub=0;
						return;
					}
				}var msg = ""+this.text;
				while(msg.indexOf("<pre")!=-1){
					
					var i=msg.indexOf("<pre");
					var j=msg.indexOf("</pre>");
					msg=msg.substring(0,i)+"[代码块]"+msg.substring(j+6,msg.length);
				}
				//console.log(msg);
				
				
				var re2 = new RegExp("\n","g");//匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
				msg = msg.replace(re2,'');//执行替换成空字符

				var re1 = new RegExp("<.+?>","g");//匹配html标签的正则表达式，"g"是搜索匹配多个符合的内容
				msg = msg.replace(re1,'');//执行替换成空字符
				
				msg=msg.toString();
				
				
				
				////console.log(temp);
				const regExp = /<img[^>]+src=['"]([^'"]+)['"]+/g;
				const images = [];
				let temp;
				while ((temp = regExp.exec(this.text)) != null) {
					images.push( temp[1]);
				}
				for(var i=0;i<images.length;i++){
					var sub=this.text.indexOf(images[i]);
				
					this.text=this.text.substring(0,sub+images[i].length)+"?imageMogr2/format/tgp"+this.text.substring(sub+images[i].length,this.text.length);
				}
				var g = new Date().getTime();
				var images1="";
				for(var i=0;i<images.length;i++){
					images1=images1+images[i]+";";
				}
				msg=msg.replace(/[\r\n]/g,"");
				if(msg.length==0&&images.length==0){
					this.sub=0;
					uni.showToast({
						title:"内容不能为空",
						icon:"error"
					})
					return;
				}
				var g = new Date().getTime();
				var tag="";
				for(var i=0;i<this.tags.length;i++){
					if(this.tags[i].length==0)
						continue;
					tag+=this.tags[i]+";";
				}
				while(true){
						var subStr="http://player.bilibili.com";
						var index=this.text.lastIndexOf(subStr);
						if(index!=-1){
							this.text=this.text.substr(0,index)+this.text.substr(index+subStr.length,this.text.length);
		
					}else{
							break;
					}
				}
				uni.request({
					url:getApp().globalData.http+"/api/newtheme",
					method:"POST",
					data:{
						"title":this.title,
						"text":this.text,
						"image":images1,
						"data":msg,
						"userid":getApp().globalData.userid,
						"cookie":getApp().globalData.cookie,
						"createtime":parseInt(g/1000),
						"tags":tag,
						"type":this.type,
						"ycsrc":this.ycsrc,
						"fenlei":this.fenlei1,
						"position":this.position,
						"lal":this.lal
					},
					success: (res) => {
					//	//console.log(res);
						if(res.data.code==1001)
						uni.showToast({
							title:"发布成功",
							icon:"success"
						})
						else if(res.data.code==1002){
							uni.showToast({
								title:"账号禁用中",
								icon:"error"
							})
						}
						setTimeout(() => {  
							uni.navigateBack({
								
							})}
							, 1000)
					},
					fail: (res) => {
						//console.log(res);
						uni.showToast({
							title:"发布失败",
							icon:"error"
						})
						this.sub=0;
					}
				})
			},

		
		}
	}
</script>

<style>
.op{
	opacity: 0.95; 
}
</style>



