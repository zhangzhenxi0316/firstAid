<template>
	<view class="box">
		<!-- 头部开始 -->
		<view class="header">
			<!-- 头部顶部开始 -->
			<view class="header_banner">
				<view class="userinfo">
					<view class="userinfo_item">编号2</view>
					<view class="userinfo_item">zzx</view>
					<view class="userinfo_item">admin</view>
				</view>
			</view>
			<!-- 头部顶部结束 -->
			<!-- 头部控制条开始 -->
			<view class="header_control">
				<view class="mode_control">
					<view class="mode_item" v-for="item in modeList" :key="item.id">
						<uni-icons :type="item.isCurrent?'circle-filled':'circle'"></uni-icons>
						<text>{{item.name}}</text>
					</view>
					<view class="button_control">
						{{isStart?"停止":"开始"}}
					</view>
				</view>

				<view class="setting">
					<view class="setting_item">设置</view>
					<view class="setting_item">成绩</view>
					<view class="setting_item">用户设置</view>
					<view class="setting_item">关于</view>
				</view>
			</view>
			<!-- 头部控制条结束 -->
			<!-- logo -->
			<view class="logo_wrapper">
				<view class="text">
					<view class="en">
						CPR
					</view>
					<view class="zn">
						心肺复苏考核系统
					</view>
				</view>
				<view class="logoImage">
					<image src="/static/logo1.png" mode="aspectFill"></image>
				</view>
			</view>
		</view>
		<!-- 头部结束 -->
		<!-- 数据部分开始 -->
		<view class="main">
			<view class="content">
				<!-- 左侧信息部分 -->
				<view class="left_msg">
					<view class="judge">
						<view class="judge_item" v-for="item in operateList" :key="item.id">
							<uni-icons :type="item.isCorrect?'checkbox':'close'" :color="item.isCorrect?'green':'red' "></uni-icons>
							<text>{{item.name}}</text>
						</view>

					</view>
					<view class="judge_list">
						<view class="list_item" v-for="item in 6">
							<view class="item_title">倒计时</view>
							<view class="item_msg">
								<text class="num">120</text>
								<text class="unit">次</text>
							</view>
						</view>
					</view>
				</view>
				<!-- 详细信息 -->
				<view class="detail_msg">
					<!-- 标题 -->
					<view class="detail_item">
						<view class="detail_title press">按压</view>
						<view class="detail_item_list">
							<view class="list_item" v-for="item in operateDetail.press" :key="item.id">
								<view class="item_title">{{item.title}}</view>
								<view class="item_num">{{item.num?item.num:'--'}}</view>
							</view>
						</view>
					</view>
					<view class="detail_item">
						<view class="detail_title ">吹气</view>
						<view class="detail_item_list">
							<view class="list_item" v-for="item in operateDetail.blow" :key="item.id">
								<view class="item_title">{{item.title}}</view>
								<view class="item_num">{{item.num?item.num:'--'}}</view>
							</view>
						</view>
					</view>
					<view class="detail_item aed">
						<view class="detail_title ">AED</view>
						<view class="detail_item_list aed">
							<view class="list_item" v-for="item in operateDetail.AED" :key="item.id">
								<view class="item_title">{{item.title}}</view>

							</view>
						</view>
					</view>
				</view>
			</view>
			<!-- 数据可视化开始 -->
			<view class="visualization">
				<view class="qiun-charts">
					<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA"></canvas>
				</view>
				<view class="qiun-charts">
					<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA"></canvas>
				</view>
			</view>
		</view>
		<!-- 数据部分结束 -->

	</view>
</template>

