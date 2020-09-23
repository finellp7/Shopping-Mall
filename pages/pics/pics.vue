<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view 
			@click="leftClickHandle(index,item.id)"
			:class="active === index?'active':''"
			 v-for="(item,index) in cates" :key="item.id">{{item.title}}</view>
		</scroll-view>
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="(item,index) in secondData" :key="item.id">
				<image :src="item.img_url" @click="preview(item.img_url)"></image>
				<text>{{item.title}}</text>
			</view>
			<text v-if="secondData.length===0">暂无数据</text>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cates: [],
				active:0,
				secondData:[]
			}
		},
		methods: {
			async getPicsCate() {
				const res = await this.$myRequest({
					url:'/api/getimgcategory'
				})
				// console.log(res)
				this.cates = res.data.message
				this.leftClickHandle(0,this.cates[0].id)
			},
			async leftClickHandle(index,id) {
				this.active = index
				// 获取右侧的数据
				const res = await this.$myRequest({
					url:'/api/getimages/'+id
				})
				// console.log(res)
				this.secondData = res.data.message
			},
			preview(current) {
				const urls = this.secondData.map(item=>{
					return item.img_url
					// map方法遍历对象，将返回一个新数组
				})
				console.log(urls)
				uni.previewImage({
					current,
					urls
				})
			}
		},
		onLoad() {
			this.getPicsCate()
		}
	}
</script>

<style lang="scss">
page {
	height: 100%;
}
.pics{
	height: 100%;
	display: flex;
	.left {
		width: 200rpx;
		height: 100%;
		view {
			height: 60px;
			line-height: 60px;
			color: #333;
			text-align: center;
			font-size: 30rpx;
			border-right: 1px solid #eee;
			border-top: 1px solid #eee;
		}
		.active {
			background: $shop-color;
			color: #fff;
		}
	}
	.right {
		height: 100%;
		width: 520rpx;
		margin: 10rpx auto;
		.item {
			border-radius: 5px;
			width: 520rpx;
			height: 540rpx;
			text {
				font-size: 30rpx;
				line-height: 60rpx;
			}
		}
	}
}
</style>
