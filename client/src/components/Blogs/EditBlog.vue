<template>
  <div class="edit-blog-container">
    <form v-on:submit.prevent="editBlog">
      <senter><h1>แก้ไขข้อมูล</h1></senter>
      <p>ชื่อโรงเรียน<input type="text" v-model="blog.title" /></p>
      
      <transition name="fade">
        <div class="thumbnail-pic" v-if="blog.thumbnail != 'null'">
          <img :src="BASE_URL + blog.thumbnail" alt="thumbnail" />
        </div>
      </transition>
      
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
        <p v-if="isInitial">Drag your file(s) here to begin<br />or click to browse</p>
        <p v-if="isSaving">Uploading {{ fileCount }} files...</p>
        <p v-if="isSuccess">Upload Successful.</p>
      </div>
      
      <transition-group tag="ul" class="pictures">
        <li v-for="picture in pictures" v-bind:key="picture.id">
          <img :src="BASE_URL + picture.name" alt="picture image" />
          <br />
          <button v-on:click.prevent="useThumbnail(picture.name)">Upload</button>
          <button v-on:click.prevent="delFile(picture)">Delete</button>
        </li>
      </transition-group>
      
      <div class="clearfix"></div>
      <p>รายละเอียดโรงเรียน: <input type="text" v-model="blog.content" /></p>
      <p>หลักสูตรการศึกษา: <input type="text" v-model="blog.category" /></p>
      <p>ระดับการศึกษา: <input type="text" v-model="blog.status" /></p>
      <p>
        <button type="submit">ยืนยัน</button>
        <button v-on:click="navigateTo('/blogs')">กลับ</button>
      </p>
    </form>
  </div>
</template>

<script>
import BlogsService from "@/services/BlogsService";
import VueCkeditor from "vue-ckeditor2";
import UploadService from "../../services/UploadService";

const STATUS_INITIAL = 0,
  STATUS_SAVING = 1,
  STATUS_SUCCESS = 2,
  STATUS_FAILED = 3;

export default {
  components: { VueCkeditor },
  data() {
    return {
      BASE_URL: "http://localhost:8081/assets/uploads/",
      error: null,
      uploadError: null,
      currentStatus: null,
      uploadFieldName: "userPhoto",
      uploadedFileNames: [],
      pictures: [],
      pictureIndex: 0,
      blog: {
        title: "",
        thumbnail: "null",
        pictures: "null",
        content: "",
        category: "",
        status: "",
      },
      config: {
        toolbar: [
          ["Bold", "Italic", "Underline", "Strike", "Subscript", "Superscript"],
        ],
        height: 300,
      },
    };
  },
  methods: {
    async delFile(material) {
      let result = confirm("Want to delete?");
      if (result) {
        let dataJSON = { filename: material.name };
        await UploadService.delete(dataJSON);
        this.pictures = this.pictures.filter(picture => picture.id !== material.id);
      }
    },
    async editBlog() {
      try {
        await BlogsService.put(this.blog);
        this.$router.push({ name: "blogs" });
      } catch (err) {
        console.log(err);
      }
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async save(formData) {
      try {
        this.currentStatus = STATUS_SAVING;
        await UploadService.upload(formData);
        this.currentStatus = STATUS_SUCCESS;

        this.uploadedFileNames.forEach(uploadFilename => {
          if (!this.pictures.find(picture => picture.name === uploadFilename)) {
            this.pictureIndex++;
            this.pictures.push({ id: this.pictureIndex, name: uploadFilename });
          }
        });
        this.clearUploadResult();
      } catch (error) {
        console.log(error);
        this.currentStatus = STATUS_FAILED;
      }
    },
    filesChange(fieldName, fileList) {
      const formData = new FormData();
      if (!fileList.length) return;
      Array.from(fileList).forEach(file => {
        formData.append(fieldName, file, file.name);
        this.uploadedFileNames.push(file.name);
      });
      this.save(formData);
    },
    clearUploadResult() {
      setTimeout(() => this.reset(), 5000);
    },
    useThumbnail(filename) {
      this.blog.thumbnail = filename;
    },
  },
  computed: {
    isInitial() {
      return this.currentStatus === STATUS_INITIAL;
    },
    isSaving() {
      return this.currentStatus === STATUS_SAVING;
    },
    isSuccess() {
      return this.currentStatus === STATUS_SUCCESS;
    },
    isFailed() {
      return this.currentStatus === STATUS_FAILED;
    },
  },
  async created() {
    this.currentStatus = STATUS_INITIAL;
    try {
      let blogId = this.$route.params.blogId;
      this.blog = (await BlogsService.show(blogId)).data;
      this.pictures = JSON.parse(this.blog.pictures);
      this.pictureIndex = this.pictures.length;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style scoped>
.edit-blog-container {
  background-color: #ffeb3b; /* Yellow background */
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  display: flex;
  justify-content: center; /* Center content horizontally */
  align-items: center; /* Center content vertically */
  min-height: 100vh; /* Minimum height to fill the viewport */
}

h1 {
  color: #333; /* Dark text for the heading */
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  max-width: 600px;
  width: 100%;
  background-color: #fff; /* White background for form */
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 2px solid #ccc;
  border-radius: 5px;
  font-size: 1em;
  background-color: #f9f9f9;
  color: #333;
}

button {
  background-color: #4caf50; /* Green button */
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1em;
  cursor: pointer;
  margin: 5px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049; /* Darker green on hover */
}

.dropbox {
  outline: 2px dashed #999; /* Light gray dashed outline */
  background-color: #f0f0f0; /* Light background for dropbox */
  padding: 20px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  transition: background-color 0.3s ease;
  cursor: pointer;
  margin-top: 15px;
}

.dropbox:hover {
  background-color: #e0e0e0; /* Slightly darker on hover */
}

.input-file {
  opacity: 0;
  width: 100%;
  height: 100%;
  position: absolute;
  cursor: pointer;
}

.pictures {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding-top: 20px;
}

.pictures li {
  margin: 10px;
  text-align: center;
}

.pictures li img {
  max-width: 150px;
  border-radius: 5px;
  transition: transform 0.3s ease;
}

.pictures li img:hover {
  transform: scale(1.05);
}

.thumbnail-pic img {
  width: 200px;
  border-radius: 5px;
}

.clearfix {
  clear: both;
}

@media (max-width: 768px) {
  form {
    padding: 15px;
  }

  .pictures li img {
    max-width: 120px;
  }

  button {
    width: 100%;
  }
}
</style>
