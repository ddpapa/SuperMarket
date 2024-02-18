<template>
	<view class="container">
		<view class="navigator-wrap">
			<view class="locate">
				<u-icon name="map-fill" color="#fff" size="20"></u-icon>
				<text class="locate-text">{{locationName}}</text>
				<u-icon name="arrow-right" color="#fff" size="12"></u-icon>
			</view>
			<u-icon name="chat-fill" color="#fff" size="20"></u-icon>
			<view class="search">
				<u-icon name="search" color="#c0c0c0" size="23"></u-icon>
				<view class="search-quick-tips">

				</view>
				<text class="search-separator">|</text>
				<text class="search-right-text">搜索</text>
			</view>
		</view>
		<view class="head" :style="{ backgroundImage: `url(${backgroundImageUrl})`}">
			<u-swiper :list="swiperList" indicator indicatorMode="line"></u-swiper>
		</view>
		<view class="text-area">
		</view>
	</view>
</template>

<script setup lang="ts">
	import { onLoad } from '@dcloudio/uni-app';
	import { reactive, ref, shallowRef } from 'vue';

	const locationName = shallowRef("定位失败");
	const imageUrls = [
		'https://cdn.uviewui.com/uview/swiper/swiper1.png',
		'https://cdn.uviewui.com/uview/swiper/swiper2.png',
		'https://cdn.uviewui.com/uview/swiper/swiper3.png',
	];
	const backgroundImageUrl = ref(imageUrls[0]);
	const swiperList = reactive(imageUrls);

	onLoad(() => {
		uni.getLocation({
			type: 'gcj02',
			geocode: true,
			success: res => {
				locationName.value = res.address.poiName;
			},
			fail: () => {
				locationName.value = '定位失败';
			}
		});
	})
</script>

<style lang="scss">
	$padding-space: 25rpx;

	.container {
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		background-color: gray;

		.navigator-wrap {
			width: calc(100vw - $padding-space * 2);
			height: 44px;
			padding: var(--status-bar-height) $padding-space 0 $padding-space;
			position: absolute;
			top: 0;
			left: 0;
			z-index: 9999;
			display: flex;
			align-items: center;
			justify-content: space-between;
		}

		.locate {
			display: flex;
			align-items: center;
		}

		.locate-text {
			margin-left: 10rpx;
			font-size: 25rpx;
			color: #fff;
		}

		.search {
			height: 35px;
			width: calc(100vw - $padding-space * 2 - 45rpx);
			padding: 0 30rpx 0 15rpx;
			position: absolute;
			top: calc(var(--status-bar-height) + 44px);
			display: flex;
			align-items: center;
			border-radius: 17px;
			background-color: #fff;
			color: #c0c0c0;
		}

		.search-quick-tips {
			flex: 1;
		}

		.search-separator {
			margin: 0 10rpx;
			font-size: 22rpx;
		}

		.search-right-text {
			font-size: 26rpx;
		}

		.head {
			width: 100vw;
			height: 350px;

			.u-swiper {
				margin-top: 120px;
			}
		}
	}
</style>