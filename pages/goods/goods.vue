<template>
	<view class="goods_list">
		<goods-list :goods="goods" @goodsItemClick="goDetail"></goods-list>
		<view class="isOver" v-if="flag">-----我是有底线的-----</view>
	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				pageindex: 1,
				goods: [],
				flag:false
			}
		},
		components:{
			'goods-list':goodsList
		},
		methods: {
			async getGoodsList(callBack){
	         const res = await this.$myRequest({
		      url:'/api/getgoods?pageindex='+this.pageindex
	          })		
			   console.log(res)
			   this.goods = [...this.goods,...res.data.message]
			   callBack && callBack()
			},
			goDetail (id) {
				uni.navigateTo({
					url:'/pages/goods-detail/goods-detail?id='+id
				})
			}
		},
		onLoad () {
			this.getGoodsList()
		},
		onReachBottom() {
			console.log(this.goods)
			if(this.goods.length<this.pageindex*10) return this.flag = true
			// console.log('触底了')
			this.pageindex++
			this.getGoodsList()
		},
		onPullDownRefresh() {
			console.log('下拉刷新了')
			this.pageindex = 1
			this.goods = []
			this.flag = false
			setTimeout(()=>{
				this.getGoodsList(()=>{
					uni.stopPullDownRefresh()
				})
			},1000)
		}
	}
</script>

<style lang="scss">
  .goods_list {
	  background: #eee;
  }
  .isOver {
	  width: 100%;
	  height: 50px;
	  line-height: 50px;
	  text-align: center;
	  // background: #fff;
	  font-size: 28rpx;
  }
</style>
