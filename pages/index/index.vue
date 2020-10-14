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
					<input type="text"  placeholder="请输入老师账号" v-model="teacherAccount"/>
					<input type="text"  placeholder="请输入课程id" v-model="courseId" />
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

	let startNum = 9
	for (var i = 0; i < 200; i++) {
		xDate[i] = i * 50 / 1000
	}


	export default {
		data() {
			return {
				teacherAccount:'',
				courseId:'',
				//蓝牙搜索设备列表
				bluetoothLinks: [],

				//设备ID
				deviceID: '',

				//设备服务ID列表
				serviceList: [],

				//FFE0服务seviceID
				serviceId: '',

				//FFE0服务特征值characteristics
				characteristics: [],

				//写服务特征值
				characteristicId: "",
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
							position: 'bottom',
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
						max: 300
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

		},
		onShow() {
			uni.openBluetoothAdapter({
							success:()=> {
								console.log("蓝牙模板初始化成功")
								this.startBluetoothDeviceDiscovery()
								this.bluetoothLinks = []
							},
							fail:() => {
								//如果用户未开启蓝牙权限，弹窗提示
								uni.showToast({
									title: '请先打开蓝牙',
									icon:"none"
								});
							}
						})

		},
		methods: {
			handleSubmit(){
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

					}, 2000)
				}, 8000)
				this.timer2 = setInterval(() => {
					let randomNum = this.random(5, 150)
					if (this.option1.series[0].data.length > 200) {
						this.option1.series[0].data.shift()
					}
					// console.log(this.option1.series[0].data)
					this.option1.series[0].data.push(randomNum)


				}, 50)
			},
			random(min, max) {
				return Math.floor(Math.random() * (max - min)) + min
			},
			startBluetoothDeviceDiscovery(){
							//开始搜寻附近的蓝牙外围设备
							uni.startBluetoothDevicesDiscovery({
								success: res => {
									console.log(res)
									this.onBluetoothDeviceFound();
								},
								fail: res => {
									uni.showToast({
										icon: "none",
										title: "查找设备失败！",
										duration: 3000
									})
								}
							});
						},
						
						//监听外围设备
						onBluetoothDeviceFound(){
							uni.onBluetoothDeviceFound((devices)=>{
								if(devices.devices[0].name){
									//将有name的蓝牙设备保存到devices中
									let device = {
										name:devices.devices[0].name,
										deviceID:devices.devices[0].deviceId
									}
									this.bluetoothLinks.push(device)
									
									//检测是否查找到需要连接的设备,name = "目标蓝牙名称"
									let hasBluetooth = this.bluetoothLinks.some((item)=>item.name = "zzx")
									if(hasBluetooth){
										//筛选出需要连接的设备信息
										let deviceInfo = this.bluetoothLinks.filter((item)=>{
											//name = "目标蓝牙名称"
											return item.name="zzx"
										})
										this.stopBluetoothDevicesDiscovery()
										this.createBLEConnection(deviceInfo[0].deviceID)
									}
								}
							})
						},
						
						//停止蓝牙搜索
						stopBluetoothDevicesDiscovery(){
							uni.stopBluetoothDevicesDiscovery({
							  success:(res)=>{
								console.log("蓝牙停止搜索")
								console.log(this.bluetoothLinks)
							  }
							})
						},
						
						//连接蓝牙
						createBLEConnection(deviceID){
							console.log(deviceID)
							uni.createBLEConnection({
								deviceId:deviceID,
								success:(res)=>{
									console.log("连接目标设备成功")
									
									//监听蓝牙连接状态，成功继续，失败提示
									if(this.onBLEConnectionStateChange()){
										//获取目标蓝牙所有特征值
										setTimeout(()=>{
											this.getBLEDeviceServices()
										},1500)
									}else{
										uni.showToast({
											title:'蓝牙连接',
											icon:"none"
										})
									}
								},
								fail(err) {
									console.log(err)
								}
							})
						},
						
						//监听蓝牙适配器状态
						onBluetoothAdapterStateChange(){
							uni.onBluetoothAdapterStateChange((res)=>{
								console.log(`蓝牙适配器状态: ${res.available},蓝牙适配器搜索状态: ${res.discovering}`)
							})
						},
						//监听蓝牙连接状态
						onBLEConnectionStateChange(){
							uni.onBLEConnectionStateChange((res) => {
								console.log(`设备 ${res.deviceId} 状态改变, 连接状况: ${res.connected}`)
								return res.connected
							})
						},
						//获取目标蓝牙所有特征值
						getBLEDeviceServices(){
							uni.getBLEDeviceServices({
								deviceId:this.deviceID,
								success: (res) => {
									console.log('目标蓝牙服务:', res.services)
									
									//记录所有服务特征值
									this.serviceList = res.services
									
									//遍历结果是否是FFE0服务
									res.services.forEach((item)=>{
										if(item.uuid.indexOf("FFE0")!=-1){
											this.serviceId = item.uuid;
											console.log('serverId:', this.serviceId)
											this.getBLEDeviceCharacteristics()
										}
									})
								},
								fail: (err) => {
									console.log("获取目标蓝牙服务失败:" + err)
								}
							})
						},
						
						//获取特定服务特征值
						getBLEDeviceCharacteristics(){
							uni.getBLEDeviceCharacteristics({
								deviceId:this.deviceID,
								serviceId:this.serviceId,
								success: (res) => {
									console.log("获取的" + this.serviceId + "服务的特征值：" + JSON.stringify(res.characteristics));
									
									this.characteristics = res.characteristics
									
									//遍历寻找"write"特征值FFF1
									res.characteristics.forEach((item)=>{
										if(item.uuid.indexOf("FFE1")!=-1){
											this.characteristicId = item.uuid;
											console.log('characteristicId:', this.characteristicId)
											
											//订阅该特征值
											this.notifyBLECharacteristicValueChange()
										}
									})
								},
								fail: (err) => {
									console.log("获取服务特征值失败" + err)
								}
							})
						},
						
						//启用低功耗蓝牙设备特征值变化时的 notify 功能
						notifyBLECharacteristicValueChange(){
							uni.notifyBLECharacteristicValueChange({
								deviceId:this.deviceID,
								serviceId:this.serviceId,
								characteristicId:this.characteristicId,
								success: (res) => {
									console.log('notifyBLECharacteristicValueChange success', res.errMsg)
									this.onBLECharacteristicValueChange()
								},
								fail: (err) => {
									console.log('notifyBLECharacteristicValueChange fail', res.errMsg)
								}
							})
						},
						
						//监听低功耗蓝牙设备的特征值变化
						onBLECharacteristicValueChange(){
							uni.onBLECharacteristicValueChange((res)=>{
								  console.log(`characteristic ${res.characteristicId} has changed, now is ${res.value}`)
								  console.log(this.ab2hex(res.value))
							})
						},
						
						//ArrayBuffer16进制转换二进制
						ab2hex(buffer){
						  const hexArr = Array.prototype.map.call(
						    new Uint8Array(buffer),
						    function (bit) {
						      return ('00' + bit.toString(16)).slice(-2)
						    }
						  )
						  return hexArr.join('')
						},
						
						//读取二进制数据
						readBLECharacteristicValue(){
							uni.readBLECharacteristicValue({
								deviceId:this.deviceID,
								serviceId:this.serviceId,
								characteristicId:this.characteristicId,
								success: (res) => {
									
								}
							})
						},
						
						//输出二进制数据
						writeBLECharacteristicValue(){
							uni.readBLECharacteristicValue({
								deviceId:this.deviceID,
								serviceId:this.serviceId,
								characteristicId:this.characteristicId,
								success: (res) => {
									
								}
							})
						},


			handleStartChange() {

				this.isStart = !this.isStart

				if (this.isStart) {
					this.option1.series[0].data = []
					this.echartsInit()
				} else {
					clearTimeout(this.timer3)
					clearInterval(this.timer1)
					clearInterval(this.timer2)
				}
			},
			hanleTagger(id) {
				this.modeList = this.modeList.map(item => {
					item.id === id ? item.isCurrent = true : item.isCurrent = false;
					return item
				})
				// 考试的时候输入老师账号和课程id
				console.log(id)
				if(id==2){
					this.$refs.popup.show()
				}
			}
		}
	}
</script>

<style lang="scss">
	@import './index.scss'
</style>
