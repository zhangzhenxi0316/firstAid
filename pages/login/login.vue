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
					<input type="text" placeholder="学号/账号">
				</view>
				<view class="item">
					<input type="password" placeholder="密码">
				</view>
				<view class="btn">登录</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
@import './index.scss'
</style>
