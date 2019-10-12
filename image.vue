<template>
	<div class="ng-image" :style="height">
		<div :class="modeClass" :style="{'background-image': `url(${this.url})`}"></div>
		<img :src="url" alt="" ref="img" @load="imgLoad">
	</div>
</template>
<script>
	export default {
		name: 'NgImage',
		props: {
			mode: {
				type: String,
				default() {
					return 'scaleToFill'
				}
			},
			src: {
				type: String,
				default() {
					return ''
				}
			}
		},
		data() {
			return {
				height: {}
			}
		},
		methods: {
			imgLoad() {
				if(this.mode === 'widthFix') {
					let {naturalWidth, naturalHeight, width} = this.$refs['img'],
						height = naturalHeight * width / naturalWidth
					this.height = {height: height + 'px'}
				}
			}
		},
		computed: {
			url() {
				let path = this.src
				if(/\\/.test(path)) path.replace(/\\/g, '/')
				return path
			},
			modeClass() {
				let mode = 'scale-to-fill'
				switch(this.mode) {
					case 'aspectFit':
					mode = 'aspect-fit'
					break
					case 'aspectFill':
					mode = 'aspect-fill'
					break
					case 'widthFix':
					mode = 'width-fix'
					break				
				}
				return mode
			}
		}
	}
</script>
<style scoped>
	.ng-image {
		width: 320px;
		height: 240px;
		display: inline-block;
		overflow: hidden;
		position: relative;
	}
	.ng-image div,
	.ng-image img {
			width: 100%;
			height: 100%;
		}
	.ng-image img {
		display: block;
		position: absolute;
		top: 0;
		left: 0;
		opacity: 0;
	}
	.ng-image div {
		background-repeat: no-repeat;
	}
	.ng-image .scale-to-fill {
		background-position: 0 0;
		background-size: 100% 100%;
	}
	.ng-image .aspect-fill {
		background-position: center center;
		background-size: cover;
	}
	.ng-image .aspect-fit {
		background-position: center center;
		background-size: contain;
	}
	.ng-image .width-fix {
		background-size: 100% 100%;
	}
</style>
