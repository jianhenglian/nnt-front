<template>
	<view class="flex align-center justify-center" 
	hover-class="bg-hover-light" @click="$emit('click')"
	style="height: 90rpx;width: 90rpx;">
		<text class="iconfont font-md">{{icon}}</text>
	</view>
</template>

<script>
	export default {
		name:'my-icon-button',
		props: {
			icon: {
				type: String,
				default: ''
			},
		},
	}
</script>

<style>
</style>
