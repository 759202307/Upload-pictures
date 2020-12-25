<template>
    <el-form
        ref="form"
        :model="form"
        label-width="150px"
    >
        <el-form-item label="logo：">
            <el-upload
                class="upload-poster"
                action="#"
                :on-progress="logoPreview"
                :show-file-list="false"
                :before-upload="
                    beforeAvatarUpload.bind(this, 500, 60, 'reclogoFile')
                "
                :auto-upload="true"
            >
                <img
                    :src="imgURL"
                    class="avatar"
                />
                <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                <div slot="tip" class="el-upload__tip tip">
                    （建议尺寸xx*xx）
                </div>
            </el-upload>
        </el-form-item>
    </el-form>
</template>

<script>
export default {
    components: {},
    data() {
        return {
            imgURL: "xxxxx",
        }
    },
    mounted() {
    },
    methods: {
        beforeAvatarUpload(width, height, type, file) {
            let fileName = file.name;
            let regex = /(.jpg|.jpeg|.gif|.png)$/;//图片格式校验
            if (type == "reclogoFile") {
                if (!regex.test(fileName.toLowerCase())) {
                    return false;
                }
            }
            return new Promise(function(resolve, reject) {
                let _URL = window.URL || window.webkitURL;
                let img = new Image();
                img.onload = function() {
                    let valid = img.width >= width && img.height >= height;//调用方法传入宽高
                    valid ? reject() : resolve();
                };
                img.src = _URL.createObjectURL(file);
            }).then(
                () => {
                    return file;
                },
                () => {
                    this.$message.error("图片尺寸大小不符合要求，请重新上传!");
                    return Promise.reject();
                }
            );
        }
        logoPreview(event, file) {
            this.imgURL = URL.createObjectURL(file.raw);
        }
    },
};
</script>

<style lang="scss">