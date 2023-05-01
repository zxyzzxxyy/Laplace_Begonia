<template>
	<view style="background-color: #ffffff;font-size: 16px;display: flex;flex-direction: row;">
		<view style="width:20rem;;height:auto;box-shadow: 0 2PX 12PX 0 rgb(0 0 0 / 6%);background-color: white;padding:1rem">
			<view>
				<text style="font-size: 20px;font-weight: 700;">用 户</text>
				<u-divider ></u-divider>
				<view style="display: flex;flex-direction: row;color:#8bc863" v-if="choose1==0">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>用户管理</text>
				</view>
				<view style="display: flex;flex-direction: row;" v-if="choose1!=0" @click="choose1=0">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>用户管理</text>
				</view>
				<view style="display: flex;flex-direction: row;color:#8bc863;margin-top: 1rem;" v-if="choose1==1">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>系统消息</text>
				</view>
				<view style="display: flex;flex-direction: row;margin-top: 1rem;" v-if="choose1!=1" @click="choose1=1,getall_systemmessage()">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>系统消息</text>
				</view>
				<view style="display: flex;flex-direction: row;color:#8bc863;margin-top: 1rem;" v-if="choose1==3">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>头像框</text>
				</view>
				<view style="display: flex;flex-direction: row;margin-top: 1rem;" v-if="choose1!=3" @click="choose1=3,getheadborderlist()">
					<u-icon name="account" size="18" style="margin-right: 0.2rem;"></u-icon>
					<text>头像框</text>
				</view>
			</view>
		</view>
		<!--头像框-->
		<view style="padding:1rem;width:100%" v-show="choose1==3">
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px;">头像框</text>
				<u-divider style="width: 100%;" ></u-divider>
				
				<view style="height:300px;width:100%;display: flex;align-items: center;justify-content: center;">
					<view>
					<userhead :type="1" style=" transform: scale(3);;width:100px;height:100px;margin-right:300px" :size="'100'" username="畅言" userheadimage="https://txtzz-1301452902.file.myqcloud.com/RqH32RQWblpBddrwujizvGJLy.jpeg" :headborder="headborder" :pianyi="pianyi"></userhead>
					</view>
					<view>
					<userhead  style=" transform: scale(3);;width:40px;height:40px" :size="'40'" username="畅言" userheadimage="https://txtzz-1301452902.file.myqcloud.com/RqH32RQWblpBddrwujizvGJLy.jpeg" :headborder="headborder" :pianyi="pianyi"></userhead>
					</view>
					
				</view>
				<u-divider style="width: 100%;margin-top:100px"></u-divider>
				<view v-if="headborder!=''" style="display: flex;flex-direction: row;flex-wrap: wrap;">
					<view style="display: flex;flex-direction: row;align-items: center;">
						左图缩放
						<u-number-box style="margin-left: 10px;":min="-100"v-model="pianyi.pianyi1" @change="valChange"></u-number-box>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 10px;">
						左图左移
						<u-number-box style="margin-left: 10px;" :min="-100"v-model="pianyi.left" @change="valChange"></u-number-box>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 10px;">
						左图上移
						<u-number-box style="margin-left: 10px;":min="-100"v-model="pianyi.top" @change="valChange"></u-number-box>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 10px;">
						右图缩放
						<u-number-box style="margin-left: 10px;" :min="-100"v-model="pianyi.pianyi" @change="valChange"></u-number-box>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 10px;">
						右图左移
						<u-number-box style="margin-left: 10px;" :min="-100"v-model="pianyi.left1" @change="valChange"></u-number-box>
					</view>
					<view style="display: flex;flex-direction: row;align-items: center;margin-left: 10px;">
						右图上移
						<u-number-box style="margin-left: 10px;"  :min="-100"v-model="pianyi.top1" @change="valChange"></u-number-box>
					</view>
					<view style="margin-left:10px;">
						<u-button type="primary" style="height:30px" text="保存"
						@click="saveheadborder()"
						></u-button>
					</view>
				</view>
				<u-divider style="width: 100%;" v-if="headborder!=''"></u-divider>
				<view style="display: flex;flex-direction: row;flex-wrap: wrap;align-items: center;width:80vw
				">
				<view v-for="(item,index) in headborderlist" @click="setheadborder(item)">
					<userhead  
					
					style=" margin-bottom:10px;width:100px;height:100px;margin-left:20px;margin-right:20px;border:1px solid #cfd4dc;padding:40px;border-radius: 15px;" 
					:type="1"
					:size="'100'" username="畅言" 
					userheadimage="https://txtzz-1301452902.file.myqcloud.com/RqH32RQWblpBddrwujizvGJLy.jpeg" 
					:headborder="item.url" 
					:pianyi="item"
					
					></userhead>
				</view>
				<view style=" ;width:100px;height:100px;margin-left:20px;margin-right:20px;border:1px solid #cfd4dc;padding:40px;border-radius: 15px;
				display: flex;align-items: center;justify-content: center;"
				 @click="newheadborder()">			
					<view style="font-size: 20px;color:#909399;">
						新增
					</view>
				</view>
				</view>
			</view>
		</view>
		<!--系统消息-->
		<view style="padding:1rem;width:100%" v-show="choose1==1">
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px;">系统消息</text>
				<u-divider style="width: 100%;" ></u-divider>
				<view>
					<text>发布系统消息</text>
					<view style="display: flex;flex-direction:column">
						<view style="display: flex;flex-direction: row;width:auto;align-items: center;margin-right: 5rem;margin-top: 1rem;">
							<text>通知用户ID(0为全体用户):</text>
							<input  style="margin-left: 2rem;width:10rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;margin-right:2rem" v-model="messageuserid"/>
							<text>网址链接:</text>
							<input  style="margin-left: 2rem;width:10rem;height:2.3rem;border:1px solid #d8d8d8;border-radius: 3px;padding-left: 10px;" v-model="src"/>
							<view style="width:5rem;margin-left:1rem">
							<u-button type="primary" text="发布" size="18"
							:disabled="messageuserid==''||message==''"
							@click="newsystemmessage()"
							></u-button>
							</view>
						</view>
						<view style="display: flex;flex-direction: row;width:auto;margin-right: 5rem;margin-top: 1rem;">
							<text>通知内容:</text>
							
							<textarea v-model="message" placeholder="" style="background-color: white;width:50rem;height:9rem;padding-left:1%;padding-right:1%;padding-top: 0.5rem;padding-bottom:0.5rem;border-radius: 5px;border:1px solid #d8d8d8;margin-left:1rem" maxlength="1500" ></textarea>
							<view style="width:10rem;height:10rem;border-radius: 5px;background-color: #f4f4f5;display: flex;flex-direction: column;align-items: center;justify-content: center;margin-left:2rem" @click="uploadimage()" v-if="messageimage==''">
								<u-icon name="plus" color="#909399" size="28"></u-icon>
								<text style="color:#909399">上传图片</text>
							</view>
							<u--image :src="messageimage" style="margin-right:1rem;margin-left:1rem"  
							height="10rem"
							v-if="messageimage!=''"></u--image>
							<text style="color:#8bc863" v-if="messageimage!=''" @click="messageimage=''">删除</text>
						</view>
					</view>
				</view>
				<u-divider style="width: 100%;" ></u-divider>
				<view v-for="(item,index) in systemmessage">
					<view style="display: flex;flex-direction:row">
						<u-avatar mode="aspectFill" :src="item.systemheadimage" size="45"></u-avatar>
						<view style="display: flex;flex-direction:column;margin-left:5px">
							<text>{{item.systemname}}</text>
							<text style="color:#909399">发布于:{{times(item.createtime)}}</text>
							<view style="margin-top: 20px;display:flex;flex-direction:column">
								<text v-if="item.userid!=0">通知用户UID : {{item.userid}}</text>
								<text v-if="item.userid==0">全体用户</text><br>
								<text>内容 : {{item.message}}</text><br>
								<text v-if="item.src!=''">网址链接 : {{item.src}}</text>
								<text v-if="item.src==''">网址链接 : 无</text>
								<br>
								<u--image :src="item.images" style="margin-right:1rem;margin-left:1rem" height="10rem" v-if="item.images!=''"></u--image><br v-if="item.images!=''">
								<view style="display: flex;flex-direction:row">
								<text>状态 : </text><text style="opacity:0">1</text><text v-if="item.show1==1" style="color:#5ac725"> 正常</text><text v-if="item.show1==0" style="color:#f56c6c">已删除</text>
								</view><br>
								<text v-if="item.show1==1" style="color:#8bc863" @click="deletemessage(item.id)">删除</text>
								<text v-if="item.show1==0" style="color:#8bc863" @click="replymessage(item.id)">恢复</text>
							</view>
						</view>
					</view>
					
					<u-divider style="width: 100%;" ></u-divider>
				</view>
			</view>
		</view>
		<!--搜索页-->
		<view style="padding:1rem;width:100%" v-show="show==0&choose1==0">
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px;">用户管理</text>
				<u-divider style="width: 100%;" ></u-divider>
				<view>
					<text>用户名</text>
					<input v-model="username "style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;"/>
				</view>
				<view style="margin-top: 1rem;">
					<text>用户UID</text>
					<input v-model="userid" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
				</view>
				<view style="margin-top: 1rem;">
					<text>用户邮箱</text>
					<input v-model="email" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
				</view>
				<view style="width:5rem;margin-top: 2rem;">
					<u-button v-show="buttom==0" type="primary" text="搜索" size="18"
					@click="buttom=1,search()"
					></u-button>
					<u-button v-show="buttom==1" type="primary" loading loadingText="搜索中" size="18"
					
					></u-button>
				</view>
			</view>
		</view>
		<!--搜索结果-->
		<view style="padding:1rem;width:100%;height:80vh;overflow-y: hidden;" v-show="show==1&choose1==0">
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px">用户管理</text>
				<u-divider style="width: 100%;" ></u-divider>
				<text style="font-size: 14px;">用户搜索结果</text>
				<view style="display: flex;">
					<text style="font-size: 14px;margin-top: 1rem;">共找到{{usermessage.length}}名符合条件的用户</text>
					<button style="border: initial;background-color: white;margin:0;font-size: 14px;padding-top:0.5rem;" class="changecolor" @click="research()">
						重新搜索
					</button>
				</view>
				<view style="display: flex;flex-direction: row;margin-top: 2rem;">
					<view style="width: 8rem;">
						<u-checkbox-group v-model="checked" placement="row">
							<u-checkbox  @change="con1()" name="1"></u-checkbox>
						</u-checkbox-group>
					</view>
					<view style="width:10rem">
						<text>UID</text>
					</view>
					<view style="width:10rem">
						<text>头像</text>
					</view>
					<view style="width:10rem">
						<text>用户名</text>
					</view>
					<view style="width:10rem">
						<text>主题数</text>
					</view>
					<view style="width:10em">
						<text>用户组</text>
					</view>
					<view style="width:10em;margin-left: auto;text-align: center;;margin-right: 10rem;">
						<text>状态</text>
					</view>
				</view>
				<u-divider ></u-divider>
				<view style="overflow-y: auto;height:80vh">
				<view v-for="(item,index) in usermessage">
					<view style="display: flex;flex-direction: row;align-items: center;">
						<view style="width: 8rem;">
							<u-checkbox-group  placement="row" :value="select" >
								<u-checkbox  :name="item.userid" @change="con(item.userid)"
								></u-checkbox>
							</u-checkbox-group>
						</view>
						<view style="width:10rem">
							<text>{{item.userid}}</text>
						</view>
						<view style="width:10rem">
							<u-avatar mode="aspectFill" :src="item.userheadimage" v-if="item.userheadimage!=null" size="45"></u-avatar>
							<u-avatar :text="item.username.substr(0,1)"  randomBgColor v-if="item.userheadimage==null" size="45"></u-avatar>
						</view>
						<view style="width:10rem">
							<u--text :lines="1" :text="item.username"></u--text>
						</view>
						<view style="width:10rem">
							<text>{{item.themenum}}</text>
						</view>
						<view style="width:10em">
							<text v-show="item.admin==0">普通用户</text>
							<text v-show="item.admin==1">管理员</text>
						</view>
						<view style="width:10rem;margin-left: auto;margin-right: 10rem;" v-if="item.ban==0">
							<button style="border: initial;background-color: white;margin:0;font-size: 14px;padding-top:0.5rem;" class="changecolor" @click="more(item)">
								详情
							</button>
							<!--
							<button style="border: initial;background-color: white;margin:0;font-size: 14px;padding-top:0.5rem;" class="changecolor" @click="ban(item.userid,1,index)">
								禁用
							</button>
							-->
						</view>
						<view style="width:10rem;margin-left: auto;margin-right: 10rem;" v-if="item.ban==1">
							<button style="border: initial;background-color: white;margin:0;font-size: 14px;padding-top:0.5rem;" class="changecolor"  @click="more(item)">
								详情
							</button>
							<button style="border: initial;background-color: white;margin:0;font-size: 14px;padding-top:0.5rem;" class="changecolor"  @click="ban(item.userid,0,index)">
								恢复
							</button>
						</view>
					</view>
					<u-divider ></u-divider>
				</view>
				<view style="height:30vh">
					
				</view>
				</view>
			</view>
		</view>
		<!--个人详情-->
		<view style="padding:1rem;width:100%;height:80vh;overflow-y: hidden" v-show="show==2&choose1==0">
			<view style="display: flex;padding-left: 1rem;flex-direction: column;">
				<text style=";font-size: 20px">用户管理</text>
				<u-divider style="width: 100%;" ></u-divider>
				<view style="height:80vh;overflow-y: auto;width:auto"> 
					<text>{{user.username}} (UID: {{user.userid}})</text>
					<view style="width:20rem;margin-bottom: 2rem;display: flex;flex-direction:column;">
						<image :src="user.userheadimage" style="width: 8rem;height:8rem;border-radius: 5px;margin-top: 2rem;margin-bottom: 5px;border: 1px dashed #c8c9cc" mode="aspectFill"
						v-if="user.userheadimage!=null"
						></image>
						<view  style="width: 8rem;height:8rem;border-radius: 5px;margin-top: 2rem;margin-bottom: 5px;border: 1px dashed #c8c9cc;display: flex;align-items: center;justify-content: center;" mode="aspectFill" @click="head()"
						v-if="user.userheadimage==null"
						>
						<u-icon name="plus" size="50"></u-icon>
						</view>
						<text v-if="user.userheadimage!=null" style="color:#8bc863" @click="deletehead()">删除头像</text>
					</view>
					<view>
						<text>用户名</text>
						<input v-model="user.username "style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;"/>
					</view>
					<view style="margin-top: 1rem;">
						<text>用户邮箱</text>
						<input v-model="user.email" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
					</view>
					<view style="margin-top: 1rem;">
						<text>签名</text>
						<input v-model="user.sign" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
					</view>
					<view style="margin-top: 1rem;">
						<text>新密码</text>
						<input v-model="password" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
					</view>
					<view style="margin-top: 1rem;">
						<text>确认新密码</text>
						<input v-model="repassword" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
					</view>
					<view style="margin-top: 1rem;">
						<text>用户角色</text>
						
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
					</view>
					<view style="margin-top: 1rem;">
						<text>是否被禁用</text>
						
						<u-checkbox-group
						            v-model="checkboxValue2"
						            placement="row"
						        >
						            <u-checkbox
									style="margin-right: 0.5rem;margin-top: 0.5rem;"
						                :customStyle="{marginBottom: '8px'}"
						                v-for="(item, index) in checkboxList2"
						                :key="index"
						                :label="item.name"
						                :name="item.name"
						                :checked="item.checked"
										@change="checkboxChange1(index)"
						            >
						            </u-checkbox>
						        </u-checkbox-group>
					</view>
					<view style="margin-top: 1rem;display: flex;flex-direction: column;"
					v-if="user.ban==1"
					>
						<text>禁用原因:</text><br>
						<input v-model="banreason" style="width:50rem;height:2.5rem;border:1px solid #d8d8d8;margin-top: 1rem;border-radius: 3px;padding-left: 10px;" />
					</view>
					<view style="margin-top: 1rem;display: flex;flex-direction: column;"
						v-if="user.ban==1"
					>
						<text>禁用时间:</text><br>
						<u-checkbox-group
						            v-model="checkboxValue1"
						            placement="row"
						        >
						            <u-checkbox
									style="margin-right: 0.5rem;margin-top: 0.5rem;"
						                :customStyle="{marginBottom: '8px'}"
						                v-for="(item, index) in checkboxList3"
						                :key="index"
						                :label="item.name"
						                :name="item.name"
						                :checked="item.checked"
										@change="checkboxChange2(index)"
						            >
						            </u-checkbox>
						        </u-checkbox-group>
						<text v-if="bantime1!=0">禁用至:{{time1()}}</text>
					</view>
								
					<view style="margin-top: 1rem;display: flex;flex-direction: column;">
						<text>注册日期:</text><br>
						<text>{{times(user.registertime)}}</text>
					</view>
					<view style="margin-top: 1rem;display: flex;flex-direction: column;">
						<text>头像框:</text><br>
						<view>
							<text>已拥有</text><br>
							<view style="margin-top:10px;display: flex;flex-direction:row;flex-wrap: wrap;border:1px solid #cfd4dc;border-radius: 15px;min-height:150px;padding:10px">
							<view v-if="user.userheadborderlist.list1!=undefined" v-for="(item,index) in user.userheadborderlist.list1" 
							@click="deleteuserheadborder(item)"	
							>
								<userhead  
							
								style=" ;width:100px;height:100px;margin-left:20px;margin-right:20px;border:1px solid #cfd4dc;padding:40px;border-radius: 15px;margin-bottom:10px" 
								:type="1"
								:size="'100'" username="畅言" 
								userheadimage="https://txtzz-1301452902.file.myqcloud.com/RqH32RQWblpBddrwujizvGJLy.jpeg" 
								:headborder="item.url" 
								:pianyi="item"
								
								></userhead>
							</view>
							</view>
						</view>
						<view>
							<text>未拥有</text><br>
							<view style="margin-top:10px;display: flex;flex-direction:row;flex-wrap: wrap;border:1px solid #cfd4dc;border-radius: 15px;padding:10px;min-height:150px">
								<view  v-if="user.userheadborderlist.list2!=undefined" v-for="(item,index) in user.userheadborderlist.list2"
								@click="adduserheadborder(item)"
								>
									<userhead  
									
									style=" ;width:100px;height:100px;margin-left:20px;margin-right:20px;border:1px solid #cfd4dc;padding:40px;border-radius: 15px;margin-bottom:10px" 
									:type="1"
									:size="'100'" username="畅言" 
									userheadimage="https://txtzz-1301452902.file.myqcloud.com/RqH32RQWblpBddrwujizvGJLy.jpeg" 
									:headborder="item.url" 
									:pianyi="item"
									
									></userhead>
								</view>
							</view>
						</view>
					</view>
					<view style="margin-top: 1rem;display: flex;flex-direction: column;">
						<text>上次登录日期:</text><br>
						<text>{{times(user.lastlogintime)}}</text>
					</view>
					<view style="display: flex;flex-direction: row;margin-top: 2rem;">
						<view style="width:5rem;margin-right: 3rem;">
							<u-button v-show="buttom==0" type="primary" text="保存" size="18"
							@click="save()"
							></u-button>		
						</view>
						<view style="width:5rem;">
							<u-button v-show="buttom==0" type="primary" text="返回" size="18"
							@click="show=1"
							></u-button>		
						</view>
					</view>
					<view style="height:10vh">
					</view>
				</view>
			</view>
		</view>
		<view>
				<u-toast ref="uToast"></u-toast>

		</view>
	</view>
