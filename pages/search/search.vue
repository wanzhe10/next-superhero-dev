<template>
	<view class="page">
		<view class="search-block">
			<view class="search-ico-wappper">
				<image src="/static/icos/search.png" mode="" class="search-ico"></image>
			</view>
			<input 
			type="text"
			 value=""  
			 placeholder="搜索预告" 
			 maxlength="10" 
			 class="search-text" 
			 confirm-type="search"
			 @confirm="searchMe"
			 />
			 <!-- @confirm="searchMe" -->

		</view>
		<view class="movie-list page-block">
			<view class="movie-wapper"	v-for="trailer in trailerList">
				<image 
				:src="trailer.cover" 
				class="poster"
				:data-trailerId="trailer.id"
				@click="showTrailer"
				>
				</image>
			</view>
		<!-- 	<view class="movie-wapper">
				<image src="http://122.152.205.72:88/superhero/MARVEL/AntMan2/cover.png" class="poster"></image>
			</view> -->
		</view>
	
	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				trailerList:[],
				keywords:"", //搜索关键字
				page:1, //当前第几页
				totalPages:1 //总页数
			}
		},
		onLoad() {
				var that = this;
				uni.showLoading({
					mask:true,
					title:'请稍后'
				});
				uni.showNavigationBarLoading();
				var serverUrl = that.serverUrl;
				// 查询猜你喜欢
				uni.request({
					url: serverUrl + '/search/list?qq=&keywords=&page=&pageSize=&', //直接在url侯曼拼接也可以
					method: "POST",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						// debugger
						console.log(res.data);
						if (res.data.status == 200) {
							that.trailerList = res.data.data.rows;
						}
					},
					// 请就到数据  隐藏等待图标
					complete: () => {
						uni.hideNavigationBarLoading()
						uni.hideLoading()
					}
				});
		},
		onReachBottom() {
				var that = this;
				 var page = that.page + 1; //查询下一页面当前页数累加1
				 var keywords = that.keywords; //获得当前页面中data里的搜索值
				 var totalPages = that.totalPages;//获得总页数
				 // 如果当前需要的分页的分页数和总页数相等， 就不分页
				 if(page > totalPages){
					 return;
				 }
				 that.pagedTralilerList(keywords,page,15);
		},
		methods: {
			// 分页
			pagedTralilerList(keywords,page,pageSize){
				var that = this;
				uni.showLoading({
					mask:true,
					title:'请稍后'
				});
				uni.showNavigationBarLoading();
				var serverUrl = that.serverUrl;
				// 查询猜你喜欢
				uni.request({
					url: serverUrl + '/search/list?qq=&keywords='+keywords
																	+'&page='+page
																	+'&pageSize='+pageSize, //直接在url侯曼拼接也可以
					method: "POST",
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						// debugger
						console.log(res.data);
						if (res.data.status == 200) {
							var tempList = res.data.data.rows;
							that.trailerList = that.trailerList.concat(tempList); //数组拼接 放到数组后面
							that.totalPages = res.data.data.total;
							that.page = page; 	//覆盖当前页面里的page
						}
					},
					// 请就到数据  隐藏等待图标
					complete: () => {
						uni.hideNavigationBarLoading()
						uni.hideLoading()
					}
				});
					
			},
			searchMe(e){
				var that = this;
				//获取搜索的内容
				var value = e.detail.value;
				console.log(value)
				that.keywords = value;
				that.trailerList = [];
				this.pagedTralilerList(value,1,15)
				
				
			},
			showTrailer(e){
				var trailerId =e.currentTarget.dataset.trailerid;
				// 页面跳转接口IPI
				uni.navigateTo({
					url:"../movie/movie?trailerId="+trailerId,
					
				})
			}
		}
	}
</script>

<style>
	@import url("search.css");
	
</style>
