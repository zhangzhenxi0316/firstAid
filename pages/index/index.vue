<template>
	<view class="box">
		<!-- 头部开始 -->
		<view class="header">
			<!-- 头部顶部开始 -->
			<view class="space"></view>
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
					<!-- 训练控制 -->
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
					<navigator url="/pages/login/login">
						<view class="setting_item">用户设置</view>
					</navigator>
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
							<uni-icons v-show="item.name==''?false:true"  :type="item.isCorrect?'checkbox':'close'" :color="item.isCorrect?'green':'red' "></uni-icons>
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
				<view class="echarts">
					<echarts :option="option1"></echarts>
				</view>
				<view class="echarts">
					<echarts :option="JSON.parse(JSON.stringify(option1))"></echarts>
				</view>

			</view>
			<!-- 数据部分结束 -->

		</view>
		<!-- 考试信息弹框 -->
		<wyb-popup class="popup-wrapper" ref="popup" type="center" height="180" width="250" radius="6" :showCloseIcon="true">
			<view class="popup-content">
				<view class="popup-title">
					请填写完整资料
				</view>
				<view class="popup-input">
					<input type="text" placeholder="请输入老师账号" v-model="teacherAccount" />
					<input type="text" placeholder="请输入课程id" v-model="courseId" />
				</view>
				<view class="popup-input">
					<button class="pupup-btn" @click="handleSubmit"> 确定</button>
				</view>
			</view>
		</wyb-popup>

	</view>
</template>

