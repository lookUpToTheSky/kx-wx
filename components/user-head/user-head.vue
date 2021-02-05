<template>
	<view class="head-wrap">
		<!-- 人员项目 -->
		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-20">
			<view class="u-m-r-10">
				<u-avatar :src="userInfo.avatarUrl" size="100"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-14 u-p-b-20">{{userInfo.nickName}}</view>
				<view class="u-font-12 u-tips-color">帐号: admin</view>
			</view>
			<view class="u-m-l-10 u-p-10 u-m-b-10">
				<u-button @click="show = true">
					<text class="u-m-r-10">{{activePro.label}}</text>
					<u-icon :name="show?'arrow-down':'arrow-right'" size="20"></u-icon>
				</u-button>
				<u-select v-model="show" :list="list" @confirm="confirm"></u-select>
			</view>
		</view>
		<!-- 预警通知 -->
		<view class="u-demo-area">
			<u-toast type="warning" ref="uToast"></u-toast>
			<u-notice-bar :autoplay="true" playState="play" :speed="100" @getMore="getMore" 
			mode="horizontal" :show="warnList.length > 0" type="warning" :list="warnList"
			:moreIcon="true" :volumeIcon="true" :duration="2000" :isCircular="true"></u-notice-bar>
		</view>
	</view>
	
</template>

<script>
	export default {
		props: {
			activePro: Object,
			list: Array
		},
		data() {
			return {
				show: false,
				userInfo: null,
				warnList: [
					'1.锦瑟无端五十弦，一弦一柱思华年',
					'2.庄生晓梦迷蝴蝶，望帝春心托杜鹃',
					'3.沧海月明珠有泪，蓝田日暖玉生烟',
				]
			}
		},
		created() {
			uni.getStorage({
				key: 'userInfo',
				success: (res) =>{
					this.userInfo = res.data;
				},
				fail: (err)=> {
					console.log(err)
				}
			});
		},
		methods: {
			confirm(data) {
				uni.setStorage({
					key: 'project',
					data: {...data[0]}
				})
				this.$emit("changePro", data[0])
			},
			// 更多
			getMore() {
				this.$refs.uToast.show({
					type: 'warning',
					url: ''
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.head-wrap {
		border-bottom: 10rpx solid #f5f2f0;
	}
	.user-box {
		background-color: #fff;
	}
</style>
