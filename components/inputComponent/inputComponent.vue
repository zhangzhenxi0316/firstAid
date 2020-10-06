<template>
	<view class="inputWrapper">
		<view class="title" :style="{width:width+'px'}">{{title}}</view>
		<picker class="picker" mode="selector"  :value="index" :range="arr" @change="handleChange">
			<view>{{defaultV||value}}</view>
		</picker>
		{{unit}}
	</view>
</template>

<script>
	export default {
		onLoad() {
			
		},
		props:{
			defaultV:{
				type:String,
				default:''
			},
			type:{
				type:String,
				default:''
			},
			arr:{
				type:Array,
				default(){
				 return[]
				}
			},
			width:{
				type:String,
				default:'65'
			},
			title:{
				type:String,
				default:''
			},
			unit:{
				type:String,
				default:'秒'
			}
		},
		data() {
			return {
				// numArr:num,
				value:'请选择',
				index:0
			};
		},
		methods:{
			handleChange(e){
				console.log(e.target)
				this.index = e.target.value
				this.value = this.arr[this.index]
				console.log(this.value)
				this.$emit('hanleInputChange',{value:this.value,type:this.type})
			}
		}
	}
</script>

<style lang="scss">
	@import './index.scss'

</style>
