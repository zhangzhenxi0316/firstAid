<template>
	<view>
		<view class="main">
			<view class="header">
				<navigator url="/pages/login/login">
					<view class="login">登录</view>
				</navigator>
				<view class="registry">注册</view>
			</view>
			<view class="login_input">
				<view class="borderBottom">
					<view class="item" v-show="this.index==0">
						<input type="text" placeholder="老师账号" v-model="username">
					</view>
					<view class="item" v-show="this.index==1">
						<input type="number" placeholder="学生学号" v-model="username">
					</view>
					<view class="item">
						<input type="text" :placeholder="this.index==2?'用户名':'姓名'" v-model="name" @change="handlePasswordChange">
					</view>
					<view class="item">
						<input type="password" placeholder="密码" v-model="password" @change="handlePasswordChange">
					</view>

					<view class="currentPassword">
						<view class="item">
							<input type="password" placeholder="确认密码" v-model="currentPassword" @change="handlePasswordChange">
						</view>
						<uni-icons class="icon_password" :type="this.current?'checkbox':'close'" :color="this.current?'green':'red'" v-show="this.password.length>0&&this.currentPassword.length>0"></uni-icons>
					</view>
					<view class="item" v-show="this.index!==2">
						<input type="text" placeholder="性别" v-model="sex">
					</view>
					<view class="item" v-show="this.index ==0">
						<input type="number" placeholder="年龄" v-model="age">
					</view>
					<view class="item">
						<input type="text" placeholder="所属部门" v-model="department">
					</view>
					<view class="item" v-show="this.index==1">
						<input type="text" placeholder="所属班级" v-model="teaClass">
					</view>
					<view class="item" v-show="this.index==0">
						<input type="number" placeholder="教授的课程id" v-model="courseId">
					</view>
					<view class="item" v-show="this.index==2">
						<input type="number" placeholder="电话" v-model="phone">
					</view>
				</view>
				<view class="select">
					<picker class="picker" mode="selector" :range="['老师','学生','用户']" @change="handleChange">
						<view>{{['老师','学生','用户'][index]}}</view>
					</picker>
					<uni-icons class="icons" type="arrowdown"></uni-icons>
				</view>
				<view class="btn" @click="handleSumbit">登录</view>
			</view>
		</view>
	</view>
</template>

<script>
	// 0代表老师 1学生 2 用户
	export default {
		data() {
			return {
				name:'',
				index: 0,
				username: '',
				password: '',
				currentPassword: '',
				current: true,
				sex: '',
				department: '',
				age: '',
				teaClass: '',
				courseId: '',
				phone: ''
			}
		},
		methods: {
			handleChange(e) {
				this.index = e.detail.value
			},
			handlePasswordChange() {
				console.log(1)
				this.current = this.password === this.currentPassword ? true : false
				console.log(this.current)
			},
			handleSumbit() {
				uni.showLoading({
					title:'正在注册'
				})
				let url;
				let data;
				if(!this.current){
					uni.showToast({
						icon:'none',
						title:'两次密码不一样'
					})
					return
				}
				if(this.username===''||this.password===''||this.department===''){
					uni.showToast({
						icon:'none',
						title:'请填写所有注册资料'
					})
					return 
				}
				switch (this.index) {
					case 0:
					// 老师
					if(this.sex==''&&this.age==''||this.name==''||this.courseId==''){
						uni.showToast({
							icon:'none',
							title:'请填写所有注册资料'
						})
						return 
					}
					url = '/teacher/register'
					data = {
						teaName:this.name,
						teaAge:this.age,
						teaAccount:this.username,
						teaPassword:this.password,
						teaSex:this.sex,
						teaCourse:parseInt(this.courseId),
						teaDepart:this.department
					}
					
						break;
					case 1:
					// 学生
					if(this.sex==''&&this.name===''||this.teaClass==''){
						uni.showToast({
							icon:'none',
							title:'请填写所有注册资料'
						})
						return 
					}
					url = '/student/register'
					data = {
						stuOn:parseInt(this.username),
						stuName:this.name,
						stuAge:this.age,	
						stuPassword:this.password,
						stuSex:this.sex,
						stuClass:this.teaClass,
						stuDepartment:this.department
					}
						break;
					case 2:
					if(this.Phone==''){
						uni.showToast({
							icon:'none',
							title:'请填写所有注册资料'
						})
						return
					}
					url='/user/registry';
					data={
						userName:this.username,
						password:this.password,
						Phone:this.phone,
						Depart:this.department
					}
					
						break;
					default:
						break;
				}
				// data.status = this.state.index
				uni.request({
					method: 'POST',
					url,data
					
				}).then(res=>{
					uni.hideLoading()
					uni.showToast({
						icon:'success',
						title:'注册成功'
					})
					setTimeout(()=>{
						uni.navigateTo({
							url:'/pages/login/login'
						})
					},1000)
				}).catch(err=>{
					uni.showToast({
						title:'注册失败',
						icon:'none'
					})
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './index.scss'
</style>
