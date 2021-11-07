<template>
  <div>
    <h1>สั่งซื้อและส่งหลักฐานการโอนเงิน</h1>
    <form v-on:submit.prevent="editBlog">
      <p>ชื่อ-นามสกุล : <input type="text" v-model="blog.title" /></p>
      <transition name="fade">
      <div class="thumbnail-pic" v-if="blog.thumbnail != 'null'">
        <img :src="BASE_URL+blog.thumbnail" alt="thumbnail">
      </div>
      </transition>
      <h4>ช่องทางการชำระเงิน</h4>
                      พร้อมเพย์ : 0830521809
                    <br>
                      ธนาคารกสิกร : 0568467167
                    <br>
                      ธนาคารกรุงไทย : 8570660766
                    <br>
                    <br>
                    <h4>หากท่านได้ชำระสินค้าแล้ว รบกวนส่งแนบหลักฐานการชำระด้วย</h4>
      
      <form enctype="multipart/form-data" novalidate>
        <div class="dropbox">
        <input
          type="file"
          multiple
          :name="uploadFieldName"
          :disabled="isSaving"
          @change="
            filesChange($event.target.name, $event.target.files);
            fileCount = $event.target.files.length;
          "
          accept="image/*"
          class="input-file"
        />
        <!-- <p v-if="isInitial || isSuccess"/> -->
        <p v-if="isInitial">คลิกเพื่อเลือกรูปภาพ <br> ส่งหลักฐานการโอนเงิน</p>
        <p v-if="isSaving">Uploading {{ fileCount }} files...</p>
        <p v-if="isSuccess">Upload Successful.</p>
        <p v-if= "isFailed">Upload Failed</p>
        </div>
        <div>
            <ul class="pictures">
                <li v-for="picture in pictures" v-bind:key="picture.id">
                    <img style="margin-bottom:5px;"
                    :src="BASE_URL+picture.name"
                    alt="picture image">
                    <br />
                    <button v-on:click.prevent="delFile(picture)">Delete</button>
                    <button v-on:click.prevent="useThumbnail(picture.name)">Thumbnail</button>
                </li>
            </ul>
        </div>
      </form>
      <p><strong>ที่อยู่และเบอร์โทรติดต่อ : </strong></p>
      <p>
        <vue-ckeditor 
          v-model.lazy="blog.content" 
          :config="config" 
          @blur="onBlur($event)" 
          @focus="onFocus($event)" 
        />
      </p>
      <p>สีและขนาด : <input type="text" v-model="blog.category" /></p>
      <p>จำนวน : <input type="text" v-model="blog.status" /></p>
      <p><b-button-group>
          <b-button variant="success" type="submit">Update Order</b-button>
          <b-button variant="dark" v-on:click="navigateTo('/blogs')">กลับ</b-button>
      </b-button-group></p>
    </form>
  </div>
</template>
<script>
import BlogsService from "@/services/BlogsService";
import VueCkeditor from 'vue-ckeditor2'
import UploadService from "@/services/UploadService";

const STATUS_INITIAL = 0,
  STATUS_SAVING = 1,
  STATUS_SUCCESS = 2,
  STATUS_FAILED = 3;

