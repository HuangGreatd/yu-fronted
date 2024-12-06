<template>
	<div>
		<button class="button" @click="onWxLogin">微信授权登录</button>
	</div>
</template>

<script setup lang="ts">
	const onWxLogin = () => {
	  uni.login({
	    provider: 'weixin',
	    success: async (res) => {
	      const data = await uni.request({
	        method: "POST",
	        url: "http://localhost:8080/api/user/auth/wechat",
	        data: {
	          code: res.code
	        }
	      })
	      console.log(data);
	
	    },
	  })
	}
	
</script>



<style scoped>
	.button {
		color: white;
		padding: 6rpx;
		background-color: #ED4556;
		border-radius: none;
	}
</style>