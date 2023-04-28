<template>
	<view style="background-color: #ffffff;font-size: 16px;display: flex;flex-direction: row;">
		<view style="width:20rem;height:auto;box-shadow: 0 2PX 12PX 0 rgb(0 0 0 / 6%);background-color: white;padding:1rem">
			<view>
				<text style="font-size: 20px;font-weight: 700;">数 据</text>
				<u-divider ></u-divider>
				<view style="display: flex;flex-direction: row;color:#99dc6d">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>数据统计</text>
				</view>
			</view>
		</view>
		<view style="padding:1rem;width:100%" >
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px">数据统计</text>
				<u-divider style="width: 100%;" ></u-divider>
				<view style="display: flex;flex-direction: column;">
					<text style="font-size: 18px;font-weight: 700;margin-bottom: 1.5rem;">{{username}} 您好！</text>
					<text style="font-size: 16px;margin-bottom:0.5rem">现在是 {{nowDate}} &nbsp论坛已运行{{rundata}}</text>
					<u-divider style="width: 100%;" ></u-divider>
				</view>
			</view>
			<view style=";border:solid 1px #d8d8d8;padding-top: 1.5rem;padding-bottom: 1.5rem;margin-bottom: 1.5rem;">
				<view style="display: flex;flex-direction: row;justify-content: center;">
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">今日活跃用户数</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.todayhotuser}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;margin-left: 4rem;margin-right: 2rem;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">今日新增用户数</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.newregister}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;margin-left: 2rem;margin-right: 4rem;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">今日主题发布数</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.todaynewtheme}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">今日回复数</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.todaynewreply}}</text>
					</view>
				</view>
				<view style="display: flex;flex-direction: row;justify-content: center;margin-top: 2rem;">
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">用户总数量</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.totleusernum}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;margin-left: 4rem;margin-right: 2rem;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">主题总数量</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.totlethemenum}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;margin-left: 2rem;margin-right: 4rem;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">回复总数量</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.totlereplynum}}</text>
					</view>
					<view style="height:8rem;width:20rem;border:solid 1px #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;">
						<text style="font-size: 22px;color:#909399;font-weight: 500;">精华内容总数量</text>
						<text style="font-size: 22px;font-weight: 700;margin-top: 1rem;">{{today.jingnum}}</text>
					</view>
				</view>
			</view>
			<view>
				<view style=";border:solid 1px #d8d8d8;">
					<view style="width:100%;border-bottom: 1px solid #d8d8d8;padding-top:0.5rem;padding-bottom: 0.5rem;display: flex;flex-direction: row;">
						<u-icon name="calendar" size="25" style="margin-left: 1rem;" color="#99dc6d"></u-icon>
						<text style="font-size: 20px;margin-left: 0rem;">站点每日数据</text>
					</view>
					<qiun-data-charts 
				      type="line"
				      style="margin-top: 2rem;"
					  :canvas2d="true"
					  canvasId="OxsfiqbxdTjvPIEyBCaUARedLrThYdfH"
				      :chartData="chartData"
					  :opts="opts"
					  :loadingType="5"
				    />
				</view>
			</view>
			<view style="display: flex;flex-direction: row;margin-top: 2rem;margin-bottom: 1rem;">
				<view style="border:solid 1px #d8d8d8;width:48%">
					<view style="width:100%;border-bottom: 1px solid #d8d8d8;padding-top:0.5rem;padding-bottom: 0.5rem;display: flex;flex-direction: row;">
						<u-icon name="calendar" size="25" style="margin-left: 1rem;" color="#99dc6d"></u-icon>
						<text style="font-size: 20px;">文章分类占比</text>
					</view>
					<view class="charts-box">
					    <qiun-data-charts 
						  v-if="loadover"
					      type="pie"
					      :chartData="chartData1"
					      :canvas2d="true"
					      canvasId="ZSrOhhgEbTrAkWmNVBYACBmLdQItLnnS"
					    />
					  </view>
				</view>
				<view style="border:solid 1px #d8d8d8;width:48%;margin-left: 4%;">
					<view style="width:100%;border-bottom: 1px solid #d8d8d8;padding-top:0.5rem;padding-bottom: 0.5rem;display: flex;flex-direction: row;">
						<u-icon name="calendar" size="25" style="margin-left: 1rem;" color="#99dc6d"></u-icon>
						<text style="font-size: 20px;">热门话题</text>
					</view>
					<view class="charts-box">
					    <qiun-data-charts 
					        type="word"
					        :chartData="chartData2"
					        :canvas2d="true"
					        canvasId="owqpLZfCzNgRIMqMvBxTxmTuxabNKZXS"
							@getIndex="con"
					        />
					  </view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				 opts: {
				        color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc"],
				        padding: [15,10,0,15],
				        enableScroll: false,
				        legend: {},
				        xAxis: {
				          disableGrid: true
				        },
				        yAxis: {
				          gridType: "dash",
				          dashLength: 2
				        },
				        extra: {
				          line: {
				            type: "curve",
				            width: 2,
				                       activeType: "hollow",
				                       linearType: "custom",
				                       onShadow: false,
				                       animation: "horizontal"

				          }
				        }
				},
				loadover:false,
				username:"",
				nowDate:"",
				rundata:"",
				today:[],
				week:[],
				chartData:{
					categories:[],
					
					series:[
					
					]
				},
				chartData1:{
					series:[
					]	
				},
				chartData2:{
					series:[
	
					]
				}
			}
		},
		mounted() {
			this.onload();
		},
		methods: {
			con:function(e){
				console.log(e);
			},
			onload(){
				this.username=getApp().globalData.username;
				this.currentTime();
				uni.request({
					url:getApp().globalData.http+"/api/getStatistics",
					method:"POST",
					data:{},
					success: (res) => {
						this.today=res.data.today;
						{
							for(var i=6;i>=0;i--){
								this.chartData.categories.push(this.gettime(res.data.todaytime-i*(24*60*60)));
							}
						}
						{
							var data=[];
							for(var i=0;i<res.data.fenleichart.length;i++){
								data.push(res.data.fenleichart[i]);
							}
							var temp={"data":data};
							this.chartData1.series.push(temp);
						}
						{
							var temp=[];
							for(var i=0,j=30;i<res.data.hottagschart.length;i++,j--){
								temp.push({
									"name":res.data.hottagschart[i].name,
									"textSize":j,
									"data":undefined
								})
							}
							this.chartData2.series=temp;
							
						}
							var temp1={
								name:"每日发帖",
								data:res.data.week.weektodaynewtheme,
								setShadow: [
							        3,
									8,
									10,
							        "#1890FF"
							    ],
							}
							var temp2={
								name:"日活用户",
								data:res.data.week.weektodayhotuser,
								setShadow: [
							        3,
									8,
									10,
							        "#91CB74"
							    ],							
							}
							var temp3={
								name:"每日回复",
								data:res.data.week.weektodaynewreply,
								setShadow: [
							        3,
									8,
									10,
							        "#FAC858"
							    ],							
							}
							var temp4={
								name:"日注册数",
								data:res.data.week.weeknewregister,
								setShadow: [
							        3,
									8,
									10,
							        "#EE6666"
							    ],								
							}
							this.chartData.series.push(temp2);
							this.chartData.series.push(temp4);
							this.chartData.series.push(temp1);
							this.chartData.series.push(temp3);		
							this.loadover=true;
							
					}
				})
			},
			currentTime() {
			     setInterval(this.formatDate, 500);
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
			gettime:function(e){
				e=e*1000;
				var now = new Date(e)
				var month = now.getMonth() + 1; //取得日期中的月份，其中0表示1月，11表示12月
				var date = now.getDate(); //返回日期月份中的天数（1到31）
				return month +"-" + date
			}
		}
	}
</script>

<style>
.charts-box {
    width: 100%;
    height: 300px;
  }
</style>
