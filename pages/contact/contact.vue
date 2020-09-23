<template>
	<view class="contact">
		<image class="img" src="http://www.itcast.cn/2018czydz/images/gywmban.jpg"></image>
		<view class="info">
			<view @click="phone">联系电话: 400-618-9090 ( 点击拨打 )</view>
			<view>地址：天津市和平区滨江道170号米莱欧百货4层1号商铺 </view>
		</view>
		<map class="map" :longitude="longitude" :latitude="latitude" :markers="markers"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			  latitude:39.132439,
			  longitude:117.207196,
			  markers:[
				  {
		          latitude:39.132439,
		          longitude:117.207196,
			      iconPath:'../../static/hmlogo.png',
				  height:30,
				  width:30
				  }
			  ]
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber:'400-618-9090'
				})
			}
		}
	}
</script>

<style lang="scss">
.contact {
	.img {
		width: 750rpx;
		height: 320rpx;
	}
	.info {
		padding:10rpx 20rpx;
		font-size: 30rpx;
		view{
			line-height: 80rpx;
			border-bottom: 1px solid #eee;
		}
	}
	.map {
		width: 750rpx;
		height: 750rpx;
	}
}
</style>
