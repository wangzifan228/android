<template>
	<view class="content">
		<view class="btn-row" v-show="nshow==0">
			<button v-if="hasLogin" type="default" @tap="nshow=1" style="margin-bottom: 15px;">个人信息</button>
			<button v-if="hasLogin" type="default" @tap="nshow=2" style="margin-bottom: 15px;">抑郁自测</button>
			<button v-if="!hasLogin" type="primary" class="primary" @tap="bindLogin">登录</button>
			<button v-if="hasLogin" type="default" @tap="bindLogout">退出登录</button>
		</view>
		<view v-show="nshow==1">
			<view class="input-row border">
				<text class="title">账号：</text>
				<input type="text" focus clearable v-model="account" placeholder="请输入账号"></input>
			</view>
			<view class="input-row border">
				<text class="title">密码：</text>
				<input disabled="disabled" type="password" displayable v-model="password" placeholder="请输入密码"></input>
			</view>
			<view class="input-row">
				<text class="title">邮箱：</text>
				<input type="text" clearable v-model="email" placeholder="请输入邮箱"></input>
			</view>
			            <button @click="xiugai()" type="primary" >修改</button>

		</view>
		<view>
			<button v-if="nshow!=0" type="default" @tap="nshow=0" style="margin-bottom: 15px;position: fixed;bottom: 60px;right: 15px;z-index: 9999;">返回个人中心</button>
			<view v-if="nshow==2" class="container">
				<block v-for="(item,index) in list" :key="index">
					<view class="box1">
						<view class="box1-1">
							<view class="box1-1-1">
								抑郁自测
							</view>
							
						</view>
						
					</view>
					<view class="box2">
						<view style="margin: 10px 10px;" class="box2-1" v-for="(tms,index1) in item.tmList" :key="tms.tmid">
							<view class="box2-1-1">
								{{index1+1}}.{{tms.tmmc}}<text>({{tms.tmlx}})</text>
							</view>
							<view class="box2-1-2">
								<block v-for="(xxs,idx3) in tms.xxList" :key="xxs.value">
									<view class="uni-list-cell xz" v-if="tms.tmda==xxs.value" @click="radioChange(1,tms.tmid,item.courseId,xxs.value)">
										<view class="xz1"></view>
										<view>{{xxs.name}}</view>
									</view>
									<view class="uni-list-cell" v-else @click="radioChange(0,tms.tmid,item.courseId,xxs.value)">
										<view class="wxz"></view>
										<view>{{xxs.name}}</view>
									</view>
								</block>
							</view>
						</view>
						<view class="box2-2" @click="save()" style="text-align: center;">
							<view class="box2-2-1">提交</view>
						</view>
					</view>
				</block>
			</view>

		</view>
	</view>
</template>

<script>
	import service from '../../service.js';
	
	import {
		mapState,
		mapMutations
	} from 'vuex'

	export default {
		computed: {
			...mapState(['hasLogin', 'forcedLogin'])
			
			
		},
		data() {
			return {
				account: '',
					password: '',
					email: '',
				nshow:0,
				ce: 0,
				list: [{
					courseId: 1,
					courseName: '装饰艺术设计',
					score: 0,
					skjs: '张萌',
					tmList: [{
							tmid: 1,
							tmmc: '我觉得闷闷不乐，情绪低沉。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 2,
							tmmc: '我觉得一天之中早晨最好。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 3,
							tmmc: '我一阵阵哭出来或觉得想哭。。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 4,
							tmmc: '我晚上睡眠不好。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 5,
							tmmc: '我与异性密切接触时和以往一样感到愉快。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 6,
							tmmc: '我夜间睡眠不好',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 7,
							tmmc: '我要哭或想哭。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 8,
							tmmc: '我感到早晨心情最好。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 9,
							tmmc: '我感到情绪沮丧，郁闷。',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						},
						{
							tmid: 10,
							tmmc: '你是否感到食欲不振？或情不自禁地暴饮暴食？',
							tmxh: 1,
							tmlx: '单选题',
							tmda: '',
							xxList: [{
									name: 'A.完全符合',
									value: 1,
									score: 6,
								},
								{
									name: 'B.符合',
									value: 2,
									score: 8,
								},
								{
									name: 'C.基本符合',
									value: 3,
									score: 6,
								},
								{
									name: 'D.基本不符合',
									value: 4,
									score: 5,
								}
							]
						}
					]
				}]
			}
		},
		mounted:function(){
			var _this =this
			service.getUsers().some(function(user) {
				_this.password=user.password
				_this.account=user.account
				_this.email=user.email
			});
			
		},
		methods: {
			...mapMutations(['logout']),
			bindLogin() {
				uni.navigateTo({
					url: '../login/login',
				});
			},
			radioChange(ee, tmId, courseId, value) {
				if (ee == 1) {
					return;
				}
				let score = 0;
				for (let i = 0; this.list != null && i < this.list.length; i++) {
					if (this.list[i].courseId == courseId) {
						for (let j = 0; this.list[i].tmList != null && j < this.list[i].tmList.length; j++) {
							if (this.list[i].tmList[j].tmid == tmId) {
								this.list[i].tmList[j].tmda = value;
							}
							let da = this.list[i].tmList[j].tmda;
							for (let k = 0; this.list[i].tmList[j].xxList != null && k < this.list[i].tmList[j].xxList.length; k++) {
								if (this.list[i].tmList[j].xxList[k].value == da) {
									score += this.list[i].tmList[j].xxList[k].score;
								}
							}
						}
						this.list[i].score = score;
						break;
					}
				}
			},
			xiugai(){
				debugger
				var arr = []
	let ret = '';
	ret = uni.getStorageSync('USERS_KEY');
	if (!ret) {
		ret = '[]';
	}
	arr= JSON.parse(ret);

				for(var i in arr){
					if(this.account==arr[i]["account"]){
					arr[i]["password"]=this.password	
					arr[i]["email"]=this.email	
					}
				}
					
				uni.setStorageSync('USERS_KEY', JSON.stringify(arr));
				
				uni.showModal({
						title: '提示',
						content: '修改成功',
						showCancel: false,
						confirmText: '知道了',
						success: function(res) {
							if (res.confirm) {
				
							}
						}
					})
				
			},
			save() {
				var fen = this.list[0]["score"]
				uni.showModal({
					title: '提示',
					content: '评测已成功提交~得分'+fen+'\n本量表按最新中国常模结果设计，总粗分的正常上限为41分，分值越低状态越好，标准分为总粗分乘以1.25后所得的整数部分。我国以SDS标准分≥50为有抑郁症状。',
					showCancel: false,
					confirmText: '知道了',
					success: function(res) {
						if (res.confirm) {

						}
					}
				})
			},
			bindLogout() {
				this.logout();
				/**
				 * 如果需要强制登录跳转回登录页面
				 */
				if (this.forcedLogin) {
					uni.reLaunch({
						url: '../login/login',
					});
				}
			}
		}
	}
