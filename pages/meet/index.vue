<template>
	<view class="meet-wrap">
		<view class="fixed-top">
			<UserHead @changePro="changePro" :list="projects" :activePro="activePro"></UserHead>
		</view>
		<!-- 会议列表 -->
		<view class="meet-list-wrap">
			<text>当前会议列表</text>
			<view class="meet-list" @click="intoMeetRoom(item)" :class="{active: item.status == 1}" v-for="item in meetList" :key="item.id">
				<text class="title">{{item.name}}</text>
				<view class="">
					<text>发起人:</text>
					<text>{{item.meetStartUser}}</text>
				</view>
				<text>共{{item.count}}人参会</text>
				<view class="">
					<text>开始时间:</text>
					<text>{{new Date().toLocaleTimeString()}}</text>
				</view>
				<view class="meet-status" :class="{activeStatus: item.status == 1}">
					<text>{{item.status == 1 ? '进行中' : '已结束'}}</text>
				</view>
			</view>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import UserHead from '@/components/user-head/user-head.vue'
	export default {
		components: {
			UserHead
		},
		data() {
			return {
				activePro: {},
				meetList: [
					{
						id: 1,
						meetStartUser: '老总',
						name: 'XX会议',
						status: 1,
						count: 5
					},
					{
						id: 2,
						meetStartUser: '小明',
						name: '工作讨论',
						status: 0,
						count: 12
					},
					{
						id: 3,
						meetStartUser: '隔壁老王',
						name: '明日计划',
						status: 1,
						count: 2
					},
					{
						id: 4,
						meetStartUser: '二线',
						name: '会议',
						status: 1,
						count: 5
					},
					{
						id:5,
						meetStartUser: '二小明',
						name: '工作讨论',
						status: 0,
						count: 12
					},
					{
						id: 5,
						meetStartUser: '隔壁老娘',
						name: '明日计划',
						status: 1,
						count: 2
					}
				],
				projects: [
					{
						value: '1',
						label: '测试项目123'
					},
					{
						value: '2',
						label: '测试项目456'
					}
				]
			}
		},
		onShow() {
			this.getAllPro()
		},
		methods: {
			getAllPro() {
				uni.getStorage({
					key: 'project',
					success: (res) =>{
						this.activePro = res.data;
					},
					fail: ()=> {
						this.activePro = this.list[0];
					}
				});
			},
			changePro(item) {
				this.activePro = item
			},
			intoMeetRoom(item) {
				if(item.status === 1) {
					this.$refs.uToast.show({
						type:'warning',
						title: '无权限进入!未开通'
					})
				}else{
					this.$refs.uToast.show({
						title: '此会议已结束!'
					})
				}
			}
		}
	}
</script>

<style lang="scss">
.meet-wrap{
	background-color: #ededed;
	padding-top: 214rpx;
}
.fixed-top {
	position: fixed;
	width: 100%;
	top: 0;
	left: 0;
	z-index: 999999;
	background-color: #f5f0f2;
}
.meet-list-wrap {
	padding: 20rpx;
	background-color: #fff;
	.meet-list {
		border: 1px solid #dedede;
		padding: 20rpx;
		margin-top: 20rpx;
		text-align: center;
		overflow: hidden;
		position: relative;
		background-color: #f5f0f2;
		&.active {
			color: #fff;
			background-color: #2979FF;
		}
		.title {
			font-weight: 550;
			line-height: 60rpx;
			font-size: 14px;
		}
		.meet-status {
			position: absolute;
			top: -30rpx;
			right: -80rpx;
			text-align: center;
			color: #888;
			width: 200rpx;
			height: 100rpx;
			line-height: 160rpx;
			transform: rotateZ(45deg);
			&.activeStatus {
				color: #eee;
				background-color: red;
			}
		}
	}
}
</style>
