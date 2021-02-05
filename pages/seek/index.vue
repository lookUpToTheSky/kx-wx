<template>
	<view class="seek-wrap">
		<view class="fixed-top">
			<UserHead @changePro="changePro" :list="projects" :activePro="activePro"></UserHead>
			<!-- 巡检上传记录 -->
			<view class="select-wrap">
				<view class="tag-wrap">
					<view class="tag-head">
						<text> 巡检记录</text>
						<view class="right" @click="uploadFile">
							<text>上传</text>
							<u-image width="28rpx" height="28rpx" src="@/static/uview/example/upload-icon.svg"></u-image>
						</view>
						<view class="right" @click="showFilter = !showFilter; showYear = false">
							<text>筛选</text>
							<u-image width="30rpx" height="30rpx" src="@/static/uview/example/filter-icon.svg"></u-image>
						</view>
					</view>
					<view class="slot-content" :class="{show: showFilter}" @tap="showFilter = false">
						<view @tap.stop>
							
							<view class="item-wrap">
								<view class="time-head">
									<view class="title">时间</view>
									<view>
										<u-button @click="showYear = true">
											<text class="u-m-r-10">{{year}}</text>
											<u-icon :name="showYear?'arrow-down':'arrow-right'" size="20"></u-icon>
										</u-button>
										<u-select v-model="showYear" :list="listYear" @confirm="confirmYear"></u-select>
									</view>
								</view>
								<view class="item-box">
									<view class="item" :class="[activeTime == index ? 'active' : '']" @tap="tagClick('time',index)" v-for="(item, index) in 12" :key="index">
										{{year.substr(2)}}{{item+1}}月
									</view>
								</view>
							</view>
							<view class="item-wrap">
								<view class="title">类型</view>
								<view class="item-box">
									<view class="item" :class="[activeType == index ? 'active' : '']" @tap="tagClick('type',index)" v-for="(item, index) in list" :key="index">
										{{item.label}}
									</view>
								</view>
							</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		<!-- 巡检上传记录列表 -->
		<view class="list-wrap">
			<view class="u-card-list" v-for="item in 5" :key="item">
				<u-card :title="recordsList[0].title" :sub-title="recordsList[0].time" :thumb="recordsList[0].thumb">
					<view slot="body">
						<view class="u-body-item u-flex u-col-between u-p-t-0">
							<view class="list-item">
								<text class="">巡检人: </text>
								<text class="u-body-item-title u-line-2">撒旦撒旦</text>
							</view>
							<view class="list-item">
								<text class="">类型: </text>
								<text class="u-body-item-title u-line-2">撒旦撒旦</text>
							</view>
						</view>
						<view class="info-note">
							<u-divider half-width="50" border-color color="#000">总体情况说明</u-divider>
							<view class="u-m-t-20">
								总体情况说明总体情况说明总体情况说明总体情况说明总体情况说明总体情况说明
							</view>
						</view>
					</view>
					<view class="" slot="foot"><u-icon name="chat-fill" size="34" color="" label="30评论"></u-icon></view>
				</u-card>
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
				activePro: {},
				time: null,
				activeType: 0,
				activeTime: 0,
				year: '',
				showUpload: false,
				showFilter: false,
				showYear: false,
				listYear: [],
				recordsList: [{title: '巡查采集点_001',time: '2020-10-20', thumb: 'http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg'}],
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
				list: [{
						label: '全部',
						active: false         ,
					},
					{
						label: '图片',
						active: false,
					},
					{
						label: '日报',
						active: false,
					},
					{
						label: '月报',
						active: false,
					},
					{
						label: '周报',
						active: false,
					},
					{
						label: '年报',
						active: false,
					},
					{
						label: '预警信息',
						active: false,
					}
				],
			}
		},
		onShow() {
			this.getAllPro()
			this.getYearList()
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
			getYearList() {
				const nowYear = new Date().getFullYear()
				let startYear = 2017
				this.listYear = []
				for(let i = startYear; i <= nowYear; i++) {
					this.listYear.unshift({value: i, label: i + '年'})
				}
				this.year = this.listYear[0].label
			},
			changePro(item) {
				this.activePro = item
			},
			changeYear(val) {
				console.log(this.year.length)
				if(this.year.length < 4) {
					this.year += val
				}
			},
			tagClick(selectType, index) {
				if(selectType === 'type') {
					this.activeType = index
				}else{
					this.activeTime = index
				}
			},
			confirmYear(data) {
				this.year = data[0].label
			},
			uploadFile() {
				uni.showToast({
					icon: 'none',
					title:'无权限上传'
				})
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
	.seek-wrap {
		padding-top: 296rpx;
		background-color: #f5f2f0;
		padding-bottom: 2rpx;
		.tag-wrap {
			position: relative;
			width: 100%;
			background-color: #FFFFFF;
			font-size: 16px;
			border-bottom: 1px solid #eee;
			.tag-head {
				padding: 0 30rpx;
				line-height: 80rpx;
				font-size: 16px;
				width: 100%;
				display: flex;
				justify-content: space-between;
				
			}
			.right {
				display: flex;
				align-items: flex-start;
			}
		}
		.slot-content {
			&.show {
				transform: translateY(100%);
				background-color: rgba(0,0,0,0.3);
			}
			position: absolute;
			bottom: 0;
			left: 0;
			z-index: -1;
			width: 100%;
			height: calc(100vh - 290rpx);
			transform-origin: top;
			transform: translateY(0);
			transition: all 0.5s;
			background-color: rgba(0,0,0,0);
			.item-wrap {
				background-color: #FFFFFF;
				.title {
					height: 40rpx;
					line-height: 64rpx;
					padding: 0 40rpx;
					font-size: 16px;
				}
				.time-head {
					width: 100%;
					padding: 0 20rpx;
					padding-top: 20rpx;
					display: flex;
					justify-content: space-between;
				}
			}
			.item-box {
				display: flex;
				flex-wrap: wrap;
				padding: 18rpx;
				border-bottom: 1px solid #eee;
				justify-content: space-between;
				
				.item {
					border: 1px solid $u-type-primary;
					color: $u-type-primary;
					padding: 8rpx 40rpx;
					border-radius: 100rpx;
					margin-top: 20rpx;
				}
				
				.active {
					color: #FFFFFF;
					background-color: $u-type-primary;
				}
			}
		}
		.u-card-wrap { 
			background-color: $u-bg-color;
			padding: 1px;
		}
		
		.u-body-item {
			width: 100%;
			font-size: 32rpx;
			color: #333;
			padding: 20rpx 10rpx;
		}
		
		.list-item {
			display: flex;
			margin-right: 20rpx;
			font-size: 14px;
			text {
				margin-right: 10rpx;
			}
		}	
		.u-body-item image {
			width: 120rpx;
			flex: 0 0 120rpx;
			height: 120rpx;
			border-radius: 8rpx;
			margin-left: 12rpx;
		}
	}
	
</style>
