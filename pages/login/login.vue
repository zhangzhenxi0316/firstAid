<template>
	<view>
		<view class="main">
			<view class="header">
				<view class="login">登录</view>
				<navigator url="/pages/registry/registry">
					<view class="registry">注册</view>
				</navigator>
				
			</view>
			<view class="login_input">
				<view class="item">
					<input type="text" :placeholder="content" v-model="username">
				</view>
				<view class="item">
					<input type="password" placeholder="密码" v-model="password">
				</view>
				<view class="select">
					<picker class="picker" mode="selector" :range="['老师','学生','用户']" @change="handleChange">
						<view>{{['老师','学生','用户'][index]}}</view>
					</picker>
					<uni-icons class="icons" type="arrowdown"></uni-icons>
				</view>
				<view class="btn" @click="handleSubmit">登录</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				index:0,
				username:'',
				password:'',
				token:null
			}
		},
		computed:{
			content(){
				switch(this.index){
					case 0 :return '请输入教师账号';
					case 1 :return '请输入学生学号';
					case 2 :return '请输入用户名'
					default:return '请选择角色'
				}
			}
		},
		methods: {
			handleChange(e) {
				console.log( e.detail.value)
				this.index = e.detail.value
			},
			handleSubmit(){
				console.log(this.state)
				if(this.username==='',this.password===''){
					uni.showToast({
						icon:'none',
						title:'用户名或密码不能为空'
					})
					return
				}
				uni.showLoading({
					mask:true,
					title:'正在登录'
				})
				let data;
				let url;
				switch(this.index){
					case 0:
					data={
						teaAccount:this.username,
						teaPassword:this.password
					}
					this.token&&(data.teacherToken= this.token)
					url='/teacher/login'
					break;
					case 1:
					data={
						stuOn:parseInt(this.username),
						stuPassword:this.password
					}
					url='/student/login'
					this.token&&(data.stuToken= this.token)
					break;
					case 2:
					data={
						userName:this.username,
						password:this.password
					}
					this.token&&(data.userToken= this.token)
					url='/user/login'
					break;
				}
				
				uni.request({
					url,
					data,
					method:'POST',
					success: () => {
						console.log(url,data)
						uni.hideLoading({})
						uni.showToast({
							icon:'success',
							title:'登录成功',
							
						})
						setTimeout(()=>{
							uni.navigateTo({
								url:'/pages/index/index'
							})
						},1000)
							
						
						
					},
					fail: () => {
						uni.showToast({
							icon:'none',
							title:'用户名密码错误'
						})
						setTimeout(()=>{
							uni.navigateTo({
								url:'/pages/index/index'
							})
						},1000)
					}
				})
				
			}
		}
	}
</script>

<style lang="scss" scoped>
@import './index.scss'
</style>
