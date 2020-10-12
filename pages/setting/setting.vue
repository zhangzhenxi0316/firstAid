<template>
	<view class="wrapper">
		<!-- 头部开始 -->
		<view class="header">
			<view class="iconWrapper">
				<image src="../../static/image/heartbeat.svg" mode="aspectFit"></image>
			</view>
			<text>CPR设置</text>
		</view>
		<!-- 头部结束 -->
		<!-- 主体部分开始 -->
		<view class="main">
			<view class="manifest_item">
				<!-- 每一项设置得题目 -->
				<view class="title">常规设置</view>
				<view class="manifest_list">
					<view class="input_wrapper">
						<!-- <view class="input_title">最大用时</view>
						<view class="input">
							<input type="text" value="" /> 秒
						</view> -->
						<inputComponent :defaultV="String(setting.usual.maxTime)" type="usual" @hanleInputChange="handleMaxChange" class="item" title="最大用时 " :arr="numArr" ></inputComponent>
						<inputComponent :defaultV="String(setting.usual.TotalLoop)" type="usual"  @hanleInputChange="handleLoopChange"  class="item" title="总循环周期 " :arr="numArr"></inputComponent>
						<radio-group class="radioGroup" name=""   @change="handleRadioChange">
							<label class="item">
								<radio value="true" /><text>只有操作成功，模型才复活</text>
							</label>
							<label class="item">
								<radio value="false" /><text>无论操作成败，模型均复活</text>
							</label>
						</radio-group>
					</view>
					
				</view>
			</view>
			<!-- 按压设置 -->
			<view class="manifest_item"> 
				<!-- 每一项设置得题目 -->
				<view class="title">按压设置</view>
				<view class="manifest_list">
					<view class="input_wrapper">
						
						<view class="press_wrapper">
							<!-- <view class="left"></view> -->
							<view class="Two">
								<inputComponent :defaultV="String(setting.press.pressStart)"  @hanleInputChange="handleDeepStartChange"   type="press" class="item" title="按压深度 " unit="至" :arr="percent" ></inputComponent>
								<inputComponent :defaultV="String(setting.press.pressEnd)"  @hanleInputChange="handleDeepEndChange" width="0" type="press"  title=" " unit="厘米" :arr="percent" ></inputComponent>
							</view>
							<inputComponent  :defaultV="String(setting.press.pressPercent)"  @hanleInputChange="handlePercentChange"  type="press"  class="item" title="按压达标率 " unit="%" :arr="numArr"></inputComponent>
							
							<inputComponent  :defaultV="String(setting.press.first)"  @hanleInputChange="handleTimeStartChange" type="press"  class="item" title="首次按压中断时间上限 " width="140" :arr="numArr"></inputComponent>
							<!-- <inputComponent  :defaultV="String(setting.press.other)" @hanleInputChange="handleTimeOtherChange"  type="press"  class="item" title="其他按压中断时间上限 " width="140" :arr="numArr"></inputComponent> -->
						</view>
						
					</view>
					
				</view>
			</view>
			<!-- 吹气设置 -->
			<view class="manifest_item">
				
				<view class="title">吹气设置</view>
				<view class="manifest_list">
					<view class="input_wrapper">
							<view class="Two">
								<inputComponent :defaultV="String(setting.cui.start)"  @hanleInputChange="handleDeepStartChange"   type="cui" class="item" title="吹气量 " unit="至" :arr="cui" ></inputComponent>
								<inputComponent :defaultV="String(setting.cui.end)"  @hanleInputChange="handleDeepEndChange" width="0" type="cui"  title=" " unit="毫升" :arr="cui" ></inputComponent>
							</view>
							<inputComponent :defaulVt="String(setting.cui.percent)"  @hanleInputChange="handlePercentChange" type="cui" class="item" title="吹气达标率 " unit="%" :arr="percent"></inputComponent>
						
					</view>
					
				</view>
			</view>
			<!-- aed -->
			<view class="manifest_item">
				
				<view class="title">AED使用设置</view>
				<view class="manifest_list">
					<view class="input_wrapper">
							<inputComponent :defaultV="setting.aed.isOpen?'是':'否'"  @hanleInputChange="handleOpenChange" type="aed" class="item" title="门是否常开模式" width="160"  unit="" :arr="is" ></inputComponent>
							<inputComponent :defaultV="String(setting.aed.Loop)"  @hanleInputChange="handleLoopChange"  type="aed" class="item" title="AED使用开始在第几个循环" width="160" unit="" :arr="cui"></inputComponent>
							<inputComponent  :defaultV="setting.aed.isPrint?'是':'否'" @hanleInputChange="handlePrintChange" type="aed" class="item" title="考试完成是否及时打印" width="160" unit="" :arr="is"></inputComponent>
						
					</view>
					
				</view>
			</view>
		</view>
		<!-- 主体部分结束 -->
		<view class="footer">
			<view class="left">
				<view class="item1">赛制标准</view>
				<view class="item1">重置</view>
			</view>
			<view class="right">
				<navigator url="/pages/index/index">
					<view class="item2">取消</view>
				</navigator>
				<view class="item2" @click="handleSumit">确定</view>
			</view>
		</view>
	</view>
