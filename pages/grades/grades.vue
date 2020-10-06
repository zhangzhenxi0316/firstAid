	
<template>
	<view class="">
		
	
	<!-- <web-view src="https://zhangzhenxi0316.github.io/"></web-view> -->
	
	<view class="grades_wrapper">
		<view class="header_wrapper">
			<view class="header_title">
				<uni-icons type=""></uni-icons>
				个人成绩单
			</view>
			
			<view class="print_btn">
				打印
				
			</view>
			<navigator url="/pages/index/index">
				<view class="icon">
					× 
				</view>
			</navigator>
			
		</view>
		
		<view class="header_bottom">
			<view class="title">
				心脏复苏报告单
			</view>
			<view class="date">
				2019-07-10 02:10:15 (1234567890)
			</view>
		</view>
		
		<view class="bottom_wrapper">
			<view class="bottom_left">
				<view class="title">
					操作流程
				</view>
				
				<view class="left_item">
					<uni-icons type="checkbox" size="20px" color="#02FF39"></uni-icons>
					<text>意识判断</text>
				</view>
				<view class="left_item">
					<uni-icons type="checkbox" size="20px" color="#02FF39"></uni-icons>
					<text>动脉触摸</text>
				</view>
				<view class="left_item">
					<uni-icons type="close" size="20px" color="#FD0000"></uni-icons>
					<text>开放气道</text>
				</view>
			</view>
			
			<view class="bottom_right" >
				<view class="right_item">
					<view>项目</view>
					<view>操作标准</view>
					<view>操作标准</view>
					<view>评分</view>
				</view>
				<view class="right_item">
					<view>用时</view>
					<view>最大用时150s</view>
					<view>用时118s</view>
					<view></view>
				</view>
				
				<view class="right_item">
					<view>循环</view>
					<view>循环5次</view>
					<view>循环1次</view>
					<view></view>
				</view>
				
				<view class="right_item">
					<view>按压</view>
					<view>
						达标率70%<br>
						按压深度4-5cm<br>
					</view>
					<view>
						总6次<br>
						错误6次； 正确6次<br>
						过大1次； 过小0次<br>
						位置错误6次<br>
						未充分回弹0次<br>
						多按0次； 少按144次<br>
					</view>
					<view>正确率0%</view>
				</view>
				
				<view class="right_item">
					<view>按压中断</view>
					<view>
						首次中断25s<br>
						其余中断10s<br>
					</view>
					<view>
						首次按压中断大于25秒0次<br>
						其余按压中断大于25秒0次<br>
					</view>
					<view></view>
				</view>
				
				<view class="right_item">
					<view>吹气</view>
					<view>
						达标率70%<br>
						吹气量450-650ml<br>
					</view>
					<view>
						总36次<br>
						错误36次； 正确0次<br>
						过大0次； 过小19次<br>
						多吹0次； 少吹19次<br>
					</view>
					<view>正确率0%</view>
				</view>
				
				<view class="right_item">
					<view>除颤</view>
					<view>
						除颤能量200-300焦耳<br>
						至少除颤2次<br>
					</view>
					<view>
						总除颤0次，有效除颤0次<br>
					</view>
					<view></view>
				</view>
				
				<view class="right_item">
					<view>按压频率</view>
					<view>
						平均值 :  128次/分； 错误:  119次<br>
					</view>
					<view>
					</view>
					<view></view>
				</view>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				RightHeight:''
			}
		},
		methods: {
		},
		onReady(){
			
			// let rightDOM = document.getElementsByClassName('bottom_right')[0].offsetHeight;
			// this.RightHeight = rightDOM;
			// let leftDOM = document.getElementsByClassName('bottom_left')[0]
			// console.log(leftDOM)
			// leftDOM.style.height = rightDOM + 'px';
			
		}
	}
</script>

<style>
	@import url("grades.css");
</style>
