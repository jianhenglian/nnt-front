<template>
	<view>
		<view :class="getClass">
			<!-- 状态栏 -->
			<view :style="'height:'+statusBarHeight+'px'"></view>
			<!-- 导航 -->
			<view class="w-100 flex align-center justify-between" style="height: 90rpx;">
				<!-- 左边 -->
				<view class="flex align-center">
					<!-- 返回按钮 -->
					<!-- #ifndef MP -->
					<free-icon-button v-if="showBack" @click="back"
					:icon="'\ue60d'"></free-icon-button>
					<!-- #endif -->
					<!-- 标题 -->
					<slot>
						<text v-if="title" class="font-md ml-3">{{getTitle}}</text>
					</slot>
				</view>
				<!-- 右边 -->
				<view class="flex align-center" v-if="showRight">
					<slot name="right">
						<free-icon-button @click="search" 
						:icon="'\ue6e3'"></free-icon-button>
						<free-icon-button @click="openExtend"
						:icon="'\ue682'"></free-icon-button>
					</slot>
				</view>
			</view>
		</view>
		<!-- 占位 -->
		<view v-if="fixed" :style="fixedStyle"></view>
		
		<!-- 扩展菜单 -->
		<free-popup v-if="showRight" ref="extend" :bodyWidth="320" :bodyHeight="525"
		bodyBgColor="bg-dark" transformOrigin="right top">
			<view class="flex flex-column" 
			style="width: 320rpx;height: 525rpx;">
				<view class="flex-1 flex align-center" 
				hover-class="bg-hover-dark"
				v-for="(item,index) in menus"
				:key="index"
				@click="clickEvent(item)">
					<text class="iconfont pl-3 pr-2 font-md text-white">{{item.icon}}</text>
					<text class="font-md text-white">{{item.name}}</text>
				</view>
			</view>
		</free-popup>
		
		
	</view>
</template>

<script>

	export default {
		name:'my-navbar',
		props: {
			showBack:{
				type:Boolean,
				default:false
			},
			backEvent:{
				type:Boolean,
				default:true
			},
			title: {
				type: [String,Boolean],
				default:false 
			},
			fixed:{
				type:Boolean,
				default:true
			},
			noreadnum:{
				type:[Number,String],
				default:0
			},
			bgColor:{
				type:String,
				default:"bg-light"
			},
			showRight:{
				type:Boolean,
				default:true
			}
		},
		data() {
			return {
				statusBarHeight:0,
				navBarHeight:0,
				menus:[
					{
						name:"发起群聊",
						event:"navigateTo",
						path:"/pages/mail/mail/mail?type=createGroup",
						icon:"\ue633"
					},
					{
						name:"添加好友",
						event:"navigateTo",
						path:"/pages/common/search/search",
						icon:"\ue65d"
					},
					// #ifndef H5
					{
						name:"扫一扫",
						event:"",
						icon:"\ue614"
					},
					// #endif
					{
						name:"收付款",
						event:"",
						icon:"\ue66c"
					},
					{
						name:"帮助与反馈",
						event:"",
						icon:"\ue66c"
					}
				],
			}
		},
		mounted() {
			// #ifdef APP-PLUS-NVUE
			this.statusBarHeight = plus.navigator.getStatusbarHeight()
			// #endif
			this.navBarHeight = this.statusBarHeight + uni.upx2px(90)
		},
		computed: {
			fixedStyle() {
				return `height:${this.navBarHeight}px`
			},
			getTitle(){
				let noreadnum = this.noreadnum > 0 ? '('+this.noreadnum+')' : ''
				return this.title + noreadnum
			},
			getClass(){
				let fixed = this.fixed?'fixed-top':''
				return `${fixed} ${this.bgColor}` 
			}
		},
		methods: {
			openExtend() {
				this.$refs.extend.show(uni.upx2px(415),uni.upx2px(150))
			},
			// 返回
			back(){
				if(this.backEvent){
					return uni.navigateBack({
						delta: 1
					});
				}
				this.$emit('back')
			},
			search(){
				uni.navigateTo({
					url: '/pages/common/search/search'
				});
			},
			clickEvent(item){
				this.$refs.extend.hide()
				switch (item.event){
					case 'navigateTo':
					uni.navigateTo({
						url: item.path,
					});
						break;
					default:
					uni.showToast({
						title: '靓仔，自己发挥',
						icon: 'none'
					});
						break;
				}
			}
		},
	}
</script>

<style>
</style>
