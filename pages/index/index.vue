<template>
	<view class="page">
		<!-- <HelloComp myval="Hello Next"></HelloComp> -->
		<!-- <trailerStars innerScroe = "9.1" showNum="1"></trailerStars> -->
		<!-- 轮播图star -->
		<swiper :indicator-dots="true" :autoplay="true" class="carousel">
			<!-- 	<swiper-item >
				<image src="../../static/carousel/batmanvssuperman.png" class="carousel"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/carousel/spiderman.png" class="carousel"></image>
			</swiper-item> -->

			<swiper-item v-for="carousel in carouseList" :key="carousel.sort">
				<navigator 
				:url="'../movie/movie?trailerId='+carousel.movieId"
				open-type="navigate"
				>
					<image :src="carousel.image" class="carousel"></image>
				</navigator>
			</swiper-item>
		</swiper>
		<!-- 轮播图end -->

		<!-- 热门超英 star -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wawpper">
				<image src="../../static/icos/hot.png" class="hot-ico"></image>
				<view class="hot-tittle">
					热门超英
				</view>
			</view>
		</view>
		<scroll-view scroll-x="true" class="page-block hot">
			<view class="single-poster" v-for="hotSuperHeor in hotSuperHeorList">
				<view class="poster-wapper">
						<navigator 
					:url="'../movie/movie?trailerId='+hotSuperHeor.id"
					open-type="navigate"
					>
					<image :src="hotSuperHeor.cover" class="poster"></image>
					</navigator>
					<view class="movie-name">
						{{hotSuperHeor.name}}
					</view>
					<!-- 	<view class="movie-score-wapper">
					<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
					<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
					<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
					<image src="../../static/icos/star-yellow.png" class="star-ico"></image>
					<image src="../../static/icos/star-gray.png" class="star-ico"></image>
					<view class="movie-score">
						9.1
					</view>
				</view> -->
					<trailerStars :innerScroe="hotSuperHeor.score" showNum="1"></trailerStars>
				</view>
			</view>
		</scroll-view>
		<!-- 热门超英 end -->

		<!-- 热门预告 star -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wawpper">
				<image src="../../static/icos/interest.png" class="hot-ico"></image>
				<view class="hot-tittle">
					热门预告
				</view>
			</view>
		</view>
		<view class="hot-movises page-block">
			<video v-for="trailer in TrailerList" :src="trailer.trailer" :poster="trailer.poster" class="hot-movie-single"
			 controls>
			</video>
		</view>
		<!-- 热门预告 end -->

		<!-- 猜你喜欢 star -->
		<!-- <view class="page-block super-hot">
			<view class="hot-tittle-wawpper">
				<image src="../../static/icos/guess-u-like.png" class="hot-ico"></image>
				<view class="hot-tittle">
					猜你喜欢
				</view>
			</view>
		</view>
		
		<view class="page-block guess-u-like">
			<view class="page-block single-like-movie">
				<image src="http://122.152.205.72:88/superhero/MARVEL/Avengers3/poster.png" class="link-movie" ></image>
					<view class="movie-desc">
						<view class="movie-tettle">
							蝙蝠侠大战超人蝙蝠侠大战超人蝙蝠侠大战超人蝙蝠侠大战超人
							
						</view>
							<trailerStars :innerScroe = "9.1" showNum="0"></trailerStars>
								<view class="movie-info">
								2018 / 美国 / 科技  动作
							</view>
								<view class="movie-info">
								安其拉 / 亨利 / 小李
							</view>
						
					</view>
					<view class="movie-oper" @click="praiseMe">
						<image src="../../static/icos/praise.png" class="praise-ico" ></image>
						<view class="praise-me">
							赞一下
						</view>
						<view :animation="animationData"  class="praise-me animation-opacity">
							+1
						</view>
					</view>
				
			</view>
			
		</view>
		 -->
		<view class="page-block super-hot">
			<view class="hot-tittle-wawpper">
				<image src="../../static/icos/guess-u-like.png" class="hot-ico"></image>
				<view class="hot-tittle">
					猜你喜欢
				</view>
			</view>
		</view>
		<view class="page-block guess-u-like">
			<view class="page-block single-like-movie" v-for="(guess,gIndex) in guessULikeList">
					<navigator 
				:url="'../movie/movie?trailerId='+guess.id"
				open-type="navigate"
				>
				<image :src="guess.cover" class="link-movie"></image>
				</navigator>
				<view class="movie-desc">
					<view class="movie-tettle">
						{{guess.name}}
					</view>
					<trailerStars :innerScroe="9.1" showNum="0"></trailerStars>
					<view class="movie-info">
						{{guess.basicInfo}}
					</view>
					<view class="movie-info">
						{{guess.releaseDate}}
					</view>

				</view>
				<view class="movie-oper" :data-gIndex="gIndex" @click="praiseMe">
					<image src="../../static/icos/praise.png" class="praise-ico"></image>
					<view class="praise-me">
						赞一下
					</view>
					<view :animation="animationDataArr[gIndex]" class="praise-me animation-opacity">
						+1
					</view>
				</view>

			</view>

		</view>

		<!-- 猜你喜欢 end -->
	</view>
