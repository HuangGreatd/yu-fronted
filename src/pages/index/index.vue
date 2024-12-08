<template>
	<div>
		<!-- 点击获取--头像 -->
		<view class="avatar">
			<button open-type="chooseAvatar" class="avatarButton" @chooseavatar="getavatar">
				<image class="picture" :src="headerAvatar ? headerAvatar : ' ../../../static/login/logo.png'" mode="">
				</image>
			</button>
		</view>

		<!-- 点击获取--昵称 -->
		<view class="nick_name">
			<text class="infoName">用户昵称:&nbsp;&nbsp;&nbsp;&nbsp;</text>
			<input type="nickname" class="infoNickName" v-model="getName" placeholder="请输入名字" @change="getname" />

		</view>

		<view class="divider"></view>

		<button class="button" @click="onWxLogin">微信授权登录</button>

	</div>
</template>

<script setup lang="ts">
	import { ref } from 'vue';

	const headerAvatar = ref('');
	const getName = ref('');

	const onWxLogin = () => {
		uni.login({
			provider: 'weixin',
			success: async (res) => {
				const avatar = uni.getStorageSync('avatar');
				const nickname = uni.getStorageSync('nickname');

				const data = await uni.request({
					method: "POST",
					url: "http://localhost:8080/api/user/auth/wechat",
					data: {
						code: res.code,
						avatar,
						nickname
					}
				})
				console.log(data);

			},
		})
	}

	const getavatar = (e : any) => {
		console.log('-e---', e);
		headerAvatar.value = e.detail.avatarUrl;
		uni.setStorageSync('avatar', e.detail.avatarUrl);
	};

	const getname = (e : any) => {
		console.log('获取昵称', e);
		getName.value = e.detail.value;
		uni.setStorageSync('nickname', e.detail.value);
	};
</script>



<style scoped>
	.button {
		color: white;
		padding: 6rpx;
		background-color: #ED4556;
		border-radius: none;
		margin-top: 50px;
	}

	.divider {
		border-bottom: 1px solid #ccc;
		/* 设置下边框为1像素的灰色实线，模拟分割线效果，可以根据喜好调整颜色和粗细等 */
		margin: 10px 0;
		/* 设置上下外边距为10像素，让分割线与上下元素有一定间隔，可按需调整 */
		margin-top: 20px;
	}
</style>