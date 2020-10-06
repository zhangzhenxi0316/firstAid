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
					<view class="mode_item" v-for="item in modeList" :key="item.id" @click="hanleTagger(item.id)">
						<uni-icons :type="item.isCurrent?'circle-filled':'circle'"></uni-icons>
						<text>{{item.name}}</text>
					</view>
					<view class="button_control" @click="handleStartChange">
						{{isStart?"停止":"开始"}}
					</view>
				</view>

				<view class="setting">
					<navigator url="/pages/setting/setting">
					<view class="setting_item">设置</view>
					</navigator>
					<navigator url="/pages/grades/grades">
						<view class="setting_item">成绩</view>
					</navigator>
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
			handleStartChange(){
				this.isStart = !this.isStart
			},
			hanleTagger(id){
				this.modeList = this.modeList.map(item=>{
					item.id===id?item.isCurrent = true:item.isCurrent = false;
					return item 
				})
			}
			,
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

<style scoped lang="scss">
	@import  './index.scss'
</style>
