<template>
	<div class="body">
		<div v-if="pageLoad">
			 
			<div class="main-body">
				<div class="uhead">

					<image @click="gourl('../user/user_head')" class="uhead-img" :src="pageData.data.user_head+'.100x100.jpg'"></image>

					<div class="uhead-box">
						<div class="uhead-nick">{{pageData.data.nickname}}</div>
						 
						<div class="uhead-rnum flex-ai-center flex">

							金币
							<text class="cl-money mgl-5 mgr-5">{{pageData.data.gold}}</text>

							积分
							<text class="cl-money mgl-5">{{pageData.data.grade}}</text>

						</div>

					</div>
					<navigator url="../user/set" class="flex-center btn-small btn-link iconfont icon-settings"></navigator>
				</div>
				 
				 
				<div v-for="(item,index) in pageData.navList" :key="index">
					<div class="m-navPic mgb-5">
						 
						<navigator v-for="(cc,ccindex) in item.child" :url="cc.link_url" :key="ccindex" class="m-navPic-item">
							<div class="m-navPic-icon" v-bind:class="cc.icon"></div>
							<div class="m-navPic-title">{{cc.title}}</div>
						</navigator>
						 
					</div>
				</div>	
				 


			</div>
			 
		</div>
		<div v-else class="bg-fff pd-10">
				<div class="flex flex-center mgb-10 cl2">您还未登录,请先登录</div>
				<div class="flex flex-center">
					<navigator  class="btn-small" url="../login/index">前往登录</navigator>
				</div>
			
		</div>
		<mt-footer tab="user"></mt-footer>
	</div>
</template>

<script>
	 
	import mtFooter from "../../components/footer.vue";
	export default {
		components: {
			mtFooter
		},
		data: function(){
			return {
				pageLoad: false,
				pageHide: false,
				pageData: {}
			}
		},
		onLoad: function (option) {
			uni.setNavigationBarTitle({
				title: "个人中心",
			})
			this.getPage();
		},
		onShow: function () {
			if (this.pageHide) {
				this.pageHide = false;
				this.getPage();
			}
		},
		onHide: function () {
			this.pageHide = true;
		},
		methods: {
			gourl: function (url) {
				uni.navigateTo({
					url: url,
				})
			},
			getPage: function () {
				var that = this;
				uni.request({
					url: that.app.apiHost + "?m=user&ajax=1",
					data: {
						authcode: that.app.getAuthCode(),
						fromapp:that.app.fromapp()
					},
					success: function (res) {
						if (res.data.error == 1000) {
							/*
							uni.navigateTo({
								url: "../login/index",
							})
							*/
						} else {
							that.pageLoad = true;
							that.pageData = res.data.data;
						}


					}
				})
			}
		},
	}
</script>

<style>
	.body {
		background-color: #e3e3e3;
	}

	.uhead {
		display: flex;
		flex-direction: row;
		padding: 22upx;
		background-color: #fff;
		margin-bottom: 11upx;
	}

	.uhead-img {
		width: 172upx;
		height: 172upx;
		margin-right: 22upx;
		display: block;
		border-radius: 50%;
	}

	.uhead-box {
		flex: 1;
	}

	.uhead-nick {
		margin-bottom: 10upx;
		font-size: 34upx;
	}

	.uhead-rnum {
		color: #999;
		margin-bottom: 32upx;
		line-height: 29upx;
		display: flex;
		font-size: 29upx;
	}

	.order-box {
		background-color: #fff;
		padding: 22upx;
		margin-bottom: 22upx;
	}

	.order-box-hd {
		display: flex;
		flex-direction: row;
		line-height: 79upx;
		border-bottom: 2upx solid #ddd;
		margin-bottom: 22upx;
	}

	.order-box-status {
		display: flex;
		flex-direction: row;
	}

	.order-box-item {
		flex: 1;
		text-align: center;
		font-size: 32upx;
		color: #666;
	}

	.order-box-item .iconfont {
		display: block;
		color: #666;
		font-size: 42upx;
	}

	.row-item {
		display: flex;
		flex-direction: row;
		padding: 10upx 16upx;
		margin-bottom: 10upx;
		font-size: 35upx;
		color: #555;
		border-bottom: 2upx solid #eee;
	}

	.row-icon {
		position: relative;
		top: -5upx;
		font-size: 40upx;
		margin-right: 10upx;
		color: #555;
	}
</style>