</template>

<script>
	let f = length => Array.from({length}).map((v, k) => k);
	let arr = f(1000)
	//达标率
	let percent = f(101)
	//吹气量
	let cui = f(20).map(item=>item*100)
	//aed
	let aed = f(20)
	
	export default {
		data() {
			return {
				numArr:arr,
				percent:percent,
				cui:cui,
				is:['是','否'],
				aed:aed,
				
				setting:{
					usual:{
						maxTime:150,
						TotalLoop:5,
						// 单选第一个为true
						isCorrent:1,
					},
					press:{
						pressStart:0,
						pressEnd:0,
						pressPercent:70,
						first:0,//最大按压中断时间
						
					},
					cui:{
						start:0,
						end:0,
						percent:70
					},
						
					aed:{
						isOpen:true,
						Loop:2,
						isPrint:true
					}
					
					
				}
			}
		},
		onLoad() {
			// console.log(this.cui)
			// this.numArr = arr
		},
		methods: {
			handleSumit(){
				uni.showLoading({
					title:"正在上传设置"
				})
				let data = {
					maxUseTime:this.setting.usual.maxTime,
					totalCycle:this.setting.usual.TotalLoop,
					modelResurrection:this.setting.usual.isCorrent,
					pressDepthMin:this.setting.press.pressStart,
					pressDepthMax:this.setting.press.pressEnd,
					pressSuccessRate:this.setting.press.pressPercent,
					pressMaxPauseTime:this.setting.press.first,
					blowAirMin:this.setting.cui.start,
					blowAirMax:this.setting.cui.end,
					blowSuccessRate:this.setting.cui.percent,
					aedUse:this.setting.aed.Loop,
					doorAlwaysOpen:this.setting.aed.isOpen?1:0,
					testCompletedInstantPrint:this.setting.aed.Print?1:0
				}
				console.log(data)
				uni.request({
					url:'http://set/insertOrUpdate',
					data:data,
					method:"POST",
					success: () => {
						uni.hideLoading()
							uni.showToast({
								icon:'none',
								title:'数据上传成功'
							})
					},
					fail: () => {
						uni.hideLoading()
						uni.showToast({
							icon:'none',
							title:'数据上传失败'
						})
					}
				})
				
			},
			handleMaxChange(e){
				this.setting.usual.maxTime = e.value
			},
			handleLoopChange(e){
				if(e.type === 'usual')this.setting.usual.TotalLoop = e.value;
				else this.setting.aed.Loop = e.value
			},
			handlePercentChange(e){
				this.setting.press.pressPercent = e.value
			},
			handleTimeStartChange(e){
				this.settting.press.first = e.value
			},
			// handleTimeOtherChange(e){
			// 	this.settting.press.other = e.value
			// },
			handlePercentChange(e){
				this.setting.cui.percent = e.value
			},
			handleOpenChange(e){
				this.setting.aed.isOpen = e.value==='是'?true:false
			},
			handlePrintChange(e){
				this.setting.aed.isPrint = e.value==='是'?true:false
			},
			handleDeepStartChange(e){
				if(e.type==='press')this.setting.press.pressStart = e.value;
				else this.setting.cui.start = e.value
			},
			handleDeepEndChange(e){
				if(e.type==='press')this.setting.press.pressEnd = e.value;
				else this.setting.cui.end = e.value
			},
			handleRadioChange(e){
				// console.log(e)
				this.setting.usual.isCorrent = e.detail.value ==='true'?true:false
			}
		}
	}
</script>

<style  lang="scss">
@import "./index.scss"
</style>
