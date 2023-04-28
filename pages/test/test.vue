<template>
	<view style="width:100vw;height:100vh">
	<vue-particles
	      color="#dedede"
	      :particleOpacity="0.7"
	      :particlesNumber="40"
	      shapeType="star"
	      :particleSize="4"
	      linesColor="#FFFFFF"
	      :linesWidth="2"
	      :lineLinked="true"
	      :lineOpacity="0.4"
	      :linesDistance="150"
	      :moveSpeed="3"
	      :hoverEffect="true"
	      hoverMode="grab"
	      :clickEffect="true"
	      clickMode="push"
	      class="cash"
	    >
	    </vue-particles>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				
			}
		},
		
		methods: {
			dark() {
			
			}
		}
	}
</script>

<style>

</style>
