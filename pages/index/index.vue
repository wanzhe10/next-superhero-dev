<template>
	<view class="page">
		<swiper :indicator-dots="true" :autoplay="true" class="carousel">
		<!-- 	<swiper-item >
				<image src="../../static/carousel/batmanvssuperman.png" class="carousel"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/carousel/spiderman.png" class="carousel"></image>
			</swiper-item> -->
			
				<swiper-item v-for="carousel in carouseList">
				<image 
				:src="carousel.image" 
				class="carousel"
				>
				</image>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	// import common from "../../common/common.js"
	export default {
		data() {
			return {
				carouseList: []
			}
		},
		onLoad() {
			// 请求轮播图
			// var me = this;
			// 获取common.js中的服务器地址
			// debugger;
			// var serverUrl =common.serverUrl;
			
			// 通过挂载到main.js中获取服务器地址，作为全局变量
			var serverUrl =this.serverUrl;
			uni.request({
				url: serverUrl + '/index/carousel/list',
				method: "POST",
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				data: {
					qq: ''
				},
				success: (res) => {
					// debugger
					console.log(res.data);
					if (res.data.status == 200) {
						this.carouseList = res.data.data;

					}

				}
			});
		},
		methods: {

		}
	}
</script>

<style>
	@import url("index.css");
</style>
