<template>
	<view class="home-wrap">
		<view class="fixed-top">
			<UserHead @changePro="changePro" :list="projects" :activePro="activePro"></UserHead>
		</view>
		<!-- 监测类型显示采集点 -->
		<view class="u-demo-area">
			<u-toast ref="uToast"></u-toast>
			<u-tabs bg-color="#ffff" :bold="bold" :active-color="activeColor" :list="monitorList"
			@change="changeMonitor" :current="currentMonitor" :is-scroll="true" :offset="[0, 0]"></u-tabs>
			<view class="point-wrap u-p-20 u-m-t-4">
				<u-checkbox-group @change="checkboxGroupChange">
					<u-checkbox 
						@change="checkboxChange" 
						v-model="item.checked" 
						v-for="(item, index) in pointList" :key="index" 
						:name="item.pointName"
						size="40"
						shape="circle"
					>{{item.pointName}}</u-checkbox>
				</u-checkbox-group>
			</view>
		</view>
		<!-- 显示类型 -->
		<view class="data-wrap u-m-t-10">
			<u-subsection 
			:active-color="activeColor"
			@change="sectionChange" 
			mode="subsection"
			:list="[{id: 0, name: '数据曲线'},{id: 1, name: '实时数据'}]" 
			:current="0"></u-subsection>
			<view class="data-show-wrap">
			</view>
		</view>
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
				showWarn: true,
				currentMonitor: 0,
				activePro: {},
				projects: [
					{
						value: '1',
						label: '测试项目123'
					},
					{
						value: '2',
						label: '测试项目456'
					}
				],
				monitorList: [{
					name: '关注',
					count: 100
				}, {
					name:  '推荐',
					count: 7
				}, {
					name: '电影'
				},{
					name: '电视剧'
				},{
					name: '小视频'
				}, {
					name:  '游戏'
				}, {
					name: '校园'
				},{
					name: '影视'
				},{
					name: '音乐'
				}],
				pointList: [
					{pointId: 1, pointName: 'jsd-012313213213', checked: false},
					{pointId: 2, pointName: 'jsd-03', checked: false},
					{pointId: 3, pointName: 'jsd-03', checked: false},
					{pointId: 4, pointName: 'jsd-03', checked: false},
					{pointId: 5, pointName: 'jsd-03', checked: false},
					{pointId: 6, pointName: 'jsd-03', checked: false},
					{pointId: 7, pointName: 'jsd-03', checked: false},
					{pointId: 8, pointName: 'jsd-03', checked: false},
					],
				activeColor: this.$u.color['primary'],
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
				this.activePro = {...item}
			},
			changeMonitor(index) {
				this.currentMonitor = index;
			},
			// 选择采集点
			checkboxGroupChange() {
				
			},
			checkboxChange() {
				
			},
			sectionChange() {
				
			}
		}
	}
</script>

<style lang="scss" scoped>
	.fixed-top {
		position: fixed;
		width: 100%;
		top: 0;
		left: 0;
		z-index: 99;
		background-color: #f5f0f2;
	}
	.home-wrap {
		padding-top: 214rpx;
		background-color: #f5f0f2;
		.point-wrap {
			width: 100%;
			min-height: 300rpx;
			background-color: #fff;
		}
		.data-show-wrap {
			width: 100%;
			height: 500rpx;
			background-color: #fff;
		}
	}
</style>
