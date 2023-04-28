<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "@/uni_modules/uview-ui/index.scss";
</style>
<script>
	import { L2Dwidget } from 'live2d-widget'
	export default {
		data() {
			return {
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
				]
			}
		},	
		globalData: {  
		    //http: "http://127.0.0.1:8806",
			http: "https://www.txtz.club:8806",
			scroll_top1:0,
			userbackgroundimage:"",
			searchtuijian:[],
			username:"",
			scroll_top:0,
			ban:0,
			userid:-1,
			showl2d:false,
			l2d:0,
			email:"",
			userheadimage:"",
			admin:0,
			cookie:"",
			showjuge:true,
			attention:[],
			mylike:[],
			firstopen:[],
			replymessage:[],
			replymessagenum:0,
			replymessageid:0,
			replymessageshow:false,
			chatlist:[],
			chat:[],
			chat_username:"",
			chat_userid:0,
			chat_userheadimage:"",
			xintiao:null,
			chatlistback:[],
			showpixivbg:false,
			l2dlist:[
				'https://unpkg.com/live2d-widget-model-miku@1.0.5/assets/miku.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-chitose/assets/chitose.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-epsilon2_1/assets/Epsilon2.1.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-haru/01/assets/haru01.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-haru/02/assets/haru02.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-haruto/assets/haruto.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-hibiki/assets/hibiki.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-hijiki/assets/hijiki.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-izumi/assets/izumi.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-koharu/assets/koharu.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-nico/assets/nico.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-nietzsche/assets/nietzche.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-ni-j/assets/ni-j.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-nipsilon/assets/nipsilon.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-nito/assets/nito.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-shizuku/assets/shizuku.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-tororo/assets/tororo.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-tsumiki/assets/tsumiki.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-unitychan/assets/unitychan.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-wanko/assets/wanko.model.json',
				'https://txtzz-1301452902.file.myqcloud.com/L2Dwidget/live2d-widget-model-z16/assets/z16.model.json'
			]
		},  
		methods:{
			open:function(){
				if(getApp().globalData.cookie!='')
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
												
						}
					}
				})
				if(getApp().globalData.cookie!='')
				{
										   
					getApp().getmychat();
					var id=0;
					if(getApp().globalData.chatlist.length!=0){
						id=getApp().globalData.chatlist[getApp().globalData.chatlist.length-1].id;
					}
					uni.request({
					url:getApp().globalData.http+"/api/getmychatlist",
					method:"POST",
					data:{
						"id":id,
						"userid":getApp().globalData.userid,
					},
					success:(res)=>{
						for(var i=0;i<res.data.length;i++)
						getApp().globalData.chatlist.push(res.data[i]);
						//#ifdef APP-PLUS
						plus.push.addEventListener("click", function(msg) {
						    uni.reLaunch({
						    	
						
						    	url:'index?value=2'  
						    })                             
						}, false); 
						
						// createMessage
						var options = {cover:true};    
						var num=0;
						for(var i=0;i<res.data.length;i++){
							if(res.data[i].b!=getApp().globalData.userid)
							num++;
						}
						var str = num+"条新消息";
						if(res.data.length>1)
						plus.push.createMessage(str, "LocalMSG1", options);
						//#endif
						uni.setStorage({
						    key: "chatlist",
						    data: getApp().globalData.chatlist,
						    success: function () {
						    } 
						});
						getApp().connectSocketInit();
					}
					})
				}
			},
			getmychat:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getmychat",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						cookie:getApp().globalData.cookie,
						id:0
					},
					success:(res)=>{
						getApp().globalData.chat=res.data;
						for(var i=0;i<getApp().globalData.chat.length;i++)
						if(getApp().globalData.chat[i].image!=''){
							getApp().globalData.chat[i].message+="[图片]";
						}
					},
					fail:(res)=>{
						
					},
				})
			},
			send(a,b,message,image,type){
				var juge=0;
				this.socketTask.send({
					data: 
						'{"a":"'+a+'p",'+
						'"b":'+b+','+
						'"message":"'+message+'",'+
						'"image":"'+image+'",'+
						'"type":"pc"'+
						'}',
					
					async success() {
						
					},
				});
			},
			connectSocketInit:function() {
							// 创建一个this.socketTask对象【发送、接收、关闭socket都由这个对象操作】
							this.socketTask = uni.connectSocket({
								// 【非常重要】必须确保你的服务器是成功的,如果是手机测试千万别使用ws://127.0.0.1:9099【特别容易犯的错误】
								//url: "ws://127.0.0.1:8806/api/message/"+getApp().globalData.userid+"p",
								url: "wss://www.txtz.club:8806/api/message/"+getApp().globalData.userid+"p",
								success(data) {
									//console.log("websocket连接成功");
								},
							});
			 
							// 消息的发送和接收必须在正常连接打开中,才能发送或接收【否则会失败】
							this.socketTask.onOpen((res) => {
								//console.log("WebSocket连接正常打开中...！");
								this.is_open_socket = true;
								getApp().globalData.xintiao=null;
								clearInterval(getApp().globalData.xintiao);
								this.timeoutObj = setInterval(function() {
												uni.sendSocketMessage({
													data: '{"type":"ping"}',
													success: res => {
													//	console.log('连接中....');
													},
													fail: err => {
													//	console.log('连接失败重新连接....');
														getApp().connectSocketInit();
													}
												});
											}, 5000);
								// 注：只有连接正常打开中 ，才能正常成功发送消息
								
								// 注：只有连接正常打开中 ，才能正常收到消息
								this.socketTask.onMessage((res) => {
									//console.log("接受到消息");
									var temp=JSON.parse((res.data));
									if(temp.type=="back"){
										getApp().globalData.chatlistback.push(temp.message);
										
										for(var i=0;i<getApp().globalData.chatlist.length;i++){
										
											if(getApp().globalData.chatlist[i].id==temp.message){
											
												getApp().globalData.chatlist.splice(i,1);
												uni.setStorage({
												    key: "chatlist",
												    data: getApp().globalData.chatlist,
												    success: function () {
												    } 
												});
												
											}
										}
										return;
									}
									getApp().getmychat();
									getApp().globalData.chatlist.push(JSON.parse((res.data)));
									var userid1=getApp().globalData.chatlist[getApp().globalData.chatlist.length-1].a;
										var userid2=getApp().globalData.chatlist[getApp().globalData.chatlist.length-1].b;
										var userid3=0;
										if(userid1==getApp().globalData.userid){
											userid3=userid2;
										}
										else{
											userid3=userid1;
										}
										uni.request({
											url:getApp().globalData.http+"/api/getusername",
											method:"POST",
											data:{
												"userid":userid3
											},
											success:(res)=>{
												var username=res.data.username;
												//#ifdef APP-PLUS
												plus.push.addEventListener("click", function(msg) {
												    uni.reLaunch({
												    	
												
												    	url:'index?value=2'  
												    })                             
												}, false); 
												var options = {cover:true,title:username};    
												var str = getApp().globalData.chatlist[getApp().globalData.chatlist.length-1].message;    
												if(getApp().globalData.show1==true)
												plus.push.createMessage(str, "LocalMSG", options);
												//#endif
											}
									});
								});
							})
							// 这里仅是事件监听【如果socket关闭了会执行】
							this.socketTask.onClose(() => {
							//	console.log("已经被关闭了")
								getApp().connectSocketInit();
							})
						},
		},
		onShow:function(){
		},
		onLaunch: function() {
			uni.getStorage({
			    key:"showl2d",
			    success: function(res){
			        getApp().globalData.showl2d=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			 uni.getStorage({
			     key:"l2d",
			     success: function(res){
			         getApp().globalData.l2d=res.data;
			 		//console.log("admin:"+getApp().globalData.admin);
			 	}
			  });
			var url = getApp().globalData.l2dlist[getApp().globalData.l2d];
			if(getApp().globalData.showl2d)
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
			
			});
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
			setTimeout(() => {
				getApp().open();
			}
			, 50)
		},
		onHide: function() {
		},
		onShow: function()  {
			
			uni.request({
				url:getApp().globalData.http+"/api/gethottagschart",
				method:"POST",
				data:{},
				success: (res) => {
					{
						
						getApp().globalData.searchtuijian=res.data.hottagschart;
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
						getApp().globalData.userheadimage=res.data.data.userheadimage;
						getApp().globalData.userbackgroundimage=res.data.data.userbackgroundimage;
						getApp().globalData.toutu=res.data.data.toutu;
						this.admin=getApp().globalData.admin;
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
						    key: "toutu",
						    data: getApp().globalData.toutu,
						    success: function () {
						    }
						});
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
			uni.getStorage({
			    key:"showpixivbg",
			    success: function(res){
			        getApp().globalData.showpixivbg=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			uni.getStorage({
			    key:"firstopen",
			    success: function(res){
			        getApp().globalData.firstopen=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			uni.getStorage({
			    key:"userid",
			    success: function(res){
			        getApp().globalData.userid=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			 uni.getStorage({
			     key:"l2d",
			     success: function(res){
			         getApp().globalData.l2d=res.data;
			 		//console.log("admin:"+getApp().globalData.admin);
			 	}
			  });
			uni.getStorage({
			    key:"op",
			    success: function(res){
			        getApp().globalData.op=res.data;
					//console.log("admin:"+getApp().globalData.admin);
				}
			 });
			 uni.getStorage({
			     key:"cookie",
			     success: function(res){
			         getApp().globalData.cookie=res.data;
			 		//console.log("admin:"+getApp().globalData.admin);
			 	}
			  });
			  uni.getStorage({
			      key:"showl2d",
			      success: function(res){
			          getApp().globalData.showl2d=res.data;
			  		//console.log("admin:"+getApp().globalData.admin);
			  	}
			   });
			  uni.getStorage({
			      key:"showjuge",
			      success: function(res){
			          getApp().globalData.showjuge=res.data;
			  		//console.log("admin:"+getApp().globalData.admin);
			  	}
			   });
			  uni.getStorage({
			      key:"username",
			      success: function(res){
			          getApp().globalData.username=res.data;
			  		//console.log("admin:"+getApp().globalData.admin);
			  	}
			   });
			   uni.getStorage({
			       key:"email",
			       success: function(res){
			           getApp().globalData.email=res.data;
			   		//console.log("admin:"+getApp().globalData.admin);
			   	}
			    });
				uni.getStorage({
				    key:"userheadimage",
				    success: function(res){
				        getApp().globalData.userheadimage=res.data;
						//console.log("admin:"+getApp().globalData.admin);
					}
				 });
				 uni.getStorage({
				     key:"userbackgroundimage",
				     success: function(res){
				         getApp().globalData.userbackgroundimage=res.data;
				 		//console.log("admin:"+getApp().globalData.admin);
				 	}
				  });
				  uni.getStorage({
				      key:"sign",
				      success: function(res){
				          getApp().globalData.sign=res.data;
				  		//console.log("admin:"+getApp().globalData.admin);
				  	}
				   });
				   uni.getStorage({
				       key:"registertime",
				       success: function(res){
				           getApp().globalData.registertime=res.data;
				   		//console.log("admin:"+getApp().globalData.admin);
				   	}
				    });
					uni.getStorage({
					    key:"save",
					    success: function(res){
					        getApp().globalData.save=res.data;
							//console.log("admin:"+getApp().globalData.admin);
						}
					 });
					 uni.getStorage({
					     key:"bylike",
					     success: function(res){
					         getApp().globalData.bylike=res.data;
					 		//console.log("admin:"+getApp().globalData.admin);
					 	}
					  });
					  uni.getStorage({
					      key:"replymessagenum",
					      success: function(res){
					          getApp().globalData.replymessagenum=res.data;
					  	}
					   });
					   uni.getStorage({
					       key:"replymessageid",
					       success: function(res){
					           getApp().globalData.replymessageid=res.data;
					   		//console.log("admin:"+getApp().globalData.admin);
					   	}
					    });
						uni.getStorage({
						    key:"replymessageshow",
						    success: function(res){
						        getApp().globalData.replymessageshow=res.data;
								//console.log("admin:"+getApp().globalData.admin);
							}
						 });
			if(getApp().globalData.cookie!=''){
				{
					var id= getApp().globalData.replymessageid;
					uni.request({
						url:getApp().globalData.http+"/api/getreplymessagenum",
						method:"POST",
						data:{
							"id":id,
							"userid":getApp().globalData.userid,
							"cookie":getApp().globalData.cookie
						},
						success:(res)=>{
							getApp().globalData.replymessagenum=res.data.num;
								uni.setStorage({
								    key: "replymessagenum",
								    data: getApp().globalData.replymessagenum,
								    success: function () {
								    }
								});
							if(getApp().globalData.replymessagenum>0){
								getApp().globalData.replymessageshow=true;
								uni.setStorage({
								    key: "replymessageshow",
								    data: getApp().globalData.replymessageshow,
								    success: function () {
								    }
								});
							}
						}
					})
				}
				
				uni.request({
					url:getApp().globalData.http+"/api/user_visit",
					method:"POST",
					data:{
						"userid":getApp().globalData.userid
					},
					success: (res) => {;
					}
				})
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
						}
					}
				})
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

						}
					}
				})
			}
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
