<template>
	<view>
		<RichTextEditor
			selector="#1"
			v-model="WatchValue"
			api-key="a2b319u3slprunwyio2islj42yuiz0iuuxdyoqhzjl6caoxq"
			:init="{
				
				height: 500,
				plugins: [
				'advlist autolink lists link image charmap print preview anchor',
				'searchreplace visualblocks code fullscreen',
				'insertdatetime media table paste code wordcount',
				'autosave  codesample directionality emoticons fullpage hr nonbreaking pagebreak save tabfocus template toc visualchars'
				],
				toolbar:
					['undo redo | formatselect | bold italic underline forecolor backcolor hr | \
					alignleft aligncenter alignright alignjustify | \
					bullist numlist outdent indent | lists image media | removeformat fullscreen',
					'restoredraft charmap codesample | ltr rtl | emoticons fullpage insertdatetime |\
					link nonbreaking pagebreak paste | preview print | save searchreplace template | toc visualblocks visualchars | wordcount',
					'table tabledelete | tableprops tablerowprops tablecellprops | tableinsertrowbefore tableinsertrowafter tabledeleterow | tableinsertcolbefore tableinsertcolafter tabledeletecol'],
				language: 'zh_CN',
				skin: 'CUSTOM1',
				images_upload_url: 'https://www.txtz.club:8808/api/upload',
				file_picker_callback: function(callback, value, meta) {},
				branding: false,
				draggable_modal: true
			}"
		/>
	</view>
</template>

<script>
	import Editor from '../tinymce/tinymce-vue'
	export default {
		methods:{
		 release(){
			            //content 是文本内容带标签
			            let content = this.WatchValue.getContent();
			            // getContent( { 'format' : 'text'} );//这是获取里面的文本文件，不带标签
			            return content;
			}
		},
		props:{
			values:{
				type:String,
				default:""
			}
		},
		components: {
		    'RichTextEditor': Editor
		},
		data() {
			return {
				WatchValue:this.values
			};
		},
		watch:{
			WatchValue(value){
				
				
				this.$emit('text',this.WatchValue);
				this.$emit('change', window.tinymce.activeEditor.selection.getContent());
				
			},
			values(value){
				
				this.$emit('text',value);
			}
		}
	}
</script>

<style>

</style>
