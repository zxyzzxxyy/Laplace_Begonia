<template>
	<view style="display: flex;flex-direction: column;align-items: center;justify-content: center;height:100vh;">
		<text style="font-size: 50px;font-weight: 800;
		background-image: -webkit-linear-gradient(92deg, #06f351, #ffaa7f);
		    -webkit-background-clip: text;
		    -webkit-text-fill-color: transparent;
  " class="hue">白师加油，战胜疫情</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
  .hue{
	  position:relative;
  	animation: hue 5s infinite linear;
	 animation-direction: reverse;
  }
  @keyframes hue {
		0%{
            -webkit-filter: hue-rotate(0deg);
			font-size: 50px;
        }
        50%{
            -webkit-filter: hue-rotate(-360deg);
			font-size: 100px;
        }
		100%{
            -webkit-filter: hue-rotate(0deg);
			font-size: 50px;
        }
  }
</style>