</template>

<script>
	import md5 from "@/pages/md5.js"
	import userhead from "@/pages/index/../userhead/userhead.vue"
	export default {
		components:{
			userhead
		},
		data() {
			return {
				messageimage:"",
				message:"",
				messageuserid:"",
				username:"",
				userid:"",
				email:"",
				buttom:0,
				src:"",
				password:"",
				repassword:"",
				show:0,
				num:0,
				checked:[],
				select:[],
				banreason:"",
				usermessage:[],
				choose1:0,
				headborder:"",
				pianyi:{
					pianyi:"",
					pianyi1:"",
					left:0,
					left1:0,
					top:0,
					top1:0,
				},
				choose:[],
				user:{
					userheadborderlist:{
						list1:{
							
						},list2:{
							
						}
					}
				},
				bantime:-1,
				headborderid:0,
				bantime1:0,
				ban_sub:-1,
				headborderlist:[],
				checkboxValue1:[],
				checkboxValue2:[],
				checkboxList1: [{
				                    name: '普通用户',
				                    checked: false
				                },
				                {
				                    name: '管理员',
				                    checked: false
				                },
				            ],
				checkboxList2: [{
				                    name: '否',
				                    checked: false
				                },
				                {
				                    name: '是',
				                    checked: false
				                },
				            ],
				checkboxList3:[
					{
						name:'7天',
						checked:false,
					},
					{
						name:'30天',
						checked:false,
					},
					{
						name:'180天',
						checked:false,
					},
					{
						name:'1年',
						checked:false,
					},
					{
						name:'永久',
						checked:false,
					},
				],
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
						message: "两次输入的密码不一致",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message:"邮箱格式错误",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message:"密码长度小于七位",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
					{
						type: 'error',
						icon: false,
						title: '成功主题',
						message:"请设置禁用时间与原因",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/error.png',
					},
				],
				systemmessage:[],
			}
		},
		onShow(){
			uni.reLaunch({
				url:"../index/index"
			})
			this.getheadborderlist();
		},
		methods: {
			deleteuserheadborder(e){
				uni.request({
					url:getApp().globalData.http+"/api/deleteuserheadborder",
					method:"POST",
					data:{
						userid:this.user.userid,
						headborderid:e.id
					},success: (res) => {
						if(res.data.code==1001){
					
							for(var i=0;i<this.user.userheadborderlist.list1.length;i++){
								
								if(this.user.userheadborderlist.list1[i].id==e.id){
									this.user.userheadborderlist.list1.splice(i,1);
									this.user.userheadborderlist.list2.push(e);
									break;
								}
							}
						}
					}
				})
			},
			adduserheadborder(e){
				uni.request({
					url:getApp().globalData.http+"/api/adduserheadborder",
					method:"POST",
					data:{
						userid:this.user.userid,
						headborderid:e.id
					},success: (res) => {
						if(res.data.code==1001){
							for(var i=0;i<this.user.userheadborderlist.list2.length;i++){
								if(this.user.userheadborderlist.list2[i].id==e.id){
									this.user.userheadborderlist.list2.splice(i,1);
									this.user.userheadborderlist.list1.push(e);
									break;
								}
							}
						}
					}
				})
			},
			newheadborder(){
				let that= this;
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
								let url=res.data.location
								uni.request({
									url:getApp().globalData.http+"/api/newheadborder",
									method:"POST",
									data:{
										url:url
									},
									success: (res1) => {
										that.getheadborderlist();
									},
									fail:(res)=>{
										
									}
								})
							}
						})
					}
				});
			},
			saveheadborder(){
				uni.request({
					url:getApp().globalData.http+"/api/updateheadborder",
					method:"POST",
					data:{
						id:this.headborderid,
						pianyi:this.pianyi.pianyi,
						pianyi1:this.pianyi.pianyi1,
						top:this.pianyi.top,
						top1:this.pianyi.top1,
						left:this.pianyi.left,
						left1:this.pianyi.left1
					},
					success: (res) => {
						if(res.data.code==1001){
							this.getheadborderlist()
						}
					}
				})
			},
			valChange:function(e){
				console.log(this.pianyi);
			},
			setheadborder:function(e){
				this.headborderid=e.id;
				this.pianyi={
					"pianyi":e.pianyi,
					"pianyi1":e.pianyi1,
					"top":e.top,
					"top1":e.top1,
					"left":e.left,
					"left1":e.left1
				}
				this.headborder=e.url
			},
			getheadborderlist:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getheadborderlist",
					method:"POST",
					data:{
						
					},
					success: (res) => {
						this.headborderlist=res.data;
					}
				})
			},
			newsystemmessage:function(){
				uni.request({
				url:getApp().globalData.http+"/api/newsystemmessage",
				method:"POST",
				data:{
					"userid":parseInt(this.messageuserid),
					"cookie":getApp().globalData.cookie,
					"message":this.message,
					"src":this.src,
					"images":this.messageimage,
					
				},
				success:(res)=>{
					if(res.data.code==1001){
						this.messageimage="";
						this.messageuserid="";
						this.message="";
						this.src="";
						this.systemmessage=[];
						this.getall_systemmessage();
					}
				},
				fail:(res)=>{
					
				}
			})
			},
			deletemessage:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/deletesystemmessage",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						cookie:getApp().globalData.cookie,
						id:e
					},
					success:(res)=>{
						if(res.data.code==1001){
							this.systemmessage=[];
							this.getall_systemmessage();
						}
					},
					fail:(res)=>{
						
					}
				})
			},
			replymessage:function(e){
				uni.request({
					url:getApp().globalData.http+"/api/replysystemmessage",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						cookie:getApp().globalData.cookie,
						id:e
					},
					success:(res)=>{
						if(res.data.code==1001){
							this.systemmessage=[];
							this.getall_systemmessage();
						}
					},
					fail:(res)=>{
						
					}
				})
			},
			getall_systemmessage:function(){
				
				uni.request({
					url:getApp().globalData.http+"/api/getall_systemmessage",
					method:"POST",
					data:{
						userid:getApp().globalData.userid,
						cookie:getApp().globalData.cookie
					},
					success:(res)=>{
						this.systemmessage=res.data;
					},
					fail:(res)=>{
						
					},
				})
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
								that.messageimage=JSON.parse(res.data).location;
							}
						})
					}
				});
			},
			ban:function(userid,ban,index){
				uni.request({
					url:getApp().globalData.http+"/api/ban",
					method:"POST",
					data:{
						userid:userid,
						ban:ban
					},
					success: (res) => {
						if(res.data.code==1001){
							this.usermessage[index].ban=ban
							this.$refs.uToast.show({
								...this.upload[0],
								complete() {
									
								}
							})
						}
						else this.$refs.uToast.show({
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
			save:function(){
				if(!uni.$u.test.email(this.user.email)){
					this.$refs.uToast.show({
						...this.upload[3],
						complete() {
							
						}
					})
					return;
				}
				if(this.password!=''){
					if(this.password!=this.repassword){
						this.$refs.uToast.show({
							...this.upload[2],
							complete() {
								
							}
						})
						return;
					}
					else if(this.password.length<7){
						this.$refs.uToast.show({
							...this.upload[4],
							complete() {
								
							}
						})
						return;
					}
				
					else{
						this.$set(this.user, 'password', this.password);
					}
				}else this.$set(this.user, 'password', '');
				if(this.user.ban==1&&(this.bantime==-1||this.banreason.length==0)){
					this.$refs.uToast.show({
						...this.upload[5],
						complete() {
							
						}
					})
					return;
				}
				var password = "";
				if(this.user.password.length>0)
					password = md5.hex_md5(this.user.password);
				
				if(this.bantime>=0){
				
				var time=[3600*24*7,3600*24*30,3600*24*180,3600*24*365,3600*24*3650];
				var g=new Date().getTime();
				this.bantime=g+time[this.bantime]*1000;
				}else this.bantime=-1;
				uni.request({
					url:getApp().globalData.http+"/api/changeusermessage",
					method:"POST",
					data:{
						"userid":this.user.userid,
						"username":this.user.username,
						"userheadimage":this.user.userheadimage,
						"sign":this.user.sign,
						"email":this.user.email,
						"password":password,
						"admin":this.user.admin,
						"ban":this.user.ban,
						"banreason":this.banreason,
						"bantime":this.bantime/1000,
						"bansub":this.ban_sub
					},
					success: (res) => {
						if(res.data.code==1001){
							this.$refs.uToast.show({
								...this.upload[0],
								complete() {
									
								}
							})
						}
						else{
							this.$refs.uToast.show({
								...this.upload[1],
								complete() {
									
								}
							})
						}
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
			deletehead:function(){
				this.user.userheadimage=null;
				
			},
			head:function(){
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
								that.$refs.uToast.show({
									...that.upload[0],
									complete() {
										
									}
								})
								res.data = JSON.parse(res.data)
								that.user.userheadimage=res.data.location;
							},
							fail: () => {
								this.$refs.uToast.show({
									...this.upload[1],
									complete() {
										
									}
								})
							}
						})
					}
				});
			},
			checkboxChange2(e){
				for(var i=0;i<this.checkboxList3.length;i++){
					if(i==e){
						this.checkboxList3[i].checked=true;
						this.bantime=e;
						this.ban_sub=e;
					}
					else this.checkboxList3[i].checked=false;
				}
			},
			checkboxChange1(e){
				if(e==0){
					this.checkboxValue1=["否"];
					this.checkboxList2[0].checked=true;
					this.checkboxList2[1].checked=false;
					this.user.ban=0;
				}
				else if(e==1){
					this.checkboxValue1=["是"];
					this.checkboxList2[1].checked=true;
					this.checkboxList2[0].checked=false;
					this.user.ban=1;
				}
			},
			checkboxChange(e){
				if(e==0){
					this.checkboxValue1=["普通会员"];
					this.checkboxList1[0].checked=true;
					this.checkboxList1[1].checked=false;
					this.user.admin=0;
				}
				else if(e==1){
					this.checkboxValue1=["管理员"];
					this.checkboxList1[1].checked=true;
					this.checkboxList1[0].checked=false;
					this.user.admin=1;
				}
			},
			jugeadmin(e){
				return true;
				if(e==0&&this.user.admin==0)return true;
				else if(e==0&&this.user.admin==1)return false;
				else if(e==1&&this.user.admin==0)return false;
				else if(e==1&&this.user.admin==1)return true;
			},
			times:function(e){
				var now = new Date(e * 1000); // 依情况进行更改 * 1
				var y = now.getFullYear();
				var m = now.getMonth() + 1;
				var d = now.getDate();
				        return y + "-" + (m < 10 ? "0" + m : m) + "-" + (d < 10 ? "0" + d : d) + " " + now.toTimeString().substr(0, 8);
			},
			time1(){
				if(this.bantime1!=0){
					var now = new Date(this.bantime1 * 1000); // 依情况进行更改 * 1
						var y = now.getFullYear();
						var m = now.getMonth() + 1;
						var d = now.getDate();
						        return y + "-" + (m < 10 ? "0" + m : m) + "-" + (d < 10 ? "0" + d : d) + " " + now.toTimeString().substr(0, 8);
					
				}
			},
			more:function(e){
				this.user=e;
				this.banreason=this.user.banreason;
				this.bantime=this.user.ban_sub;
				this.bantime1=this.user.ban_time;
				if(this.bantime!=-1){
					console.log(this.bantime);
					console.log(this.checkboxList3);
					for(var i=0;i<this.checkboxList3.length;i++)
						this.checkboxList3[i].checked=false;
					this.checkboxList3[this.bantime].checked=true;
				}
				this.show=2;
				if(this.user.admin==0){
					this.checkboxList1[0].checked=true;
					this.checkboxList1[1].checked=false;
				}
				else{
					this.checkboxList1[0].checked=false;
					this.checkboxList1[1].checked=true;
				}
				if(this.user.ban==0){
					this.checkboxList2[0].checked=true;
					this.checkboxList2[1].checked=false;
				}
				else{
					this.checkboxList2[0].checked=false;
					this.checkboxList2[1].checked=true;
				}
				
			},
			con:function(e){
				if(this.select.indexOf(e)==-1)
				this.select.push(e);
				else this.select.splice(this.select.indexOf(e),1);
				if(this.select.length>0&&this.checked.length==0)this.checked.push("1");
				else if(this.select.length==0)this.checked.splice(0,1);
			},
			con1:function(e){
				if(this.select.length!=0)
					this.select.splice(0,this.select.length);
				else for(var i=0;i<this.usermessage.length;i++)
					this.select.push(this.usermessage[i].userid);
			},
			research:function(){
				this.show=0;
				this.username="";
				this.userid="";
				this.email="";
			},
			search:function(){
				uni.request({
					url:getApp().globalData.http+"/api/getusermessage",
					method:"POST",
					data:{
						"userid":this.userid,
						"username":this.username,
						"email":this.email
					},
					success:(res)=>{
						this.usermessage=res.data.data;
						this.buttom=0;
						this.show=1;
						for(var i=0;i<this.usermessage.length;i++)
							this.choose.push(false);
						
					}
				})
			},
		}
	}
</script>

<style>
button::after{border: initial;}
.changecolor{
	color:#2767a7
}
.changecolor:hover{
	color:#3998f7
}
</style>