<script>
	import uCharts from '../../components/u-charts/u-charts/u-charts.min.js'
	let _self = null;
	let canvaLineA=null;
	export default {
		data() {
			return {
				operateDetail: {
					AED: [{
							id: 0,
							title: '正确',

						},
						{
							id: 1,
							title: '操作错误',

						},
						{
							id: 2,
							title: '电极错误',

						}
					],
					press: [{
							id: 0,
							title: '正确',
							num: null
						},
						{
							id: 1,
							title: '过大',
							num: 10
						},
						{
							id: 2,
							title: '过小',
							num: 6
						},
						{
							id: 3,
							title: '多按',
							num: 6
						},
						{
							id: 4,
							title: '少按',
							num: 6
						},
						{
							id: 5,
							title: '按压频率',
							num: null
						},
						{
							id: 6,
							title: '位置错误',
							num: 10
						},
						{
							id: 7,
							title: '错误频率',
							num: 6
						},
						{
							id: 8,
							title: '回弹错误',
							num: 6
						},
						{
							id: 9,
							title: '中断过长',
							num: 6
						}
					],
					blow: [{
							id: 0,
							title: '正确',
							num: null
						},
						{
							id: 1,
							title: '过大',
							num: 10
						},
						{
							id: 2,
							title: '过小',
							num: 6
						},
						{
							id: 3,
							title: '多按',
							num: 6
						},
						{
							id: 4,
							title: '少按',
							num: 6
						}
					],
					
				},
				operateList: [{
						id: 0,
						isCorrect: true,
						name: '意识判断'
					},
					{
						id: 1,
						isCorrect: false,
						name: '动脉触摸'
					},
					{
						id: 2,
						isCorrect: false,
						name: '开放气道'
					}
				],
				modeList: [{
						id: 0,
						isCurrent: true,
						type: 'train',
						name: '训练'
					},
					{
						id: 1,
						isCurrent: false,
						type: 'test',
						name: '测试'
					},
					{
						id: 2,
						isCurrent: false,
						type: 'exam',
						name: '考试'
					}
				],
				isStart: false,
				cWidth: '',
				cHeight: '',
				pixelRatio: 1,
			}
		},
		onLoad() {
			_self = this;
			this.cWidth=972;
			this.cHeight=120;
			this.getServerData();
		},
		methods: {
			getServerData() {
				let chartData = {
					categories: ['2012', '2013', '2014', '2015', '2016', '2017'],
					series: [{
						name: '成交量A',
						data: [35, 20, 25, 37, 4, 20],
						color: '#000000'
					}, {
						name: '成交量B',
						data: [70, 40, 65, 100, 44, 68]
					}, {
						name: '成交量C',
						data: [100, 80, 95, 150, 112, 132]
					}]
				}
				

				_self.showLineA("canvasLineA", chartData);

			},
			showLineA(canvasId, chartData) {
				// console.log(canvasId,chartData)
				canvaLineA = new uCharts({
					$this: _self,
					canvasId: canvasId,
					type: 'line',
					fontSize: 11,
					legend: {
						show: true
					},
					dataLabel: false,
					dataPointShape: true,
					background: '#FFFFFF',
					pixelRatio: _self.pixelRatio,
					categories: chartData.categories,
					series: chartData.series,
					animation: true,
					xAxis: {
						type: 'grid',
						gridColor: '#CCCCCC',
						gridType: 'dash',
						dashLength: 8
					},
					yAxis: {
						gridType: 'dash',
						gridColor: '#CCCCCC',
						dashLength: 8,
						splitNumber: 5,
						min: 10,
						max: 180,
						format: (val) => {
							return val.toFixed(0) + '元'
						}
					},
					width: Number(_self.cWidth) * _self.pixelRatio,
					// width:900,
					// height:120,
					height: Number(_self.cHeight) * _self.pixelRatio,
					extra: {
						line: {
							type: 'straight'
						}
					}
				});
				console.log(canvaLineA)
			},
			touchLineA(e) {
				canvaLineA.showToolTip(e, {
					format: function(item, category) {
						return category + ' ' + item.name + ':' + item.data
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	page {
		background: #e4e4e4;

	}

	.box {

		// 头部顶部
		.header {
			position: relative;

			.header_banner {
				display: flex;
				padding: 0 25px;
				box-sizing: border-box;
				height: 45px;
				background: #E5EFFE;

				.userinfo {
					display: flex;
					font-size: 12px;
					align-items: center;

					.userinfo_item {
						margin-right: 10px;
					}
				}
			}

			// 头部控制条
			.header_control {
				display: flex;
				font-size: 12px;
				height: 55px;
				padding: 0 25px;
				background: rgb(218, 231, 240);
				background: linear-gradient(31deg, rgba(218, 231, 240, 1) 63%, rgba(147, 165, 192, 1) 100%);
				align-items: center;
				justify-content: space-between;

				.mode_control {
					display: flex;
					align-items: center;

					.mode_item {
						margin-right: 10px;
						display: flex;
						align-items: center;

					}

					// 开始停止按钮
					.button_control {
						border: 2px solid #0C74B7;
						color: #0C74B7;
						height: 30px;
						width: 80px;
						display: flex;
						justify-content: center;
						align-items: center;
						border-radius: 15px;
						margin-left: 15px;
					}
				}

				// setting
				.setting {
					color: #fff;
					display: flex;
					width: 205px;

					.setting_item {
						margin-right: 5px;
					}
				}
			}

			// logo
			.logo_wrapper {
				height: 100px;
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translateX(-50%) translateY(-50%);
				display: flex;
				justify-content: center;
				align-items: center;

				.text {
					.en {
						font-size: 30px;
						color: #27008F;
						font-weight: 700;
					}

					.zn {
						font-size: 17px;
						color: #27008F;
						font-weight: 500;
					}
				}

				.logoImage {
					width: 90px;
					height: 90px;
					overflow: hidden;
					border-radius: 50%;
					margin-left: 10px;
					background: linear-gradient(145deg, #f5ffff, #ced7e5);
					box-shadow: 11px 11px 22px #9ca3ad,
						-11px -11px 22px #ffffff;

					image {
						width: 100%;
						height: 100%;
					}
				}
			}
		}

		// 数据部分
		.main {
			font-size: 12px;
			box-sizing: border-box;
			padding: 6px;
			width: 100%;
			// height: 1000px;

			.content {
				display: flex;
				justify-content: space-between;

				.left_msg {
					width: 34vw;
					height: 230px;
					// background-color: red;
					display: flex;
					justify-content: space-between;

					.judge {
						border-radius: 5px;
						width: 23%;
						height: 230px;
						background: #fff;
						display: flex;
						flex-direction: column;
						justify-content: space-around;

						.judge_item {
							display: flex;
							justify-content: center;
							align-items: center;
						}
					}

					.judge_list {
						width: 76.5%;
						display: flex;
						flex-wrap: wrap;
						justify-content: space-around;
						align-content: space-between;

						.list_item {
							width: 32%;
							height: 49%;
							// border: 1px solid #000;
							border-radius: 5px;
							background: #fff;
							display: flex;
							justify-content: space-around;
							flex-direction: column;
							padding: 0 5px;
							box-sizing: border-box;

							.num {
								font-size: 30px;
							}
						}
					}
				}

				.detail_msg {
					width: 64vw;
					height: 230px;
					// background-color: red;
					display: flex;
					flex-direction: column;
					justify-content: space-between;

					.detail_item {
						display: flex;
						justify-content: space-between;

						.detail_title {
							// flex: 1;
							width: 16%;
							// height: 115px;
							background: #fff;
							border-radius: 5px;
							display: flex;
							justify-content: center;
							align-items: center;
							font-size: 14px;

							&.press {
								height: 115px;
							}
						}

						.detail_item_list {
							// flex: 6;
							width: 83.5%;
							display: flex;
							flex-wrap: wrap;
							justify-content: space-around;
							align-content: space-between;

							&.aed {
								.list_item {
									width: 32.5%;

									.item_title {
										display: flex;
										justify-content: center;
										align-items: center;
									}
								}
							}

							.list_item {
								width: 19%;
								// flex: 0.9;
								height: 55px;
								background: #fff;
								display: flex;
								border-radius: 5px;

								.item_title {
									flex: 1;
									font-size: 12px;
									padding: 5px 10px;

								}

								.item_num {
									flex: 2;
									display: flex;
									padding-right: 10px;
									justify-content: flex-end;
									align-items: flex-end;
									font-size: 28px;
									font-weight: 600;
								}


							}

						}
					}

				}

			}

			// 数据可视化部分
			.visualization {
				margin-top: 15px;
				width: 100%;
				height: 400px;
				background: #fff;
				border-radius: 5px;
				padding: 0 20px;
				box-sizing: border-box;
				display: flex;
				flex-direction: column;
				justify-content: space-around;

				.qiun-charts {
					canvas{
						width: 100%;
						height: 120px;
					}
					// width: 100%;
				}
			}
		}

	}
</style>