</template>

<script>
	// 导入自定义组件
	import HelloComp from "../../components/HelloComp.vue"
	import trailerStars from "../../components/trailerStars.vue"
	// import common from "../../common/common.js"
	export default {
		data() {
			return {
				carouseList: [],
				hotSuperHeorList: [],
				TrailerList: [],
				animationData: {},
				animationDataArr: [{}, {}, {}, {}, {}],
				guessULikeList: []
			}
		},
		onUnload() {
			this.animationData = {}; //页面卸载的时候清除动画数据
			this.animationDataArr = [{}, {}, {}, {}, {}]; //页面卸载的时候清除动画数据
		},
		onPullDownRefresh() {
			this.refresh();
		},
		onLoad() {
			var that = this;
			// #ifdef APP-PLUS || MP-WEIXIN
			this.animation = uni.createAnimation()
			// #endif
			// 在页面创建的时候创建一个临时动画对象
			// 请求轮播图
			// var me = this;
			// 获取common.js中的服务器地址
			// debugger;
			// var serverUrl =common.serverUrl;

			// 通过挂载到main.js中获取服务器地址，作为全局变量
			var serverUrl = that.serverUrl;

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
					// console.log(res.data);
					if (res.data.status == 200) {
						that.carouseList = res.data.data;
					}
				}
			});
			// 查询超英热门
			uni.request({
				url: serverUrl + '/index/movie/hot?type=superhero',
				method: "POST",
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				data: {
					qq: '',

				},
				success: (res) => {
					// debugger
					console.log(res.data);
					if (res.data.status == 200) {
						that.hotSuperHeorList = res.data.data;

					}

				}
			});

			// 查询预告片
			uni.request({
				url: serverUrl + '/index/movie/hot?type=trailer',
				method: "POST",
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				data: {
					qq: '',

				},
				success: (res) => {
					// debugger
					console.log(res.data);
					if (res.data.status == 200) {
						that.TrailerList = res.data.data;

					}

				}
			});
			that.refresh();
		},
		methods: {
			refresh() {
				var that = this;
				
				uni.showLoading({
					mask:true
				});
				uni.showNavigationBarLoading();
				
				
				var serverUrl = that.serverUrl;
				// 查询猜你喜欢
				uni.request({
					url: serverUrl + '/index/guessULike',
					method: "POST",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					data: {
						qq: '',

					},
					success: (res) => {
						// debugger
						console.log(res.data);
						if (res.data.status == 200) {
							that.guessULikeList = res.data.data;
						}
					},
					// 请就到数据  隐藏等待图标
					complete: () => {
						uni.hideNavigationBarLoading()
						uni.hideLoading()
						uni.stopPullDownRefresh();
					}
				});
			},

			// 实现点赞动画效果
			praiseMe(e) {
				// #ifdef APP-PLUS || MP-WEIXIN
				var gIndex = e.currentTarget.dataset.gindex; //搞不懂
				console.log(gIndex)

				// 构建动画数据，并且通过step来表示这组动画的完成
				this.animation.translateY(-60).opacity(1).step({
					duration: 400
				});
				// 导出动画数据到view组件，实现组件的动画效果
				this.animationData = this.animation;
				this.animationDataArr[gIndex] = this.animationData.export();
				// 还原动画
				setTimeout(function() {
					this.animation.translateY(0).opacity(0).step({
						duration: 0
					});
					this.animationDataArr[gIndex] = this.animationData.export();
				}.bind(this), 500);
				//#endif
			}

		},
		// 注册自定义组件
		components: {
			HelloComp,
			trailerStars
		},

	}
</script>

<style>
	@import url("index.css");
</style>