export default {
  data () {
    return {
      blog: {
        title: "",
        thumbnail: "null",
        pictures: "null", 
        category: "",
        status: "",
      },
      BASE_URL: "http://localhost:8081/assets/uploads/",
      error: null,
      //uploadedFiles: [],
      uploadError: null,
      currentStatus: null,
      uploadFieldName: "userPhoto",
      uploadedFileNames: [],
      pictures: [],
      pictureIndex: 0,
      config: {
                //toolbar: [
                //["Bold", "Italic", "Underline", "Strike", "Subscript", "Superscript"]
                //],
                //height: 300
            },
    };
  },
  methods: {
    async editBlog() {
      try {
        this.blog.pictures = JSON.stringify(this.pictures)
        await BlogsService.put(this.blog);
        this.$router.push({
          name: "blogs",
        });
      } catch (err) {
        console.log(err);
      }
    },
    navigateTo(route) {
      console.log(route);
      this.$router.push(route);
    },
    wait(ms) {
      return (x) => {
        return new Promise((resolve) => setTimeout(() => resolve(x), ms));
      };
    },
    reset() {
      //reset form to initial state
      this.currentStatus = STATUS_INITIAL;
      //this.uploadedFiles = []
      this.uploadError = null;
      this.uploadedFileNames = [];
    },
    async save(formData) {
      //upload data to the server
      try {
        this.currentStatus = STATUS_SAVING;
        await UploadService.upload(formData);
        this.currentStatus = STATUS_SUCCESS;

        let pictureJSON = []
        this.uploadedFileNames.forEach(uploadFileName => {
            let found = false;
            for (let i=0; i<this.pictures.length; i++) {
                if(this.pictures[i].name == uploadFileName){
                    found = true;
                    break;
                }
                
            }
            if(!found){
                this.pictureIndex++
                let pictureJSON = {
                    id: this.pictureIndex,
                    name: uploadFileName
                }
                this.pictures.push(pictureJSON)
            }
        });

        this.clearUploadResult();
      } catch (error) {
        console.log(error);
        this.currentStatus = STATUS_FAILED;
      }
    },
    async delFile (material){
      let result = confirm("Want to delete?")
      if (result) {
        let dataJSON = {
          "filename":material.name
        }

        await UploadService.delete(dataJSON)

        for(var i=0;i<this.pictures.length;i++){
          if(this.pictures[i].id === material.id){
            this.pictures.splice(i, 1)
            this.materialIndex--
            break
          }
        }
      }
    },
    useThumbnail (filename){
      console.log(filename)
      this.blog.thumbnail = filename
    },
    filesChange(fieldName, fileList) {
      //handle file changes
      const formData = new FormData();

      if (!fileList.length) return;

      //append the files to FormData
      Array.from(Array(fileList.length).keys()).map((x) => {
        formData.append(fieldName, fileList[x], fileList[x].name);
        this.uploadedFileNames.push(fileList[x].name);
      });

      //save here
      this.save(formData);
    },
    clearUploadResult: function () {
      setTimeout(() => this.reset(), 5000);
    },
  async created() {
    try {
      let blogId = this.$route.params.blogId;
      this.blog = (await BlogsService.show(blogId)).data;
      this.config.toolbar = [
                {
                    name: "document",
                    item: [
                        "Source",
                        "-",
                        "Save",
                        "NewPage",
                        "Preview",
                        "Print",
                        "-",
                        "Templates"
                    ]
                },
                {
                    name: "clipboard",
                    item: [
                        "Cut",
                        "Copy",
                        "Paste",
                        "PasteText",
                        "PasteFromWord",
                        "-",
                        "Undo",
                        "Redo"
                    ]
                },
                {
                    name: "editing",
                    item: [
                        "Find",
                        "Replace",
                        "-",
                        "SelectAll",
                        "-",
                        "Scayt"
                    ]
                },
                {
                    name: "from",
                    item: [
                        "From",
                        "Checkbox",
                        "Radio",
                        "TextField",
                        "Textarea",
                        "Select",
                        "Button",
                        "ImageButton",
                        "HiddenField"
                    ]
                },
                "/",
                {
                    name: "basicstyles",
                    item: [
                        "Bold",
                        "Italic",
                        "UnderLine",
                        "Strike",
                        "Subscript",
                        "Superscript",
                        "-",
                        "CopyFormatting",
                        "RemoveFormat"
                    ]
                },
                {
                    name: "paragraph",
                    item: [
                        "NumberedList",
                        "BulletedList",
                        "-",
                        "Outdent",
                        "Indent",
                        "-",
                        "Blockquote",
                        "CreateDiv",
                        "-",
                        "JustifyLeft",
                        "JustifyCenter",
                        "JustifyRight",
                        "JustifyBlock",
                        "-",
                        "BidiLtr",
                        "BidiRtl",
                        "Language"
                    ]
                },
                {
                    name: "link",
                    item: [
                        "Link",
                        "Unlink",
                        "Anchor"
                    ]
                },
                {
                    name: "insert",
                    item: [
                        "Image",
                        "Flash",
                        "Table",
                        "HorizontalRule",
                        "Smiley",
                        "SpecialChar",
                        "PageBreak",
                        "Iframe",
                        "InsertPre"
                    ]
                },
                "/",
                {
                    name: "styles",
                    item: [
                        "Styles",
                        "Format",
                        "Font",
                        "FontSize"
                    ]
                },
                {
                    name: "colors",
                    item: [
                        "TextColor",
                        "BGColor"
                    ]
                },
                {
                    name: "tool",
                    item: [
                        "Maximize",
                        "ShowBlocks"
                    ]
                },
                {
                    name: "about",
                    item: [
                        "About"
                    ]
                }
            ]
    } catch (error) {
      console.log(error);
    }
  },
},
created() {
    this.reset();
  },
  computed: {
    isInitial() {
      return this.currentStatus === STATUS_INITIAL;
    },
    issaving() {
      return this.currentStatus === STATUS_SAVING;
    },
    isSuccess() {
      return this.currentStatus === STATUS_SUCCESS;
    },
    isFailed() {
      return this.currentStatus === STATUS_FAILED;
    },
  },
  components: {
        VueCkeditor
    },
};
</script>
<style scoped>
.dropbox {
  outline: 2px dashed gray;
  outline-offset: -10px;
  background: #F0F8ff;
  color: dimgray;
  padding: 10px 10px;
  min-height: 200px;
  position: pointer;
}
.input-file {
  opacity: 0;
  width: 100%;
  height: 200px;
  position: absolute;
  cursor: pointer;
}
.dropbox:hover {
  background: #73C2FB;
}
.dropbox p {
  font-size: 1.2em;
  text-align: center;
  padding: 50px 0;
}
ul.pictures{
    list-style: none;
    padding: 0;
    margin: 0;
    float: left;
    padding-top: 10px;
    padding-bottom: 10px;
}
ul.pictures li {
    float: left;
}
ul.pictures li img{
    max-width: 180px;
    margin-right: 20px;
}
.clearfix{
    clear: both;
}
.thumbnail-pic img{
  width: 200px;
}
</style>