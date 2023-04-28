<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
		<!--顶部栏-->
		<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<!--center-->
		
		<view style="display: flex;flex-direction: column;align-items: center;width:100vw;padding-top: 8vh;height:auto;min-height: 90vh;background-size: cover;background-attachment: fixed;overflow-y: hidden;"  v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" >
			<view style=";background-color: white;width:70vw;;background-color: white;padding:10px;border-radius: 15px;"  :style="{'opacity':op}">
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
				 <u-button type="primary"  text="发布" style="width:9%;margin-left: 1%;"
				 @click="fabu()"
				 ></u-button>
			</view>
			<lRichTextEditor
				v-if="data1!=''"
				v-model="data"
				@change="GetChange"
				@text="con"
				:values="data1"
				style="width:100%;margin-bottom: 10px;"
				:style="{'opacity':op}"
				>
			</lRichTextEditor>
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
					 <u-button type="primary" :plain="true" text="原创" @click="yuanchuang=false;type=1" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;" v-if="yuanchuang==true"></u-button>
					 <u-button type="primary" :plain="true" text="搬运" @click="yuanchuang=true;type=0" style="width:50px;margin-right: auto;margin:0;margin-left: 5px;height:30px;" v-if="yuanchuang==false"></u-button>
				</view>
			</view>
				<view style="display: flex;flex-direction: row;align-items: center;flex-wrap: wrap;">
					<view v-for="(item,index) in fenlei">
					<u-tag  :text="item"  icon="tags-fill" style="width: auto;margin:10px"
					 plain plainFill v-if="index==fenleisub"></u-tag>
					<u-tag  :text="item"  icon="tags-fill" style="width: auto;margin:10px" plain v-if="index!=fenleisub"  @click="fenleisub=index,fenlei1=item"></u-tag>
					</view>
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
				op:0.95,
				yuanchuang:true,
				fenleisub:0,
				ycsrc:"",
				icon:"/static/icon.png",
				userbackgroundimage:"",
				userheadimage:"",
				src:"/static/aa.jpg",
				data:"",
				data1:"",
				username:"",
				text:"",
				image:[],
				admin:0,
				title:"",
				tags:[],
				key:"",
				tag:"",
				tags:[],
				fenlei:[],
				fenlei1:"默认分类",
				type:0,
				sub:0,
				replymessageshow:false,
				themecookie:"",
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
		onLoad(option) {
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
			uni.request({
				url:getApp().globalData.http+"/api/gettheme",
				method:"POST",
				data:{
					"themeid":parseInt(option.themeid)
				},
				success: (res) => {
				
					this.type=res.data.data.type;
					this.ycsrc=res.data.data.ycsrc;
					this.data1=res.data.data.text;
					this.text=res.data.data.text;
					this.title=res.data.data.title;
					this.themecookie=res.data.data.themecookie;
					
					if(this.title!='')
					uni.setNavigationBarTitle({
							title:this.title
					})
					else
					uni.setNavigationBarTitle({
							title:"主题"
					})
					if(res.data.tags.length>0){
						this.tags=res.data.data.tags.split(";");
						this.tags.pop();
					}
					for(var i=0;i<this.fenlei.length;i++){
					
					if(this.fenlei[i]==res.data.data.fenlei){
						
						this.fenleisub=i;
						break;
					}
					}
				}
			})
		},
		methods: {
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
				if(this.type==1&&this.ycsrc==''){
					uni.showToast({
						title:"原文链接不能为空",
						icon:"error"
					})
					return;
				}
				
				var msg = ""+this.text;
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
				
				
				////console.log(this.text);
				
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
					url:getApp().globalData.http+"/api/changetheme",
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
						"fenlei":this.fenlei[this.fenleisub],
						"themecookie":this.themecookie
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



