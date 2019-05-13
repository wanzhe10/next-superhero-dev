<template>
	<view class="page">
		<!-- 视频播放 Star -->
		<view class="player">
			<video :src="trailerInfo.trailer" :poster="trailerInfo.poster" class="movie" controls>
			</video>
		</view>
		<!-- 视频播放 end -->
		<!-- 影片基本信息 star -->
		<view class="movie-info">
			<image :src="trailerInfo.cover" class="cover"></image>
			<view class="movie-desc">
				<view class="tittle">{{trailerInfo.name}}</view>
				<view class="basc-info">{{trailerInfo.basicInfo}}</view>
				<view class="basc-info">{{trailerInfo.originalName}}</view>
				<view class="basc-info">{{trailerInfo.totalTime}}</view>
				<view class="basc-info">{{trailerInfo.releaseDate}}</view>
				<view class="scroe-block">
					<view class="big-scroe">
						<view class="scroe-words">综合评分：</view>
						<view class="movie-score">{{trailerInfo.score}}</view>
					</view>
					<view class="scroe-stars">

						<!-- 这个是判断自定义组件的bug 只有在有评分的时候显示 评分  -->
						<block v-if="trailerInfo.score >= 0">
							<trailerStars :innerScroe="trailerInfo.score" showNum="0"></trailerStars>
						</block>
						<view class="prise-counts">
							{{trailerInfo.prisedCounts}} 人点赞
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 分割线组件 -->
		<lineWapper></lineWapper>

		<!-- 剧情介绍  star -->
		<view class="plots-block">
			<view class="plots-tittle">剧情介绍</view>
			<view class="plots-desc">{{trailerInfo.plotDesc}}</view>
		</view>
		<!--  剧情介绍  end -->
		
		<!-- 演职人员star -->
			<view class="scroll-block">
					<view class="plots-tittle">演职人员</view>
					<scroll-view  scroll-X  class="scroll-list">
						<view class="actor-wapper" 	v-for="directer in directerArray">
							<image 
							 :src="directer.photo"
							class="single-actor" 
							mode="aspectFill"
							>
							</image>
							<view class="actor-nane">{{directer.name}}</view>
							<view class="actor-role">{{directer.actName}}</view>
						</view>
						<view class="actor-wapper" 	v-for="actor in actorArray">
					<image 
					 :src="actor.photo"
					class="single-actor" 
					mode="aspectFill"
					>
					</image>
					<view class="actor-nane">{{actor.name}}</view>
					<view class="actor-role">{{actor.actName}}</view>
					</view>
					
					</scroll-view>
			</view>
			<!-- 演职人员end -->
		
	<!-- 剧照star -->
		<view class="scroll-block">
				<view class="plots-tittle">剧照</view>
				<scroll-view  scroll-X  class="scroll-list">
				<image 
				v-for="(img,imgIndex) in plotPicesArray"
				 :src="img"
				class="plot-image" 
				mode="aspectFill"
				@click="lookMe"
				:data-imgIndex = "imgIndex"
				>
				</image>
				</scroll-view>
		</view>
		<!-- 剧照end -->
	</view>
</template>

<script>
	import trailerStars from "../../components/trailerStars.vue"
	import lineWapper from "../../components/line.vue"
	export default {
		data() {
			return {
				trailerInfo: {},
				plotPicesArray: [], //剧照
				directerArray: [], //导演列表
				actorArray: [] //演员列表
			}
		},
		onLoad(params) {
			// 获取上一个页面传入的参数 
			var trailerId = params.trailerId;
			// 获取预告片的详细信息
			var that = this;
				// 通过api设置导航栏的属性
			uni.setNavigationBarColor({
				frontColor:"#ffffff",
				backgroundColor:"#000000"
			}),
			uni.showLoading({
				mask: true,
				title: '请稍后'
			});
			uni.showNavigationBarLoading();
			var serverUrl = that.serverUrl;
			// 查询猜你喜欢
			uni.request({
				url: serverUrl + '/search/trailer/' + trailerId, //直接在url侯曼拼接也可以
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
						var trailerInfo = res.data.data;
						that.trailerInfo = trailerInfo;
						// console.log(JSON.parse(trailerInfo.plotPics))
						// 把剧照的字符串转化为json array
						that.plotPicesArray = JSON.parse(trailerInfo.plotPics);
					}
				},
				// 请就到数据  隐藏等待图标
				complete: () => {
					uni.hideNavigationBarLoading()
					uni.hideLoading()
				}
			});
			// 获取导演
			uni.request({
				url: serverUrl + '/search/staff/' + trailerId + '/1', //直接在url侯曼拼接也可以
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
						that.directerArray = res.data.data;
						// debugger
					}
				},
			});
			// 获取演员
			uni.request({
				url: serverUrl + '/search/staff/' + trailerId + '/2', //直接在url侯曼拼接也可以
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
						that.actorArray = res.data.data;
						// debugger
					}
				},
			});
			
		
		},
		methods: {
		lookMe(e){
			var that = this;
			var imgIndex = e.currentTarget.dataset.imgindex;
			uni.previewImage({
				urls:that.plotPicesArray,
				current:that.plotPicesArray[imgIndex]
				
			})
		},
		},
		// 注册自定义组件
		components: {
			trailerStars,
			lineWapper
		},
	}
</script>

<style>
	@import url("movie.css");
</style>