</script>

<style>
	page {
		background: #f2f2f2;
	}

		.box1 {
			background: #fff;

			.box1-1 {
				display: flex;
				justify-content: space-between;
				padding: 30rpx;
				font-size: 34rpx;
				font-family: Microsoft YaHei;
				font-weight: 400;
				color: rgba(51, 51, 51, 1);
				line-height: 40rpx;

				.box1-1-1 {
					color: rgba(51, 51, 51, 1);
				}

				.box1-1-2 {
					color: rgba(136, 136, 136, 1);

					text {
						color: rgba(0, 171, 161, 1);
					}
				}

				border-bottom: 2rpx solid rgba(244, 244, 244, 1);
			}

			.box1-2 {
				font-size: 28rpx;
				font-family: Microsoft YaHei;
				font-weight: 400;
				color: rgba(136, 136, 136, 1);
				line-height: 40rpx;

				.box1-2-1 {
					padding: 30rpx;
				}
			}
		}

		.box2 {
			background: #fff;
			margin-top: 24rpx;

			.box2-1 {
				padding: 26rpx 32rpx;
				border-bottom: 2rpx solid rgba(244, 244, 244, 1);

				.box2-1-1 {
					font-size: 34rpx;
					font-family: PingFang;
					font-weight: 500;
					color: rgba(51, 51, 51, 1);
					line-height: 52rpx;

					text {
						color: rgba(136, 136, 136, 1);
					}
				}

				.box2-1-2 {
					.uni-list-cell {
						display: flex;
						margin-top: 30rpx;
						padding: 10rpx;

						view:nth-child(1) {
							width: 32rpx;
							height: 32rpx;
							content: '';
							margin-top: 12rpx;
						}

						.wxz {
							background: url(../../static/img/s0.png);
							background-size: 100% 100%;
						}

						.xz1 {
							background: url(../../static/img/s1.png);
							background-size: 100% 100%;
						}

						view:nth-child(2) {
							font-size: 28rpx;
							margin: 4rpx 0rpx 0rpx 16rpx;
							line-height: 50rpx;
						}

						border:1px solid rgba(244, 244, 244, 1);
					}

					.xz {
						background: rgba(0, 171, 161, 0.2);
						border: 1px solid rgba(0, 171, 161, 1);
					}
				}
			}

			.box2-2 {
				width: 690rpx;
				background: rgba(0, 171, 161, 1);
				border-radius: 10rpx;
				margin: 160rpx 0rpx 60rpx 30rpx;

				.box2-2-1 {
					margin-left: 310rpx;
					font-size: 36rpx;
					padding: 30rpx 0rpx;
					color: rgba(255, 255, 255, 1);

				}
			}
		}
		.xz{
			background: HSL(201, 97%, 56%);
		}
</style>
