<template>
	<div class="container">
		<div class="upload-button-container">
			<el-upload :action="uploadAction" :on-success="handleUploadSuccess" :on-error="handleUploadError"
				:on-remove="handleRemove" :file-list="fileList" :limit="1" :multiple="false"
				:before-upload="beforeUpload" accept="video/*">
				<template #trigger>
					<el-button size="large" type="primary" class="custom-upload-button">选择视频</el-button>
				</template>
				<template #default>
					<div class="file-list-container">
						<div class="el-upload-list">
							<div v-for="(file, index) in fileList" :key="index" class="el-upload-list__item is-success">
								<div class="el-upload-list__item-thumbnail">
									<video :src="previewUrl" controls width="120" height="80"></video>
								</div>
							</div>
						</div>
					</div>
				</template>
			</el-upload>
			<el-button size="large" type="primary" class="custom-upload-button" style="width: 100px;
  transform: translateX(625%);" @click="processVideo">行为识别</el-button>
		</div>
		<div class="video-name-container" v-if="fileList.length > 0">
			<p>视频名称：{{ fileList[0].name }}</p>
		</div>
		<div class="preview-container">
			<el-alert v-if="error" :title="error" type="error" show-icon description=""></el-alert>
			<video v-if="previewUrl" :src="previewUrl" controls width="640" height="360"></video>
		</div>
	</div>
</template>


<script>
	import {
		ElMessage
	} from "element-plus";
	export default {
		data() {
			return {
				fileList: [],
				previewUrl: "",
				error: "",
				uploadAction: "http://localhost:8000/api/upload_video/",
				processedVideoUrl: "",
				processedVideoProbabilities: {},
			};
		},
		methods: {
			beforeUpload(file) {
				this.error = "";
				const isVideo = file.type.startsWith("video/");
				if (!isVideo) {
					this.error = "请上传一个视频文件";
				}
				return isVideo;
			},
			handleUploadSuccess(response, file) {
				this.previewUrl = URL.createObjectURL(file.raw);
				this.processedVideoUrl = response.output_file_url;
				this.processedVideoProbabilities = response.top_results;
				ElMessage({
					message: "视频上传成功",
					type: "success",
				});
			},
			handleUploadError(err) {
				ElMessage({
					message: `视频上传失败: ${err.message}`,
					type: "error",
				});
			},

			handleRemove() {
				this.previewUrl = "";
			},
			async processVideo() {
				if (!this.previewUrl) {
					ElMessage({
						message: "请先上传一个视频",
						type: "warning",
					});
					return;
				}

				try {
					this.previewUrl = this.processedVideoUrl;
					const probabilities = this.processedVideoProbabilities;
					let message = "视频处理成功<br/>\n";
					if (probabilities) {
						message += probabilities.map(([category, probability]) => {
							const percentage = (probability * 100).toFixed(2);
							return `${category}: ${percentage}%<br/>`;
						}).join('\n');
					}

					ElMessage({
						dangerouslyUseHTMLString: true,
						message: message,
						type: "success",
					});
				} catch (err) {
					ElMessage({
						message: `视频处理失败: ${err.message}`,
						type: "error",
					});
				}
			},

		},
	};
</script>

<style scoped>
	.custom-upload-button {
		/* 你的自定义样式 */
		border-radius: 10px;
		/* 修改按钮圆角大小 */
		font-size: 90%;
		font-family: 'Times New Roman', Times, serif;
		/* color: #ffff64; */
		border-width: 2px;
		font-weight: bolder;
		margin-right: 16px;
		/* 添加右边距以调整按钮之间的间隔 */
	}

	.container {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		min-height: 100vh;
	}

	.upload-button-container {
		position: fixed;
		bottom: 0;
		width: 100%;
		display: flex;
		flex-direction: column-reverse;
		justify-content: center;
		gap: 10px;
		/* 		padding: 16px; */
		/* 		transform: translateX(-20px); */
		/* 使按钮组向右偏移 */
	}

	.video-name-container {
		margin:
			16px auto 0 auto;
		/* 调整视频名称容器的边距 */
		text-align: center;
		/* 文本居中显示 */
	}

	.preview-container {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.el-message--error {
		white-space: pre-line
	}
</style>