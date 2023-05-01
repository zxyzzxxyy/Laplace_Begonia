<template>
	<view style="background-color: #f8f8f8;;;width:100vw;height:100vh;background-size: cover;background-attachment: fixed;overflow: hidden;
	display: flex;align-items: center;justify-content: center;
	" v-bind:style="{backgroundImage:'url(' + userbackgroundimage + ')'}" id="body">
	<titles style="width:100vw;height:auto;min-height: 50px;" :username="username" :userheadimage="userheadimage" :admin="admin" :replymessageshow="replymessageshow"></titles>
		<view style="height:70vh;width:70vw;background-color: white;border-radius: 15px;justify-content: center;display: flex;align-items: center;">
			<view style="width:70%">
			<view style="display: flex;flex-direction: row;">
			  <u--input
			    placeholder="URL"
			    border="surround"
			    v-model="url1"
				style="border-radius: 5px 0 0 5px;width:90%"
			  ></u--input>
			  <view style="width:10%;height:auto;color:white;background-color: #8bc863;display: flex;align-items: center;justify-content: center;border-radius:0  5px 5px 0;" @click="send()">
				  发送
			  </view>
			</view>
			  <u-checkbox-group
			              v-model="checkboxValue1"
			              placement="row"
			          >
			              <u-checkbox
			  			style="margin-right: 0.5rem;margin-top: 0.5rem;"
			                  :customStyle="{marginBottom: '8px'}"
			                  v-for="(item, index) in checkboxList1"
			                  :key="index"
			                  :label="item.name"
			                  :name="item.name"
			                  :checked="item.checked"
			  				@change="checkboxChange(index)"
			              >
			              </u-checkbox>
			          </u-checkbox-group>
				<view style="margin-top: 20px;margin-bottom: 20px;">
				<text style="font-size: 20px;font-weight: 800;">Request</text>
				</view>
				<view style="display: flex;flex-direction: row;">
					<view style="width: 48%;height:50%;margin-right: 4%;">
						<textarea height="250"
						  placeholder="data"
						  
						  v-model="data"
						  style="border-radius: 5px;border: #e3e8f0 solid 1px;padding:1%;width:98%"
					
						></textarea >
					</view>
					<view style="width: 48%;">
						<textarea height="250"
						  placeholder="header"
						
						  v-model="header"
						  style="border-radius: 5px;border: #e3e8f0 solid 1px;padding:1%;width:98%"
											
						></textarea >
					</view>
				</view>
				<view style="margin-top: 20px;margin-bottom: 20px;display: flex;flex-direction: row;align-items: center;;">
				<text style="font-size: 20px;font-weight: 800;">Response</text>
				<view v-if="loading" style="margin-left: auto;">
						<u-loading-icon></u-loading-icon>
					</view>
				<view style="margin-left: auto;padding: 2px 4px 2px 4px;background-color: #349c4c;border-radius: 2px;" v-if="code==200&&loading==false">
					<text style="font-size: 13px;color:white">{{code}}</text>
				</view>
				<view style="margin-left: auto;padding: 2px 4px 2px 4px;background-color: #d50000;border-radius: 2px;" v-if="code!=200&&code!=0&&loading==false">
					<text style="font-size: 13px;color:white">{{code}}</text>
				</view>
				</view>
				<textarea height="250"
				  placeholder="response"
				
				  v-model="response"
				  style="border-radius: 5px;border: #e3e8f0 solid 1px;padding:1%;width:98%"
									
				></textarea >
	
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
				admin:0,
				replymessageshow:false,
				method:"POST",
				loading:false,
				checkboxValue1:[],
				checkboxList1: [
					{
				        name: 'POST',
				        checked: true
				    },
				    {
				        name: 'GET',
				        checked: false
				    },
					{
					    name: 'CONNECT',
					    checked: false
					},
					{
					    name: 'OPTIONS',
					    checked: false
					},
					{
					    name: 'DELETE',
					    checked: false
					},
					{
					    name: 'HEAD',
					    checked: false
					},
					{
						name: 'TRACE',
						checked: false
					},
					{
						name: 'PUT',
						checked: false
					}
				],
				url1:"",
				header:"",
				data:"",
				response:"",
				code:0,
			}
		},
		onLoad() {
			
			
			this.replymessageshow=getApp().globalData.replymessageshow;
			this.userbackgroundimage=getApp().globalData.userbackgroundimage;
			this.admin=getApp().globalData.admin;
			this.userid=getApp().globalData.userid;
			this.username=getApp().globalData.username;
			this.userheadimage=getApp().globalData.userheadimage;
			
		},
		methods: {
			checkboxChange(e){
				for(var i=0;i<this.checkboxList1.length;i++){
					if(i==e){
						this.checkboxList1[i].checked=true;
						this.method=this.checkboxList1[i].name;
					}
					else this.checkboxList1[i].checked=false;
				}
			},
			send:function(){
				var header="";
				var data="";
				if(this.url1.length==0)return;
				if(this.header.length==0)header="{}";
				else header=this.header;
				if(this.data.length==0)data="{}";
				else data=this.data;
				data=JSON.parse(data);
				header=JSON.parse(header);
				this.data=JSON.stringify(data,null,'   ');
				this.header=JSON.stringify(header,null,'   ');
				this.loading=true;
				uni.request({
					url:this.url1,
					method:this.method,
					header:header,
					data:data,
					success:(res)=>{
						
						this.loading=false;
						this.code=res.statusCode;
						this.response=JSON.stringify(res.data,null,'   ')
			
					},
					fail:(res)=>{
						this.loading=false;
						this.code="fail"
						this.response=JSON.stringify(res,null,'   ')
				
					},
					complete:(res)=>{
						console.log(res);
					}
				})
			},
			back:function(){
				uni.navigateBack({
					
				})
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
