<template>
	<view>
		<view class="upimg-box bg-fff">
			<view class="none">
				<input type="text" maxlength="-1" name="imgsdata" :value="imgsData" />
			</view>
			<view class="upimg-item" v-for="(img,imgIndex) in imgsList" :key="imgIndex">
				<image class="upimg-img" :src="img.trueimgurl+'.100x100.jpg'"></image>
				<view class="upimg-del" @click="delImg(imgIndex)"></view>
			</view>
			
			<view @click="upImg()" class="upimg-btn">
				<i class="upimg-btn-icon"></i>
			</view>
		</view>
	</view>
</template>

<script>
	var app = require("../common/common.js"); 
	export default {
		name:"upimg-box",
		props:{
			defaultImgsList:{}, 
			defaultImgsData:{},
		},
		data:function(){
			return {
				imgsData:this.defaultImgsData,
				imgsList:this.defaultImgsList,
			}
		},
		methods:{
			upImg:function(){
				var that=this;
				
				uni.chooseImage({
					success: function (chooseImageRes) {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						const len=tempFilePaths.length;
						for(var i=0;i<len;i++){
							uni.uploadFile({
								url: app.apiHost+'?fromapp=wxapp&m=upload&a=img&ajax=1&authcode='+app.getAuthCode(), //仅为示例，非真实的接口地址
								filePath: tempFilePaths[i],
								name: 'upimg',
								dataType:"json",
								success: function (res) {
									
									if(!res.data.error){
										var rs=JSON.parse(res.data);
										
										var json=[{
											imgurl:rs.data.imgurl,
											trueimgurl:rs.data.trueimgurl
										}];
										that.imgsList=app.json_add(that.imgsList,json);
										if(that.imgsData!=""){
											that.imgsData=that.imgsData+","+rs.data.imgurl;
										}else{
											that.imgsData=rs.data.imgurl;
										}
										
										 
									}
								}
							});
						}
					}
				});
			},
			delImg:function(index){
				var len=this.imgsList.length;
				var imgslist=[];
				var imgsData="";
				var mgs=this.imgsList;
				for(var i in mgs){
					if(i!=index){
						imgslist.push(mgs[i]);						
					}
				}
				for(var i=0;i<imgslist.length;i++){
					if(i>0){
						imgsData+=",";
					}
					imgsData+=imgslist[i].imgurl;
				}
				console.log(imgsData);
				this.imgsData=imgsData;
				this.imgsList=imgslist;
				
			}
		}
	}
</script>

<style>

</style>
