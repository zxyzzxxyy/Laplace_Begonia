<template>
	<view style="background-color: #ffffff;font-size: 16px;display: flex;flex-direction: row;overflow-y: hidden;">
		<view style="width:20rem;height:auto;box-shadow: 0 2PX 12PX 0 rgb(0 0 0 / 6%);background-color: white;padding:1rem">
			<view style="height:100%;">
				<text style="font-size: 20px;font-weight: 700;">主 题</text>
				<u-divider ></u-divider>
				<view style="display: flex;flex-direction: row;">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text :class="{'choose':show==0}" @click="show=0">分类管理</text>
				</view>
				<view style="display: flex;flex-direction: row;margin-top: 1rem;">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text :class="{'choose':show==1}" @click="show=1">主题管理</text>
				</view>
			</view>
		</view>
		<!--left-->
			<!--分类管理-->
				<view style="padding:1rem;width:100%;height:100%;overflow-y: auto;" v-show="show==0">
					<view style="display: flex;padding-left: 1rem;flex-direction: column;">
						<text style=";font-size: 20px">分类管理</text>
						<u-divider style="width: 100%;" ></u-divider>
						<text>分类名称</text>
						<view v-for="(item,index) in fenleilist" style="display: flex;flex-direction: row;">
							<view style="display: flex;flex-direction:column">
							<input v-model="fenleilist[index].fenlei" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;margin-right: 1rem;" />
							<view style="width:48rem;margin-left:2rem;margin-top:1rem;display:flex;flex-direction:column;" v-if="jugeshow(index)">
								<text style="font-size: 14px;color:#909399">身份</text>
							<view v-for="(item,index1) in getidentitylist(fenleilist[index].id)"
							style="display: flex;flex-direction:row;width:100%"
							>
								<input  v-model="item.identity" style="width:40rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;margin-right: 1rem;" />
								
									<text class="newfenlei" style="margin-top: 1rem;" @click="deleteidentity(index,index1)">删除</text>
							</view>
							<view style="display: flex;flex-direction:row;width:100%">
								
								<input  style="width:40rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;margin-right: 1rem;" 
								v-model="newidentitys" type="text"
								/>
								<text class="newfenlei" style="margin-top: 1rem;" @click="newidentity(fenleilist[index].id)">新增</text>
							</view>
							<view style="margin-top: 1rem;">
								<text style="font-size: 14px;color:#909399">兴趣</text>
							</view>
							<view v-for="(item,index1) in getinterestlist(fenleilist[index].id)"
							style="display: flex;flex-direction:row;width:100%"
							>
								<input  v-model="item.interest" style="width:40rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;margin-right: 1rem;" />
								
									<text class="newfenlei" style="margin-top: 1rem;" @click="deleteinterest(index,index1)">删除</text>
							</view>
							<view style="display: flex;flex-direction:row;width:100%">
								
								<input  style="width:40rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;margin-right: 1rem;" 
								v-model="newinterests" type="text"
								/>
								<text class="newfenlei" style="margin-top: 1rem;" @click="newinterest(fenleilist[index].id)">新增</text>
							</view>
							</view></view>
						<view style="display: flex;flex-direction:column">
							<text v-if="fenleilist[index].fenlei!='默认分类'" class="newfenlei" style="margin-top: 1rem;" @click="changeshow9(index)">新增身份/兴趣</text>
							<text v-if="fenleilist[index].fenlei!='默认分类'" class="newfenlei" style="margin-top: 0.5rem;" @click="temp_item=item,temp_index=index,show2=true">删除</text>
						</view>
						</view>
						<input v-if="show1==1"  v-model="newfenlei" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
						<text class="newfenlei" style="margin-top: 1rem;" @click="show1=1" v-if="show1==0">新增</text>
						<view v-if="show1==1" style="width:10rem;display: flex;;">
						<text class="newfenlei" style="margin-top: 1rem;width:5rem" @click="show1=0,addfenlei()" >保存</text>
						<text class="newfenlei" style="margin-top: 1rem;" @click="show1=0" >取消</text>
						</view>
					</view>
				</view>
			<!--主题管理-->
				<view style="padding:1rem;width:90%;height:100%;overflow-y: auto;overflow-x: hidden;" v-show="show==1">
					<view style="display: flex;padding-left: 1rem;flex-direction: column;">
						<view style="display: flex;flex-direction: row;align-items: center;">
						<text style=";font-size: 20px;width:6rem">主题管理</text>
						<text style="font-size: 16px;width: 3rem;" :class="{'choose':select==0}" @click="select=0">主题</text>
						<text style="width: 3rem;" :class="{'choose':select==1}" @click="select=1">回复</text>
						<text style="width: 5rem;" :class="{'choose':select==2}" @click="select=2">楼层回复</text>
						</view>
						<u-divider style="width: 100%;" ></u-divider>
						<!--主题-->
						<view style="display: flex;flex-direction: row;flex-wrap:wrap;width:100%;;"
						v-if="select==0"
						>
					
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
								<text>作者:</text>
								<input v-model="theme.username" style="margin-left: 2rem;width:15rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
								<text>主题ID:</text>
								<input v-model="theme.themeid" style="margin-left: 2rem;width:15rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;width:40rem;margin-left: auto;margin-right: 5rem;margin-top: 1rem;">
								<text>主题内容:</text>
								<input v-model="theme.data" style="margin-left: 2rem;width:33rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-top: 1rem;margin-right: 5rem;width:30rem;">
								<text>搜索范围:</text>
								<view style="height:2.3rem;border: 1px solid #d8d8d8;border-radius: 3px;width:10rem;margin-left: 2rem;display: flex;
								align-items: center;padding-left: 0.5rem;
								" @click="show3=true">
									<text >{{columns[0][c_sub]}}</text>
									<u-icon name="arrow-down" style="margin-left: auto;margin-right: 0.5rem;"></u-icon>
								</view>
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-top: 1rem;margin-right: 5rem;">
								<text>主题类型:</text>
								<view style="height:2.3rem;border: 1px solid #d8d8d8;border-radius: 3px;width:10rem;margin-left: 2rem;display: flex;
								align-items: center;padding-left: 0.5rem;
								" @click="show4=true">
									<text >{{columns1[0][c_sub1]}}</text>
									<u-icon name="arrow-down" style="margin-left: auto;margin-right: 0.5rem;"></u-icon>
								</view>
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-top: 1rem;margin-left: auto;margin-right: 5rem;">
								<text>发布时间:</text>
								<view style="height:2.3rem;border: 1px solid #d8d8d8;border-radius: 3px;width:15rem;margin-left: 2rem;display: flex;
								align-items: center;padding-left: 0.5rem;justify-content: center;
								" @click="show5=true">
									<text style="width:7rem;color:#c8c9cc">{{startime}}</text>
									<text style="width:1.2rem">-</text>
									<text style="width:7rem;color:#c8c9cc">{{endtime}}</text>
									<u-icon name="arrow-down" style="margin-left: auto;margin-right: 0.5rem;"></u-icon>
								</view>
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-top: 1rem;width:40rem;">
								<text>回复数</text>
								<u--input v-model="minreply" style="margin-left: 2rem;max-height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;margin-right: 1rem;max-width:10rem" type="number"  placeholder="大于" @change="changeminreply"></u--input><text>-</text>
								<u--input v-model="maxreply" style="margin-left: 1rem;max-width:10rem;max-height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" type="number"  placeholder="小于" @change="changemaxreply"></u--input>
							</view>
							<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-top: 1rem;margin-left: auto;margin-right: 5rem;">
								<text>浏览次数</text>
								<u--input v-model="minlook" style="margin-left: 2rem;max-height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;margin-right: 1rem;max-width:10rem" type="number"  placeholder="大于" @change="changeminlook"></u--input><text>-</text>
								<u--input v-model="maxlook" style="margin-left: 1rem;max-width:10rem;max-height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" type="number"  placeholder="小于" @change="changemaxlook"></u--input>
								<view style="width:5rem;margin-left: 1rem;">
								<u-button  v-show="searchbuttom==0" type="primary" text="搜索" size="18"
								@click="search(),searchbuttom=1"
								></u-button>
								<u-button v-show="searchbuttom==1" type="primary" loading loadingText="搜索中" size="18"
								
								></u-button>
								</view>
							</view>
							<u-divider style="width:100%"></u-divider>
							<view style="background-color: #f4f4f5;height:5rem;width:100%;border-right: 1px solid #f4f4f5;;border-left: 1px solid #f4f4f5;display: flex;align-items: center;" v-if="themelist.length>0">
							<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:5%">
								<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="selectall==1" @click="deleteall()">
									<u-icon name="checkbox-mark" color="white"></u-icon>
								</view>
								<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="selectall==0" @click="selectall1()">
									<u-icon name="checkbox-mark" color="white"></u-icon>
								</view>
							</view>
								<view style="margin-left: 1rem;">
									主题列表
								</view>
							</view>
							<view style="border-bottom: 1px solid #d8d8d8;width:100%;border-left:1px solid #d8d8d8;
							
							" v-if="themelist.length>0">
								<view v-for="(item,index) in themelist" style="width: 100%;display: flex;row">
									<view style="width:5%;display: flex;align-items: center;justify-content: center;
									border-top: 0.1px solid #d8d8d8;
									border-right: 1px solid #d8d8d8;
									">
										<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="jugetheme(item.themeid)" @click="addselecttheme(item.themeid)"> 
											<u-icon name="checkbox-mark" color="white"></u-icon>
										</view>
										<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="!jugetheme(item.themeid)" @click="addselecttheme(item.themeid)">
											<u-icon name="checkbox-mark" color="white"></u-icon>
										</view>
									</view>
									<view style="width:95%;">
										<view style="display: flex;flex-direction: row;padding:1rem
											border-left:1px solid #d8d8d8;
											border-right: 1px solid #d8d8d8;
											border-bottom: none;
											border-top: 0.1px solid #d8d8d8;
											"
											>
											<view style="width:5rem">
											<text>{{item.username}}</text>
											</view>
											<view style="margin-left: 1rem;margin-right: 1rem;width:5rem">
											<text>发布于</text>
											</view>
											<view style="width:5rem">
											<text>{{item.fenlei}}</text>
											</view>
											<view style="margin-right: 2rem;margin-left: auto;display: flex;flex-direction: row;font-size: 14px;color:#909399">
												<view style="margin-right: 1rem;">
													<text>回复/查看</text>
												</view>
												<view style="margin-right: 2rem;">
													<text>{{item.num}}/{{item.look}}</text>
												</view>
												<view style="margin-right: 1rem;">
													<text>发布时间:</text>
												</view>
												<view>
													<text>{{time(item.createtime)}}</text>
												</view>
												<view style="margin-left: 1rem;">
													<text class="changecolor"
													@click="linkto(item.themeid)"
													>查看详情</text>
												</view>
											</view>
										</view>
										<view style="display: flex;flex-direction: row;padding:1rem
											border-left:1px solid #d8d8d8;
											border-right: 1px solid #d8d8d8;
											border-bottom: none;
											border-top: 0.1px solid #d8d8d8;
											"
											>
												<view style="width:4rem">
													<text>状态:</text>
												</view>
												<view style="color:red" v-if="item.show1==0">
												<text>已删除</text>
												</view>
												<view v-if="item.show1==1">
												<text style="color:#5ac725">正常</text>
												</view>
												<view v-if="item.jing==1" style="margin-left: 4rem;">
												<text style="color:#8bc863">精华</text>
												</view>
												<view v-if="item.top==1" style="margin-left: 4rem;">
												<text style="color:#8bc863">顶置</text>
												</view>
												<view style="margin-right: 2rem;margin-left: auto;" class="changecolor" v-if="item.show1==1"
												@click="deletetheme_admin(item.themeid)"
												>
													删除
												</view>
												<view style="margin-right: 2rem;margin-left: auto;" class="changecolor" v-if="item.show1==0"
												@click="huifutheme_admin(item.themeid)"
												>
													恢复
												</view>
										</view>
										<view style="display: flex;flex-direction: row;padding:1rem
											border-left:1px solid #d8d8d8;
											border-right: 1px solid #d8d8d8;
											border-bottom: none;
											border-top: 0.1px solid #d8d8d8;
											"
											>
											<view style="width:4rem">
												<text>主题ID:</text>
											</view>
											<view >
											<text>{{item.themeid}}</text>
											</view>
											
										</view>
										<view style="display: flex;flex-direction: row;padding:1rem
											border-left:1px solid #d8d8d8;
											border-right: 1px solid #d8d8d8;
											border-bottom: none;
											border-top: 0.1px solid #d8d8d8;
											"
											>
											<view style="width:3rem">
												<text>标题:</text>
											</view>
											<view >
											<text>{{item.title}}</text>
											</view>
											<view style="width:auto" v-if="item.title.length==0">
											<text>无</text>
											</view>
										</view>
										<view style="
										border-top: 0.1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										padding:1rem;display: flex;flex-direction: row;">
											<view style="width:3rem">
												<text >标签:</text>
											</view>
											<u-tag v-for="(item1,index) in tags(item.tags)" :text="item1"  icon="tags-fill" style="width: auto;margin-right:10px" plain plainFill @click="linktag(item)"></u-tag>
											<text v-if="tags(item.tags).length==0">无</text>
										</view>
										<view style="
										
										border-top: 0.1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										padding:1rem;display: flex;flex-direction: row;min-height:5rem">
										<text>正文内容:</text>
											<u-read-more style="max-width: 100%;margin-top: 3rem;" showHeight="10" :toggle="true"  ref="uReadMore" >
												<mp-html
												@load="load(index)"
												:content="item.text" 
												style=";margin-bottom: 25px;width:70vw;height:auto"
												/>
											</u-read-more>
										</view>
									</view>
								</view>
							</view>
							<view style="height:5rem;width:100%;display: flex;justify-content: center;align-items: center;" v-if="themelist.length>0">
								<view style="margin-right: 1rem;">共: {{themenum}}条</view> 
								<view style="margin-right: 1rem;color:#398ade" v-if="page!=1" @click="lastpage()">上一页</view>
								<view style="margin-right: 1rem;" v-if="page!=1">{{page-1}}</view>
								<view style="margin-right: 1rem;color:#8bc863">{{page}}</view>
								<view style="margin-right: 1rem;" v-if="jugepagemax()">{{page+1}}</view>
								<view style="color:#398ade" v-if="themenum>page*10" @click="nextpage()">下一页</view>
							</view>
							<view style="width:100%;margin-top: 2rem;" v-if="themelist.length!=0">
								<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
									<view style="width:10rem;">
										<text>操作</text>
									</view>
									<view>
										<text>选项</text>
									</view>
								</view>
								<u-divider ></u-divider>
								<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
									<view style="width:10rem;">
										<text>批量删除</text>
									</view>
									<view style="display: flex;flex-direction: row;">
										<view style="width:7rem" class="changecolor" @click="btn1=true">删除</view>
										<view  class="changecolor" @click="btn2=true">恢复</view>
									</view>
								</view>
								<u-divider ></u-divider>
								<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
									<view style="width:10rem;">
										<text>批量顶置</text>
									</view>
									<view style="display: flex;flex-direction: row;">
										<view style="width:7rem"  class="changecolor" @click="btn3=true">设置顶置</view>
										<view  class="changecolor" @click="btn4=true">撤销顶置</view>
									</view>
								</view>
								<u-divider ></u-divider>
								<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
									<view style="width:10rem;">
										<text>批量精华</text>
									</view>
									<view style="display: flex;flex-direction: row;">
										<view style="width:7rem"  class="changecolor" @click="btn5=true">设置精华</view>
										<view  class="changecolor" @click="btn6=true">撤销精华</view>
									</view>
								</view>
								<u-divider ></u-divider>
								<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;" >
									<view style="width:auto;">
										<text>已选择{{selecttheme.length}}条主题</text>
									</view>
								</view>
								<u-divider ></u-divider>
							</view>
							<view v-if="show7" style="width:100%;margin-top: 1rem;display: flex;flex-direction: column;align-items: center;justify-content: center;">
								<u-divider ></u-divider>
								<text style="font-size: 14px;color:#909399;">没有搜索到...</text>
							</view>
		</view>
		<!--************************************************************回复*****************************************************-->
				<view v-if="select==1" style="display: flex;height:auto;width:100%;display: flex;flex-direction: column;">
					<view style="display: flex;flex-direction: row;flex-wrap: wrap;">
						<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
							<text>作者:</text>
							<input v-model="reply.username" style="margin-left: 2rem;width:30rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
						</view>
						<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
							<text>内容包括:</text>
							<input v-model="reply.data" style="margin-left: 2rem;width:30rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
						</view>
						<view style="display: flex;flex-direction: row;width:100%;align-items: center;margin-top: 1rem;margin-left: auto;margin-right: 5rem;margin-bottom: 1rem;">
							<text>发布时间:</text>
							<view style="height:2.3rem;border: 1px solid #d8d8d8;border-radius: 3px;width:15rem;margin-left: 2rem;display: flex;
							align-items: center;padding-left: 0.5rem;justify-content: center;;
							" @click="show8=true">
								<text style="width:7rem;color:#c8c9cc">{{startime1}}</text>
								<text style="width:1.2rem">-</text>
								<text style="width:7rem;color:#c8c9cc">{{endtime1}}</text>
								<u-icon name="arrow-down" style="margin-left: auto;margin-right: 0.5rem;"></u-icon>
							</view>
							<view style="width:5rem;margin-left: 2rem;margin-right: 2rem;">
								<u-button  v-show="searchbuttom1==0" type="primary" text="搜索" size="18"
								@click="getreply(),searchbuttom1=1"
								></u-button>
								<u-button v-show="searchbuttom1==1" type="primary" loading loadingText="搜索中" size="18"
								></u-button>
							</view>
						</view>
					</view>
					<view style="background-color: #f4f4f5;height:5rem;width:100%;border-right: 1px solid #f4f4f5;;border-left: 1px solid #f4f4f5;display: flex;align-items: center;" >
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:5%">
						<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="selectall2==1" @click="deleteall_1()">
							<u-icon name="checkbox-mark" color="white"></u-icon>
						</view>
						<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="selectall2==0" @click="selectall_1()">
							<u-icon name="checkbox-mark" color="white"></u-icon>
						</view>
					</view>
					<view>
					</view>
						<view style="margin-left: 1rem;">
							回复列表
						</view>
					</view>
					<!--********************-->
					<view>
						<view style="border-bottom: 1px solid #d8d8d8;width:100%;border-left:1px solid #d8d8d8;
						" v-if="replylist.length>0">
							<view v-for="(item,index) in replylist" style="width: 100%;display: flex;row">
								<view style="width:5%;display: flex;align-items: center;justify-content: center;
								border-top: 0.1px solid #d8d8d8;
								border-right: 1px solid #d8d8d8;
								">
									<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="jugereply(item.id)" @click="addselectreply(item.id)"> 
										<u-icon name="checkbox-mark" color="white"></u-icon>
									</view>
									<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="!jugereply(item.id)" @click="addselectreply(item.id)">
										<u-icon name="checkbox-mark" color="white"></u-icon>
									</view>
								</view>
								<view style="width:95%;">
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										<view style="width:5rem">
										<text>{{item.username}}</text>
										</view>
										<view style="margin-left: 1rem;margin-right: 1rem;width:5rem">
										<text>回复于</text>
										</view>
										<view style="width:auto">
										<text>{{item.title}}</text>
										</view>
										<view style="margin-right: 2rem;margin-left: auto;font-size: 14px;color:#909399;display: flex;flex-direction: row;">
											<view style="margin-right: 0.5rem;">
												<text>发布时间: </text>
											</view>
											<view>
												<text> {{time(item.time)}}</text>
											</view>
										</view>
									</view>
									
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										{{item.data}}
									</view>
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"v-if="item.image!=''"
										>
											<text>图片:</text>
											<u-read-more style="max-width: 100%;margin-top: 3rem;" showHeight="10" :toggle="true">
												<u-album  :urls="imagesrc(item.image)" multipleSize="280" singleSize="850" space="10"></u-album>
											</u-read-more>										
									</view>
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										<text class="changecolor" v-if="item.show1==1" @click="reply_id=item.id,btn7=true">删除</text>	
										<text class="changecolor" v-if="item.show1==0" @click="reply_id=item.id,btn8=true">恢复</text>
										<view style="margin-left: auto;margin-right: 2rem;display: flex;flex-direction: row;">
											<view style="margin-right: 0.5rem;">状态:</view>
											<view v-if="item.show1==1" style="color:#5ac725;">正常</view>
											<view v-if="item.show1==0" style="color:red">已删除</view>
										</view>
									</view>
									
								</view>
							</view>
						</view>
					</view>
					<view style="height:5rem;width:100%;display: flex;justify-content: center;align-items: center;" v-if="replylist.length>0">
						<view style="margin-right: 1rem;">共: {{replynum}}条</view> 
						<view style="margin-right: 1rem;color:#398ade" v-if="page1!=1" @click="lastpage1()">上一页</view>
						<view style="margin-right: 1rem;" v-if="page1!=1">{{page1-1}}</view>
						<view style="margin-right: 1rem;color:#8bc863">{{page1}}</view>
						<view style="margin-right: 1rem;" v-if="jugepagemax1()">{{page1+1}}</view>
						<view style="color:#398ade" v-if="replynum>=page1*10" @click="nextpage1()">下一页</view>
					</view>
					<view style="width:100%;margin-top: 2rem;" v-if="replylist.length>0">
						<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
							<view style="width:10rem;">
								<text>操作</text>
							</view>
							<view>
								<text>选项</text>
							</view>
						</view>
						<u-divider ></u-divider>
						<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;">
							<view style="width:10rem;">
								<text>批量删除</text>
							</view>
							<view style="display: flex;flex-direction: row;">
								<view style="width:7rem" class="changecolor" @click="btn9=true">删除</view>
								<view  class="changecolor" @click="btn10=true">恢复</view>
							</view>
						</view>
						<u-divider ></u-divider>
						<view style="display: flex;flex-direction: row;width:100%;padding-left: 3rem;" >
							<view style="width:auto;">
								<text>已选择{{selectreply.length}}条回复</text>
							</view>
						</view>
						<u-divider ></u-divider>
					</view>
					<!--********************-->
					
					<!--********************-->
				</view>
				<view v-if="select==2" style="display: flex;height:auto;width:100%;display: flex;flex-direction: column;">
					<view style="display: flex;flex-direction: row;flex-wrap: wrap;">
						<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
							<text>作者:</text>
							<input v-model="loucengreply.username" style="margin-left: 2rem;width:30rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
						</view>
						<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
							<text>内容包括:</text>
							<input v-model="loucengreply.data" style="margin-left: 2rem;width:30rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" />
						</view>
						<view style="display: flex;flex-direction: row;width:100%;align-items: center;margin-top: 1rem;margin-left: auto;margin-right: 5rem;margin-bottom: 1rem;">
							<text>发布时间:</text>
							<view style="height:2.3rem;border: 1px solid #d8d8d8;border-radius: 3px;width:15rem;margin-left: 2rem;display: flex;
							align-items: center;padding-left: 0.5rem;justify-content: center;;
							" @click="show10=true">
								<text style="width:7rem;color:#c8c9cc">{{startime2}}</text>
								<text style="width:1.2rem">-</text>
								<text style="width:7rem;color:#c8c9cc">{{endtime2}}</text>
								<u-icon name="arrow-down" style="margin-left: auto;margin-right: 0.5rem;"></u-icon>
							</view>
							<view style="width:5rem;margin-left: 2rem;margin-right: 2rem;">
								<u-button  v-show="searchbuttom2==0" type="primary" text="搜索" size="18"
								@click="getloucengreply(),searchbuttom2=1"
								></u-button>
								<u-button v-show="searchbuttom2==1" type="primary" loading loadingText="搜索中" size="18"
								></u-button>
							</view>
						</view>
					</view>
					<view style="background-color: #f4f4f5;height:5rem;width:100%;border-right: 1px solid #f4f4f5;;border-left: 1px solid #f4f4f5;display: flex;align-items: center;" >
					<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;width:5%">
						<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="selectall2==1" @click="deleteall_1()">
							<u-icon name="checkbox-mark" color="white"></u-icon>
						</view>
						<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="selectall2==0" @click="selectall_1()">
							<u-icon name="checkbox-mark" color="white"></u-icon>
						</view>
					</view>
					<view>
					</view>
						<view style="margin-left: 1rem;">
							回复列表
						</view>
					</view>
					<!--********************-->
					<view>
						<view style="border-bottom: 1px solid #d8d8d8;width:100%;border-left:1px solid #d8d8d8;
						" v-if="loucengreplylist.length>0">
							<view v-for="(item,index) in loucengreplylist" style="width: 100%;display: flex;row">
								<view style="width:5%;display: flex;align-items: center;justify-content: center;
								border-top: 0.1px solid #d8d8d8;
								border-right: 1px solid #d8d8d8;
								">
									<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:#8bc863;" v-if="jugereply(item.id)" @click="addselectreply(item.id)"> 
										<u-icon name="checkbox-mark" color="white"></u-icon>
									</view>
									<view style="width:1rem;height:1rem;border-radius: 4px;border:1px solid #d8d8d8;display: flex;flex-direction: column;align-items: center;justify-content: center;background-color:white;" v-if="!jugereply(item.id)" @click="addselectreply(item.id)">
										<u-icon name="checkbox-mark" color="white"></u-icon>
									</view>
								</view>
								<view style="width:95%;">
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										<view style="width:5rem">
										<text>{{item.username}}</text>
										</view>
										<view style="margin-left: 1rem;margin-right: 1rem;width:5rem">
										<text>回复于</text>
										</view>
										<view style="width:auto">
										<text>{{item.title}}</text>
										</view>
										<view style="width:auto;margin-left:10px">
										<text>{{item.louceng}}楼</text>
										</view>
										<view style="margin-right: 2rem;margin-left: auto;font-size: 14px;color:#909399;display: flex;flex-direction: row;">
											<view style="margin-right: 0.5rem;">
												<text>发布时间: </text>
											</view>
											<view>
												<text> {{time(item.time)}}</text>
											</view>
										</view>
									</view>
									
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										{{item.data}}
									</view>
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"v-if="item.image!=''"
										>
											<text>图片:</text>
											<u-read-more style="max-width: 100%;margin-top: 3rem;" showHeight="10" :toggle="true">
												<u-album  :urls="imagesrc(item.image)" multipleSize="280" singleSize="850" space="10"></u-album>
											</u-read-more>										
									</view>
									<view style="display: flex;flex-direction: row;padding:1rem
										border-left:1px solid #d8d8d8;
										border-right: 1px solid #d8d8d8;
										border-bottom: none;
										border-top: 0.1px solid #d8d8d8;
										"
										>
										<text class="changecolor" v-if="item.show1==1" @click="loucengreply_id=item.id,btn11=true">删除</text>	
										<text class="changecolor" v-if="item.show1==0" @click="loucengreply_id=item.id,btn12=true">恢复</text>
										<view style="margin-left: auto;margin-right: 2rem;display: flex;flex-direction: row;">
											<view style="margin-right: 0.5rem;">状态:</view>
											<view v-if="item.show1==1" style="color:#5ac725;">正常</view>
											<view v-if="item.show1==0" style="color:red">已删除</view>
										</view>
									</view>
									
								</view>
							</view>
						</view>
					</view>
					<view style="height:5rem;width:100%;display: flex;justify-content: center;align-items: center;" v-if="loucengreplylist.length>0">
						<view style="margin-right: 1rem;">共: {{loucengreplynum}}条</view> 
						<view style="margin-right: 1rem;color:#398ade" v-if="page2!=1" @click="lastpage2()">上一页</view>
						<view style="margin-right: 1rem;" v-if="page2!=1">{{page2-1}}</view>
						<view style="margin-right: 1rem;color:#8bc863">{{page2}}</view>
						<view style="margin-right: 1rem;" v-if="jugepagemax2()">{{page2+1}}</view>
						<view style="color:#398ade" v-if="loucengreplynum>=page2*10" @click="nextpage2()">下一页</view>
					</view>
					
					<!--********************-->
					
					<!--********************-->
				</view>
			</view>
		</view>
		<!--************************************************************END***********************************************************-->
			
		<!--************************************************************END***********************************************************-->
		<u-modal @cancel="show2=false"  showCancelButton :show="show2" @confirm="deletefenlei()" ref="uModal" title="是否删除分类" ></u-modal>

		<view>
				<u-toast ref="uToast"></u-toast>
		</view>
		<view>
				<u-picker title="选择分类" :show="show3" :columns="columns" @cancel="show3=false" @confirm="choosetag" :closeOnClickOverlay="true" @close="show3=false"></u-picker>
		</view>
		<view>
				<u-picker title="选择类型" :show="show4" :columns="columns1" @cancel="show4=false" @confirm="choosetype" :closeOnClickOverlay="true" @close="show4=false"></u-picker>
		</view>
		<view>
			<u-calendar :show="show5" mode='range' @confirm="date"  :minDate="mindate" @close="show5=false" :closeOnClickOverlay="true"
			></u-calendar>
		</view>
		<view>
			<u-calendar :show="show8" mode='range' @confirm="date1"  :minDate="mindate" @close="show8=false" :closeOnClickOverlay="true"
			></u-calendar>
		</view>
		<view>
			<u-calendar :show="show10" mode='range' @confirm="date2"  :minDate="mindate" @close="show10=false" :closeOnClickOverlay="true"
			></u-calendar>
		</view>
		<view >
			<u-modal :show="btn1" title="提示" content='是否删除所选主题' :showCancelButton="true" @cancel="btn1=false"
			@confirm="deletethemeall_admin(),btn1=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn2" title="提示" content='是否恢复所选主题' :showCancelButton="true" @cancel="btn1=false"
			@confirm="huifuthemeall_admin(),btn2=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn3" title="提示" content='是否顶置所选主题' :showCancelButton="true" @cancel="btn1=false"
			@confirm="btn3=false,settop_admin()"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn4" title="提示" content='是否删除所选主题顶置' :showCancelButton="true" @cancel="btn1=false"
			@confirm="btn4=false,deletetop_admin()"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn5" title="提示" content='是否设置所选主题为精选' :showCancelButton="true" @cancel="btn1=false"
			@confirm="btn5=false,setjing_admin()"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn6" title="提示" content='是否取消所选主题为精选' :showCancelButton="true" @cancel="btn1=false"
			@confirm="btn6=false,deletejing_admin()"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn7" title="提示" content='是否删除所选回复' :showCancelButton="true" @cancel="btn7=false"
			@confirm="deletereply_admin(),btn7=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn8" title="提示" content='是否恢复所选回复' :showCancelButton="true" @cancel="btn8=false"
			@confirm="huifureply_admin(),btn8=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn9" title="提示" content='是否删除所选回复' :showCancelButton="true" @cancel="btn9=false"
			@confirm="deletereplyall_admin(),btn9=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn10" title="提示" content='是否恢复所选回复' :showCancelButton="true" @cancel="btn10=false"
			@confirm="huifureplyall_admin(),btn10=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn11" title="提示" content='是否删除所选回复' :showCancelButton="true" @cancel="btn9=false"
			@confirm="deletloucengereply_admin(),btn11=false"
			></u-modal>
		</view>
		<view >
			<u-modal :show="btn12" title="提示" content='是否恢复所选回复' :showCancelButton="true" @cancel="btn10=false"
			@confirm="huifuloucengreply_admin(),btn12=false"
			></u-modal>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchbuttom:0,
				searchbuttom1:0,
				searchbuttom2:0,
				btn1:false,
				btn2:false,
				btn3:false,
				btn4:false,
				btn5:false,
				btn6:false,
				btn7:false,
				btn8:false,
				btn9:false,
				btn10:false,
				btn11:false,
				btn12:false,
				select:0,
				show:0,
				show1:0,
				show2:false,
				show3:false,
				show4:false,
				show5:false,
				show6:false,
				show7:false,
				show8:false,
				show9:[],
				show0:[],
				show10:false,
				temp_item:"",
				newinterests:"",
				temp_index:0,
				fenleilist:[],
				mindate:"",
				columns:[[]],
				columns1:[["默认","精华","顶置"]],
				c_sub:0,
				c_sub1:0,
				newfenlei:"",
				startime:"开始日期",
				endtime:"结束日期",
				startime1:"开始日期",
				endtime1:"结束日期",
				startime2:"开始日期",
				endtime2:"结束日期",
				minreply:"",
				maxreply:"",
				minlook:"",
				maxlook:"",
				limit:0,
				limit1:0,
				limit2:0,
				themenum:0,
				replynum:0,
				loucengreplynum:0,
				themelist:[],
				page:0,
				page1:0,
				page2:0,
				reply_id:0,
				loucengreply_id:0,
				selectall:0,
				selectall2:0,
				selectreply:[],
				selecttheme:[],
				replylist:[],
				loucengreplylist:[],
				identity:[],
				interest:[],
				newidentitys:"",
				reply:{
					username:"",
					data:"",
					startime:0,
					endtime:2100000000,
				},
				loucengreply:{
					username:"",
					data:"",
					startime:0,
					endtime:2100000000,
				},
				theme:{
					username:"",
					themeid:"",
					data:"",
					fenlei:"",
					type:"默认",
					jing:'',
					top:'',
					startime:0,
					endtime:2100000000,
					minreply:0,
					maxreply:210000000,
					minlook:0,
					maxlook:210000000,
					
				},
				upload:[
					{
						type: 'success',
						icon: false,
						title: '成功主题',
						message: "添加成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "添加失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'success',
						icon: false,
						title: '成功主题',
						message: "删除成功",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message: "删除失败",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
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
				]
			}
		},
		mounted() {
			this.onshow();
		},
		onShow(){
			uni.reLaunch({
				url:"../index/index"
			})
		},
		methods: {
			changeshow9:function(e){
				//var temp=!this.show[e]9;
				this.$set(this.show9,e,!this.show9[e]);
				for(var i=0;i<this.show9.length;i++){
					if(i==e)continue;
					this.$set(this.show9,i,false);
				}
				this.newidentitys="";
				this.newinterests="";
			},
			deletereplyall_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletereplyall_admin",
					method:"POST",
					data:{
						"themelist":this.selectreply
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page1--;
							this.getreply();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			jugeshow:function(e){
				if(this.show9[e]){
					return true;
				}
				return false;
			},
			huifureplyall_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/huifureplyall_admin",
					method:"POST",
					data:{
						"themelist":this.selectreply
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page1--;
							this.getreply();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			load:function(i){
				this.$nextTick(() => {
				    	this.$refs.uReadMore[i].init();
				})
			},
			getidentitylist:function(e){
				return this.identity[e];
			},
			getinterestlist:function(e){
				return this.interest[e];
			},
			deletereply_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletereply_admin",
					method:"POST",
					data:{
						"id":this.reply_id
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.replylist.length;i++){
								if(this.replylist[i].id==this.reply_id){
									this.replylist[i].show1=0;break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			huifureply_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/huifureply_admin",
					method:"POST",
					data:{
						"id":this.reply_id
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.replylist.length;i++){
								if(this.replylist[i].id==this.reply_id){
									this.replylist[i].show1=1;break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			deletloucengereply_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletloucengereply_admin",
					method:"POST",
					data:{
						"id":this.loucengreply_id
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.loucengreplylist.length;i++){
								if(this.loucengreplylist[i].id==this.loucengreply_id){
									this.loucengreplylist[i].show1=0;break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			huifuloucengreply_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/huifuloucengreply_admin",
					method:"POST",
					data:{
						"id":this.loucengreply_id
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.loucengreplylist.length;i++){
								if(this.loucengreplylist[i].id==this.loucengreply_id){
									this.loucengreplylist[i].show1=1;break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			imagesrc:function(e){
				if(e==null)return;
				var arr=e.split(';');
				arr.pop();
				return  arr;
			},
			getloucengreply:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getloucengreply_admin",
					method:"POST",
					data:{
						"limit":parseInt(this.limit2*10),
						"username":this.loucengreply.username,
						"startime":this.loucengreply.startime,
						"endtime":this.loucengreply.endtime,
						"data":this.loucengreply.data
					},
					success: (res) => {
						this.loucengreplylist=res.data.data;
						this.loucengreplynum=res.data.num;
					
						this.searchbuttom2=0;
						this.page2=this.limit2+1;
					},
					fail: () => {
						this.searchbuttom2=0;
					}
				})
			},
			getreply:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getreply_admin",
					method:"POST",
					data:{
						"limit":parseInt(this.limit1*10),
						"username":this.reply.username,
						"startime":this.reply.startime,
						"endtime":this.reply.endtime,
						"data":this.reply.data
					},
					success: (res) => {
						this.replylist=res.data.data;
						this.replynum=res.data.num;
					
						this.searchbuttom1=0;
						this.page1=this.limit1+1;
					},
					fail: () => {
						this.searchbuttom1=0;
					}
				})
			},
			newidentity:function(e){
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/newidentity",
					method:"POST",
					data:{
						"fenleiid":e,
						"identity":this.newidentitys
					},
					success(res) {
						if(res.data.code==1001){
							that.onshow();
							that.newidentitys="";
						}
					}
				})
			},
			deleteidentity:function(e1,e2){
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/deleteidentity",
					method:"POST",
					data:{
						"id":this.identity[this.fenleilist[e1].id][e2].id
					},
					success(res) {
						if(res.data.code==1001){
							that.onshow();
						}
					}
				})
			},
			newinterest:function(e){
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/newinterest",
					method:"POST",
					data:{
						"fenleiid":e,
						"interest":this.newinterests
					},
					success(res) {
						if(res.data.code==1001){
							that.onshow();
							that.newinterests="";
						}
					}
				})
			},
			deleteinterest:function(e1,e2){
				let that=this;
				uni.request({
					url:getApp().globalData.http+"/api/deleteinterest",
					method:"POST",
					data:{
						"id":this.interest[this.fenleilist[e1].id][e2].id
					},
					success(res) {
						if(res.data.code==1001){
							that.onshow();
						}
					}
				})
			},
			getidentity:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getidentity",
					method:"POST",
					data:{},
					success: (res) => {
						this.identity=res.data;
					
					}
				})
			},
			getinterest:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getinterest",
					method:"POST",
					data:{},
					success: (res) => {
						this.interest=res.data;
					
					}
				})
			},
			onshow:function(){
				this.getfenlei();
				this.getidentity();
				this.getinterest();
				let timeStamp = new Date();
				let year = new Date(timeStamp).getFullYear()
				let month = new Date(timeStamp).getMonth() + 1 < 10 ? '0' + (new Date(timeStamp).getMonth() + 1) : new Date(timeStamp).getMonth() + 1
				let date = new Date(timeStamp).getDate() < 10 ? '0' + new Date(timeStamp).getDate() : new Date(timeStamp).getDate()
				month-=2;
				if(month<=0){
					year-=1;
					month=12+(month-2);
				}
				if(month==2){
					if(date>28)date=28;
				}
				if(month==1||month==3||month==5||month==7||month==8||month==10||month==12);
				else{
					if(date==31)date=30;
				}
				this.mindate=year+"-"+month+"-"+date;
		
			},
			linkto:function(e){
				uni.navigateTo({
					url:"../index/theme?themeid="+parseInt(e)
				})
			},
			deletejing_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletejing_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			setjing_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/setjing_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			deletetop_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletetop_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			settop_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/settop_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			deletethemeall_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletethemeall_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			huifuthemeall_admin:function(){
				uni.request({
					url:getApp().globalData.http+"/api/huifuthemeall_admin",
					method:"POST",
					data:{
						"themelist":this.selecttheme
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							this.page--;
							this.search();
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			jugepagemax:function(){
				if(this.themenum%10==0){
					if(this.page*10==this.themenum)return false;return true;
				}
				else{
					if(this.page*10>=this.themenum)return false;return true;
				}
			},
			jugepagemax1:function(){
				if(this.replynum%10==0){
					if(this.page1*10==this.replynum)return false;return true;
				}
				else{
					if(this.page1*10>=this.replynum)return false;return true;
				}
			},
			jugepagemax2:function(){
				if(this.loucengreplynum%10==0){
					if(this.page2*10==this.loucengreplynum)return false;return true;
				}
				else{
					if(this.page2*10>=this.loucengreplynum)return false;return true;
				}
			},
			lastpage:function(){
				this.limit--;
				this.search();
			},
			nextpage:function(){
				this.limit++;
				this.search();
			},
			lastpage1:function(){
				this.limit1--;
				this.getreply();
			},
			nextpage1:function(){
				this.limit1++;
				this.getreply();
			},
			lastpage2:function(){
				this.limit2--;
				this.getloucengreply();
			},
			nextpage2:function(){
				this.limit2++;
				this.getloucengreply();
			},
			selectall1:function(){
				this.selectall=1;
				for(var i=0;i<this.themelist.length;i++)
				this.selecttheme.push(this.themelist[i].themeid);
			},
			deleteall:function(){
				this.selectall=0;
				this.selecttheme=[];
			},
			selectall_1:function(){
				this.selectall2=1;
				for(var i=0;i<this.replylist.length;i++)
				this.selectreply.push(this.replylist[i].id);
			},
			deleteall_1:function(){
				this.selectall2=0;
				this.selectreply=[];
			},
			addselecttheme:function(e){
				if(this.selecttheme.indexOf(e)==-1)this.selecttheme.push(e);
				else this.selecttheme.splice(this.selecttheme.indexOf(e),1);
				if(this.selecttheme.length==0)this.selectall=0;else this.selectall=1;
			},
			jugetheme:function(e){
				if(this.selecttheme.indexOf(e)!=-1)return true;return false;
			},
			addselectreply:function(e){
				if(this.selectreply.indexOf(e)==-1)this.selectreply.push(e);
				else this.selectreply.splice(this.selectreply.indexOf(e),1);
				if(this.selectreply.length==0)this.selectall2=0;else this.selectall2=1;
			},
			jugereply:function(e){
				if(this.selectreply.indexOf(e)!=-1)return true;return false;
			},
			time:function(value){
				let date = new Date(value*1000);
				let y = date.getFullYear();
				let MM = date.getMonth() + 1;
				MM = MM < 10 ? ('0' + MM) : MM;
				let d = date.getDate();
				d = d < 10 ? ('0' + d) : d;
				let h = date.getHours();
				h = h < 10 ? ('0' + h) : h;
				let m = date.getMinutes();
				m = m < 10 ? ('0' + m) : m;
				let s = date.getSeconds();
				s = s < 10 ? ('0' + s) : s;
				return y + '年' + MM + '月' + d + '日' + h + ':' + m + ':' + s;
			},
			tags:function(e){
				var temp=[];
				var str="";str+=e;
				temp=str.split(';');
				temp.pop();
				return temp;
			},
			deletetheme_admin:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/deletetheme_admin",
					method:"POST",
					data:{
						"themeid":parseInt(e)
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.themelist.length;i++){
								if(this.themelist[i].themeid==e){
									this.themelist[i].show1=0;
									break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			huifutheme_admin:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/huifutheme_admin",
					method:"POST",
					data:{
						"themeid":parseInt(e)
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[4],
								complete() {
								}
							})
							for(var i=0;i<this.themelist.length;i++){
								if(this.themelist[i].themeid==e){
									this.themelist[i].show1=1;
									break;
								}
							}
						}
						else{
							this.$refs.uToast.show({
								...this.upload[5],
								complete() {
								}
							})
						}
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[5],
							complete() {
							}
						})
					}
				})
			},
			
			search:function(){
				this.selecttheme=[];
				this.selectall=0;
				uni.request({
					url:getApp().globalData.http+"/api/searchtheme_admin",
					method:"POST",
					data:{
						"themeid":this.theme.themeid,
						"username":this.theme.username,
						"startime":this.theme.startime,
						"endtime":this.theme.endtime,
						"jing":this.theme.jing,
						"top":this.theme.top,
						"fenlei":this.theme.fenlei,
						"maxreply":this.theme.maxreply,
						"minreply":this.theme.minreply,
						"maxlook":this.theme.maxlook,
						"minlook":this.theme.minlook,
						"data":this.theme.data,
						"limit":parseInt((this.limit*10)),
					},
					success: (res) => {
						
						this.searchbuttom=0;
						this.themenum=res.data.num;
						this.page=this.limit+1;
						this.themelist=res.data.data;
						if(res.data.data.length==0)
							this.show7=true;
						else this.show7=false
					},
					fail: (res) => {
						this.searchbuttom=0;
					}
				})
			},
			changeminreply:function(e){
				
				if(this.minreply.length==0)this.theme.minreply=0;
				else
				this.theme.minreply=parseInt(this.minreply);
			},
			changemaxreply:function(e){
				if(this.maxreply.length==0)this.theme.maxreply=2100000000;
				else
				this.theme.maxreply=parseInt(this.maxreply);
			},
			changeminlook:function(e){
				
				if(this.minlook.length==0)this.theme.minlook=0;
				else
				this.theme.minlook=parseInt(this.minlook);
			},
			changemaxlook:function(e){
				if(this.maxlook.length==0)this.theme.maxlook=2100000000;
				else
				this.theme.maxlook=parseInt(this.maxlook);
			},
			date:function(e){
				this.show5=false;
				this.theme.startime=(new Date(e[0]).getTime()/1000)-28800;
				this.theme.endtime=parseInt(new Date(e[e.length-1]).getTime()/1000)+57600;
				this.startime=e[0];
				this.endtime=e[e.length-1];
			},
			date1:function(e){
				this.show8=false;
				this.reply.startime=(new Date(e[0]).getTime()/1000)-28800;
				this.reply.endtime=parseInt(new Date(e[e.length-1]).getTime()/1000)+57600;
				this.startime1=e[0];
				this.endtime1=e[e.length-1];
			},
			date2:function(e){
				this.show10=false;
				this.loucengreply.startime=(new Date(e[0]).getTime()/1000)-28800;
				this.loucengreply.endtime=parseInt(new Date(e[e.length-1]).getTime()/1000)+57600;
				this.startime2=e[0];
				this.endtime2=e[e.length-1];
			},
			choosetype:function(e){
				this.show4=false;
				this.c_sub1=e.indexs;
				this.theme.type=e.value[0];
				if(e.indexs==0){
					this.theme.jing='';
					this.theme.top='';
				}
				else if(e.indexs==1){
					this.theme.jing='1';
					this.theme.top='0';
				}
				else if(e.indexs==2){
					this.theme.jing='0';
					this.theme.top='1';
				}
			},
			choosetag:function(e){
				this.show3=false;
				this.c_sub=e.indexs;
				this.theme.fenlei=e.value[0];
			},
			deletefenlei:function(){
				uni.request({
					url:getApp().globalData.http+"/api/deletefenlei",
					method:"POST",
					data:{
						"id":this.temp_item.id,
						"fenlei":this.temp_item.fenlei
					},
					success: (res) => {
						if(res.data.code==1001){
							this.getfenlei();
							this.$refs.uToast.show({
								...this.upload[2],
								complete() {
								}
							})
						}
						else
						this.$refs.uToast.show({
							...this.upload[3],
							complete() {
							}
						})
						this.show2=false;
					},
					fail: () => {
						this.show2=false;
						this.$refs.uToast.show({
							...this.upload[3],
							complete() {
							}
						})
					}
				})
			},
			getfenlei:function(){
				var temp=[];
				uni.request({
					url:getApp().globalData.http+"/api/getfenleis",
					method:"POST",
					data:{
					},
					success: (res) => {
						this.fenleilist=[];
						this.columns=[];
						for(var i=0;i<res.data.length;i++){
						this.fenleilist.push(res.data[i]);
						temp.push(res.data[i].fenlei);
						var sub=res.data[i].id;
						this.show9.push(false);
						}
						this.columns.push(temp);
					},
				})
			},
			addfenlei:function(){
				if(this.newfenlei.length<1){
					return ;
				}
				if(this.fenleilist.indexOf(this.newfenlei)!=-1){
					return ;
				}
				uni.request({
					url:getApp().globalData.http+"/api/newfenlei",
					method:"POST",
					data:{
						"fenlei":this.newfenlei
					},
					success: (res) => {
						if(res.data.code==1001){
							this.getfenlei();
							this.newfenlei="";
							this.$refs.uToast.show({
								...this.upload[0],
								complete() {
								}
							})
						}
						else
						this.$refs.uToast.show({
							...this.upload[1],
							complete() {
							}
						})
					},
					fail: () => {
						this.$refs.uToast.show({
							...this.upload[1],
							complete() {
							}
						})
					}
				})
			},
		}
	}
</script>
<style>
button::after{border: initial;}
.newfenlei{
	font-size: 14px;
	color:#2767a7
}
.newfenlei:hover{
	font-size: 14px;
	color:#3998f7
}
.choose{
	color:#3998f7
}
.t1{
	height:90vh;
}
.t2{
	height:auto;
}
.changecolor{
	color:#2767a7
}
.changecolor:hover{
	color:#3998f7
}
</style>