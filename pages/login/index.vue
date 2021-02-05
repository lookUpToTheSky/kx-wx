<template>
	<view class="wrap">
		<view v-if="wxLogin">
			<view class="top"></view>
			<view class="content">
				<view class="title">欢迎登录</view>
				<input maxlength="11" class="u-border-bottom" type="number" v-model="tel" placeholder="请输入手机号" />
				<button @tap="submit" :style="[inputStyle]" class="getCaptcha">获取短信验证码</button>
				<view class="tips">未注册的手机号无法登录，联系管理员注册</view>
				<!-- <view class="alternative">
					<view class="password">密码登录</view>
					<view class="issue">遇到问题</view>
				</view> -->
			</view>
			<view class="buttom">
				<view class="loginType">
					<view class="wechat item">
						<view class="icon"><u-icon size="70" name="weixin-fill" color="rgb(83,194,64)"></u-icon></view>
						微信登录
					</view>
					<view class="QQ item">
						<view class="icon"><u-icon size="70" name="account-fill" color="rgb(17,183,233)"></u-icon></view>
						账号登录
					</view>
				</view>
				<view class="hint">
					登录代表同意小程序获取用户信息（如昵称、头像、地址）
				</view>
			</view>
		</view>
		<view v-else class="wxlogin-wrap">
			<view class="logo-wrap">
				<u-icon name="account-fill" size="80"></u-icon>
			</view>
			<button @getuserinfo="appLoginWx" open-type="getUserInfo" class="login">微信一键登录</button>
			<label>{{phone}}</label>
			<button v-show="false" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber" ref="getUserPhone"> 获取手机号码</button>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
export default {
	data() {
		return {
			tel: '15288888888',
			userInfo: null,
			wxLogin: false
		}
	},
	onLoad() {
		// #ifdef MP-WEIXIN
		if(wx.hideHomeButton){  
		    wx.hideHomeButton();  
		}  
		let _this = this
		uni.getSetting({
			success(res) {
				console.log(!res.authSetting['scope.userInfo'])
				if (!res.authSetting['scope.userInfo']) {
					_this.wxLogin = false
				}else{
					_this.wxLogin = true
				}
			}
		})
		//#endif
	},
	computed: {
		inputStyle() {
			let style = {};
			if(this.tel) {
				style.color = "#fff";
				style.backgroundColor = this.$u.color['warning'];
			}
			return style;
		}
	},
	methods: {
		submit() {
			if(this.$u.test.mobile(this.tel)) {
				this.$u.route({
					url: 'pages/login/code?phone=' + this.tel
				})
			}else{
				this.$refs.uToast.show({
					title: '输入的手机号不正确！',
					type: 'wraning'
				})
			}
		},
		
		getPhoneNumber(e) {
			console.log(112, e.detail.iv);
			console.log(e.detail.encryptedData);
			wx.login({
			  success: res => {
				console.log(res.code);
			  }
			})
		},
		// 微信授权登录
		appLoginWx() {
			let _this = this
			// #ifdef MP-WEIXIN
			uni.getProvider({
			  service: 'oauth',
			  success: function (res) {
				if (~res.provider.indexOf('weixin')) {
					uni.login({
						provider: 'weixin',
						success: (res2) => {
							uni.getUserInfo({
								provider: 'weixin',
								success: (info) => {//这里请求接口
									console.log(info);
									_this.userInfo = info.userInfo
									_this.wxLogin = true
									uni.setStorage({ key: 'userInfo', data: info.userInfo});
								},
								fail: () => {
									uni.showToast({title:"微信登录授权失败",icon:"none"});
								}
							})
						},
						fail: () => {
							uni.showToast({title:"微信登录授权失败",icon:"none"});
						}
					})
				}else{
					uni.showToast({
						title: '请先安装微信或升级版本',
						icon:"none"
					});
				}
			  }
			});
			//#endif
		}
	}
};
</script>

<style lang="scss" scoped>
.wrap {
	font-size: 28rpx;
	.content {
		width: 600rpx;
		margin: 80rpx auto 0;
		.title {
			text-align: left;
			font-size: 50rpx;
			font-weight: 500;
			margin-bottom: 100rpx;
		}
		input {
			text-align: left;
			margin-bottom: 40rpx;
			padding: 20rpx 30rpx;
			border: 1px solid #eee;
			border-radius: 6rpx;
		}
		.tips {
			color: $u-type-info;
			margin-bottom: 60rpx;
			margin-top: 8rpx;
			font-size: 12px;
		}
		.getCaptcha {
			background-color: rgb(253, 243, 208);
			color: $u-tips-color;
			border: none;
			font-size: 30rpx;
			padding: 12rpx 0;
			
			&::after {
				border: none;
			}
		}
		.alternative {
			color: $u-tips-color;
			display: flex;
			justify-content: space-between;
			margin-top: 30rpx;
		}
	}
	.buttom {
		.loginType {
			display: flex;
			padding: 250rpx 150rpx 150rpx 150rpx;
			justify-content:space-between;
			
			.item {
				display: flex;
				flex-direction: column;
				align-items: center;
				color: $u-content-color;
				font-size: 28rpx;
			}
		}
		
		.hint {
			padding: 20rpx 40rpx;
			font-size: 20rpx;
			color: $u-tips-color;
			text-align: center;
		}
	}
	.wxlogin-wrap {
		width: 100vw;
		height: 100vh;
		background-color: $uni-bg-color-grey;
		display: flex;
		flex-direction: column;
		align-items: center;
		.logo-wrap {
			margin: 300rpx 0 40rpx 0;
			.logo {
				width: 200rpx;
				height: 200rpx;
				overflow: hidden;
				background-color: #12223B;
				box-shadow: 0 0 15px 5px #888;
				border-radius: 50%;
			}
		}
		.login {
			color: $uni-color-primary;
			border: 4rpx solid $uni-color-primary;
			&:hover {
				color: #fff;
				background-color: $uni-color-primary;
			}
		}
	}
}
</style>