<script>
	import echarts from '@/components/echarts/echarts.vue'
	let data = []
	let xDate = []
	// let dataArray = []
	let deviceDiscover = false;
	let devicename = 'BLE SPP'
	let msg = ''
	let startNum = 9
	for (var i = 0; i < 500; i++) {
		xDate[i] = i * 50 / 1000
	}


	export default {
		data() {
			return {
				serviceId: '',
				deviceId: '',
				notycharacteristicsId: '',
				characteristicsId: '',
				dataArray: [],
				teacherAccount: '',
				courseId: '',

				timer1: '',
				time2: '',
				option1: {
					// backgroundColor: '#2c343c',
					animation: false,
					xAxis: [{
							boundaryGap: false, // 坐标的刻度是否在中间 
							data: xDate, //数据
							axisLine: { // 坐标轴线条相关设置(颜色等)
								lineStyle: {
									color: '#0e2a60'
								}
							},
							show: false,
							axisTick: { // 坐标轴刻度相关设置
								length: '0' // 长度设置为0
							},
							axisLabel: { // 坐标轴刻度标签
								margin: 6, // 刻度标签与轴线之间的距离
								fontSize: '12',
								color: '#ffffff'
							},
							splitLine: { // 坐标轴分割线。默认数值轴显示，类目轴不显示
								show: false,
								lineStyle: {
									color: '#0e2a60'
								}
							},
						},
						{
							position: 'top',
							boundaryGap: false, // 坐标的刻度是否在中间 
							data: [0, 2, 4, 6, 8, 10], //数据
							show: true,
							axisLine: { // 坐标轴线条相关设置(颜色等)
								lineStyle: {
									color: '#fff'
								}
							},
							axisTick: { // 坐标轴刻度相关设置
								length: '0' // 长度设置为0
							},
							axisLabel: { // 坐标轴刻度标签
								margin: 6, // 刻度标签与轴线之间的距离
								fontSize: '12',
								color: '#ffffff'
							},

						}
					],
					yAxis: [{
						type: 'value',
						// data:[100,200,300,400]
						min: 0,
						max: 500,
						splitNumber:2,
						inverse: true
					}],
					series: [{
						name: '频率',

						data: [],
						type: 'line',
						smooth: true,

					}],
					grid: {
						x: 30,
						y: 20,
						x2: 10,
						y2: 30,
						borderWidth: 20
					}
				},
				bluetoothLinks: [],
				deviceID: '',
				operateDetail: {
					AED: [{
							id: 0,
							title: '',

						},
						{
							id: 1,
							title: '',

						},
						{
							id: 2,
							title: '',

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
							title: '少按',
							num: 6
						},
						{
							id: 4,
							title: '多按',
							num: 6
						},
						{
							id: 5,
							title: '回弹不充分',
							num: 0
						},
						{
							id: 6,
							title: '按压频率',
							num: null
						},
						// {
						// 	id: 6,
						// 	title: '位置错误',
						// 	num: 10
						// },
						{
							id: 7,
							title: '频率错误数量',
							num: 6
						},
						// {
						// 	id: 8,
						// 	title: '回弹错误',
						// 	num: 6
						// },
						// {
						// 	id: 9,
						// 	title: '中断过长',
						// 	num: 6
						// }
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
							title: '少吹',
							num: 6
						},
						{
							id: 4,
							title: '多吹',
							num: 6
						}
					],

				},
				operateList: [{
						id: 0,
						isCorrect: true,
						name: ''
					},
					{
						id: 1,
						isCorrect: false,
						name: ''
					},
					{
						id: 2,
						isCorrect: false,
						name: ''
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
				// 经
				traLongitude: '',
				// 纬
				traLatitude: '',
				startTime: '',
				endTime: ''
			}
		},
		onLoad() {
			// uni.showLoading({
			// 				title:'获取位置信息',
			// 			})
			uni.getLocation({
				type: 'wgs84',
				success: function(res) {
					console.log('当前位置的经度：' + res.longitude);
					console.log('当前位置的纬度：' + res.latitude);
				}
			});
			// uni.getLocation({
			// 	success: (res) => {
			// 		uni.hideLoading()
			// 		uni.showToast({
			// 			title:'位置获取成功',
			// 			icon:'none'
			// 		})
			// 		console.log(res)
			// 		this.traLongitude = res.longitude
			// 		this.traLatitude = res.latitude
			// 	},
			// 	fail: (err) => {
			// 		uni.hideLoading()
			// 		uni.showToast({
			// 			title:'位置获取失败',
			// 			icon:'none'
			// 		})
			// 	},
			// 	complete: () => {
			// 		console.log('9')
			// 		// uni.hideLoading()
			// 	}
			// })

		},
		onShow() {
			uni.openBluetoothAdapter({
				success: () => {
					console.log("蓝牙模板初始化成功")
					this.startBluetoothDeviceDiscovery()
					this.bluetoothLinks = []
				},
				fail: () => {
					//如果用户未开启蓝牙权限，弹窗提示
					uni.showToast({
						title: '请先打开蓝牙',
						icon: "none"
					});
				}
			})
		},
		onShow() {
			this.openBluetoothAdapter()

		},
		methods: {
			switchFun(category, item) {
				switch (category) {
					// 0x02 20ms数据用来看按压数据
					case "02":
						if (this.isStart) {
							let obj = this.parse02(item)
							// that.list.press.deep = obj.pressDeep;
							// that.list.cui.deep = obj.cui
							// that.option1.series.data.push(obj.pressDeep)
							if (that.option1.series[0].data.length > 500) {
								that.option1.series[0].data.shift()
								// console.log(this.option1.sseries[0].data)
							}
							// console.log(this.option1.series[0].data)
							that.option1.series[0].data.push(obj.pressDeep)
							// that.dataArray.push(obj.pressDeep) 
							console.log(obj.pressDeep, obj.cui)
						}
						break;
					case "01":
						//模型设备编码
						// console.log(item)
						let code = item.slice(8, 14)
						console.log(code)
						break;
					case "07":
						//门的状态
						let mode = parseInt(item.slice(8, 10), 16)
						break;
					case "0b":
						console.log('闻讯cpr设置');
						break;
					case "0a":
						console.log('接收设置完成');
						break;
					case "03":
						this.operateDetail.press[0].num = parseInt(item.slice(8, 10), 16)
						this.operateDetail.press[1].num = parseInt(item.slice(10, 12), 16)
						this.operateDetail.press[2].num = parseInt(item.slice(12, 14), 16)
						this.operateDetail.press[3].num = parseInt(item.slice(14, 16), 16)
						this.operateDetail.press[4].num = parseInt(item.slice(16, 18), 16)
						this.operateDetail.press[5].num = parseInt(item.slice(18, 20), 16)
						break;
					case "04":
						// blow
						this.operateDetail.blow[0].num = parseInt(item.slice(8, 10), 16)
						this.operateDetail.blow[1].num = parseInt(item.slice(10, 12), 16)
						this.operateDetail.blow[2].num = parseInt(item.slice(12, 14), 16)
						this.operateDetail.blow[3].num = parseInt(item.slice(14, 16), 16)
						this.operateDetail.blow[4].num = parseInt(item.slice(16, 18), 16)
						break;
					case "05":
						this.operateDetail.press[6].num = parseInt(item.slice(8, 10), 16)
						this.operateDetail.press[7].num = parseInt(item.slice(10, 12), 16)
						break;
					case "06":
						this.operateDetail.AED[0].title = parseInt(item.slice(8, 10), 16) == 0 ? '关机' : '开机时间不对';
						switch (item.slice(10, 12)) {
							case "00":
								this.operateDetail.AED[1].title = '电极未插入';
								break;
							case "01":
								this.operateDetail.AED[1].title = '电极插入次序不对';
								break;
							case "02":
								this.operateDetail.AED[1].title = '电极贴的位置不对';
								break;

						}
						switch (item.slice(12, 14)) {
							case "01":
								this.operateDetail.AED[2].title = '正确除颤';
								break;
							case "02":
								this.operateDetail.AED[2].title = '危险操作';
								break;
							case "03":
								this.operateDetail.AED[2].title = '错误除颤';
								break;

						}
						break;
					case "09":
						if (item.slice(8, 10) == "00") {
							this.operateList[0].name = "意识未判断";
							this.operateList[0].isCorrect = false
						} else {
							this.operateList[0].name = "意识判断"
							this.operateList[0].isCorrect = true
						}
						if (item.slice(10, 12) == "00") {
							this.operateList[1].name = "未检测到动脉";
							this.operateList[1].isCorrect = false
						} else {
							this.operateList[1].name = "检测动脉"
							this.operateList[1].isCorrect = true
						}
						if (item.slice(12, 14) == "00") {
							this.operateList[2].name = "未抬头开放气道";
							this.operateList[2].isCorrect = false
						} else {
							this.operateList[2].name = "抬头开放气道"
							this.operateList[2].isCorrect = true
						}
						break;
					case "0c":
						console.log('询问当前用户id')
						break;




				}
			},
			handleSubmit() {
				// 老师的账号和课程id的记录
			},
			echartsInit() {
				// 定时器
				this.timer3 = setTimeout(() => {
					this.timer1 = setInterval(() => {

						this.option1.xAxis[1].data = this.option1.xAxis[1].data.map(item => {
							item += 2
							return item
						})
						// this.optio.n1.xAxis[1].data.push(1)
					}, 2000)
				}, 8000)

			},
			random(min, max) {
				return Math.floor(Math.random() * (max - min)) + min
			},
			// 初始化蓝牙适配器
			openBluetoothAdapter() {
				uni.openBluetoothAdapter({
					success: (res) => {
						// 蓝牙适配器初始化成功
						console.log('openBluetoothAdapter success', res);
						this.startBluetoothDevicesDiscovery()
					},
					fail: (err) => {
						// 蓝牙适配器初始化失败
						uni.showToast({
							title: '请打开蓝牙',
							duration: 1000,
							icon: 'none'
						})
						if (err.errCode === 10001) {
							// 监听蓝牙适配器状态改变
							uni.onBluetoothAdapterStateChange((res) => {
								console.log('BluetoothAdapterStateChange', +res);
								if (res.available) {
									this.startBluetoothDevicesDiscovery()
								}

							})
						}
					}
				})
			},
			// 开始搜寻蓝牙外围设备
			startBluetoothDevicesDiscovery() {

				if (deviceDiscover) {
					return
				}
				deviceDiscover = true
				uni.startBluetoothDevicesDiscovery({
					allowDuplicatesKey: false,
					success: (res) => {
						console.log('startBluetoothDevicesDiscovery', res);
						this.onBluetoothDeviceFound();
					}
				})
			},
			// 监听到寻找到的新设备
			onBluetoothDeviceFound() {
				let that = this
				uni.showLoading({
					title: '正在搜索设备'
				})
				uni.onBluetoothDeviceFound(function(res) {
					res.devices.forEach(device => {
						if (!device.name && !device.localName) {
							return
						}
						console.log('device', device);
						//如果名字相同连接设备
						if (device.name == devicename) {
							that.createBLEConnection(device.deviceId);
							that.deviceId = device.deviceId
							uni.hideLoading()
							// console.log('ddddd')
							that.stopBluetoothDevicesDiscovery()
							uni.showLoading({
								title: '搜索到设备正在连接'
							})
						}
					})

				})
			},
			// 创建连接
			createBLEConnection(deviceId) {
				uni.createBLEConnection({
					deviceId: deviceId,
					success: (res) => {
						uni.showToast({
							title: '连接成功'
						})
						console.log('createBLEConnection', res);
						setTimeout(() => {
							this.getBLEDeviceServices(deviceId);
						}, 1000)
					},
					fail: (err) => {
						console.log('创建连接失败')
					}
				})

			},
			// 停止蓝牙搜索
			stopBluetoothDevicesDiscovery() {
				uni.stopBluetoothDevicesDiscovery({
					success: (res) => {
						console.log('停止蓝牙设备搜索')
					}
				})
			},
			// 获取蓝牙所有服务
			getBLEDeviceServices(deviceId) {
				// console.log(111,deviceId)
				let that = this;
				uni.getBLEDeviceServices({
					deviceId: deviceId,
					success: (service) => {
						let all_UUID = service.services; //取出所有的服务
						// console.log(service)
						console.log('所有的服务', all_UUID);
						let UUID_lenght = all_UUID.length; //获取到服务数组的长度
						/* 遍历服务数组 */
						for (let index = 0; index < UUID_lenght; index++) {
							let ergodic_UUID = all_UUID[index].uuid; //取出服务里面的UUID
							let UUID_slice = ergodic_UUID.slice(4, 8); //截取4到8位
							// console.log(11111,UUID_slice)
							/* 判断是否是我们需要的FEE0 */
							if (UUID_slice == 'FEE0' || UUID_slice == 'fee0') {
								let index_uuid = index;
								that.serviceId = all_UUID[index_uuid].uuid //确定需要的服务UUID

							};
						};
						console.log('需要的服务UUID', that.serviceId)
						that.getBLEDeviceCharacteristics(); //调用获取特征值函数
					},
					fail: (err) => {
						console.log('获取服务失败', err)
					}

				})
			},
			// 获取所有服务的特征值
			getBLEDeviceCharacteristics() {
				let that = this;
				let device_characteristics = [];
				let characteristics_uuid = {};
				// console.log('deviceId' + this.deviceId)
				// console.log('serviceId' + this.serviceId)
				uni.getBLEDeviceCharacteristics({
					deviceId: this.deviceId,
					serviceId: this.serviceId,
					success: (res) => {
						let characteristics = res.characteristics; //获取到所有特征值
						let characteristics_length = characteristics.length; //获取到特征值数组的长度
						console.log('获取到特征值', characteristics);
						console.log('获取到特征值数组长度', characteristics_length);
						/* 遍历数组获取notycharacteristicsId */
						for (let index = 0; index < characteristics_length; index++) {
							let noty_characteristics_UUID = characteristics[index].uuid; //取出特征值里面的UUID
							let characteristics_slice = noty_characteristics_UUID.slice(4, 8); //截取4到8位
							// console.log('id'+characteristics_slice)
							/* 判断是否是我们需要的FEE1 */
							if (characteristics_slice == 'FEE1' || characteristics_slice == 'fee1') {
								let index_uuid = index;
								that.notycharacteristicsId = characteristics[index_uuid].uuid //需确定要的使能UUID
								that.characteristicsId = characteristics[index_uuid].uuid //暂时确定的写入UUID
								// console.log('id1'+characteristicsId)
								/* 遍历获取characteristicsId */
								for (let index = 0; index < characteristics_length; index++) {
									let characteristics_UUID = characteristics[index].uuid; //取出特征值里面的UUID
									let characteristics_slice = characteristics_UUID.slice(4, 8); //截取4到8位
									/* 判断是否是我们需要的FEE2 */
									if (characteristics_slice == 'FEE2' || characteristics_slice == 'fee2') {
										let index_uuid = index;
										that.characteristicsId = characteristics[index_uuid].uuid //确定的写入UUID

									};
								};
							};
						};
						console.log('使能characteristicsId', that.notycharacteristicsId);
						console.log('写入characteristicsId', that.characteristicsId);
						that.notycharacteristics(); //使能事件

					},
					fail: (err) => {
						console.log('getBLEDeviceCharacteristics', err)
					}

				})
			},
			/* 使能函数 */
			notycharacteristics() {
				let that = this;
				let recv_value_ascii = "";
				let string_value = "";
				let recve_value = "";
				uni.notifyBLECharacteristicValueChange({
					deviceId: that.deviceId,
					serviceId: that.serviceId,
					characteristicId: that.notycharacteristicsId,
					state: true,
					success: (res) => {
						console.log('使能成功', res);
						/* 设备返回值 */
						uni.onBLECharacteristicValueChange((res) => {

							let result = res.value;
							let hex = that.buf2hex(result);
							hex = hex + msg
							// console.log('返回的值', hex);
							let resObj = that.parse(hex)
							// console.log(hex)
							msg = resObj.slice
							
							resObj.strArr.map((item, index) => {
								let category = item.slice(6, 8)
								// 
								that.switchFun(category, item)
							})
						});
					},

					fail: function(res) {
						console.log('使能失败', res);
					}
				})
			},


			/* ArrayBuffer类型数据转为16进制字符串 */
			buf2hex(buffer) { // buffer is an ArrayBuffer
				return Array.prototype.map.call(new Uint8Array(buffer), x => ('00' + x.toString(16)).slice(-2)).join('');
			},



			parse02(str) {
				let i = 8;
				let pressDeep = parseInt(str.slice(8, 10), 16) //按压距离
				let cui = parseInt(parseInt(str.slice(10, 12), 16) << 8) + parseInt(str.slice(12, 14), 16)
				let position = str.slice(14, 16)
				return {
					pressDeep,
					cui,
					position
				}
			},
			parse(str) {
				let exp = /a55a/g
				let tap = true;
				let indexArr = []
				let strArr = []
				let obj = {}
				while (tap) {
					let res = exp.exec(str);
					if (res) {
						indexArr.push(res.index)
					} else {
						tap = false;
					}
				}

				for (let i = 0; i < indexArr.length; i++) {
					// 获取长度
					let length = parseInt(str.slice(indexArr[i] + 4, indexArr[i] + 6), 16) * 2 + 6
					// indexArr[i]+4==str.length?length=0:length=length
					// console.log(indexArr[i]+length)
					if (indexArr[i] + length - 1 < str.length) {
						let strRes = str.slice(indexArr[i], indexArr[i] + length)
						strArr.push(strRes)
						if (i == indexArr.length - 1) {
							strRes = str.slice(indexArr[i] + length)
							obj.slice = strRes
							// console.log(strRes)
						}
					} else {
						let strRes = str.slice(indexArr[i])
						obj.slice = strRes
						// console.log(indexArr[i]+length)
					}

				}
				// if (indexArr.pop())
					return { 
						strArr,
						...obj
					}
			},


			handleStartChange() {

				this.isStart = !this.isStart

				if (this.isStart) {
					this.option1.series[0].data = []
					this.startTime = Date.now()
					this.echartsInit()
				} else {
					// 结束
					clearTimeout(this.timer3)
					clearInterval(this.timer1)
					// clearInterval(this.timer2)
					this.endTime = Date.now()

					let str = this.dataArray.join()
					console.log(str)
				}
			},
			hanleTagger(id) {
				this.modeList = this.modeList.map(item => {
					item.id === id ? item.isCurrent = true : item.isCurrent = false;
					return item
				})
				// 考试的时候输入老师账号和课程id
				console.log(id)
				if (id == 2) {
					this.$refs.popup.show()
				}
			},
			//上传训练数据
			submitTrain() {
				uni.request({
					url: '/record/insert',
					method: 'POST',
					data: {

					}
				})
			},

		}
	}
</script>

<style lang="scss">
	@import './index.scss'
</style>
