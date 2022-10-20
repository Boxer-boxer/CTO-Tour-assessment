<template>
  <div class="container">
    <div class="message" v-if="messageActive">
      <div class="message-container">
        <button @click="closeMessage" class="close-btn"><span>x</span></button>
        <span class="message-bookmarked">Number of times this image has been bookmarked: {{bookmarked}}</span>
        <pictureSubmissionForm 
          @submit-picture="(formValue) => submitPicture(formValue)"
          :currImage="currImage"
          :validationMessage="validationMessage"
        />
      </div>
    </div>

    <filterButtons 
      @fetch-all="fetch()" @filter-values="(n) => filterValues(n)" 
      :folders=folders
    />

    <catPicture 
      v-for="pic in data"
      :pic="pic"
      :selectedImages="selectedImages"
      :key="pic.image_id"
      @bookmark="(v) => bookmark(v)"
    />

  </div>
</template>

<script>
import pictureSubmissionForm from "./components/pictureSubmissionForm.vue";
import filterButtons from "./components/filterButtons.vue";
import catPicture from "./components/catPicture.vue";

export default {
  name: 'App',
  components: {
    pictureSubmissionForm,
    filterButtons,
    catPicture
  },
  data() {
    return {
      data: [],
      messageActive: false,
      bookmarked: 0,
      currImage: null,
      selectedImages: [],
      folderName: "",
      folders: {},
      savedValue: '',
      validationMessage: ''
    }
  },
  methods: {
    async fetch() {
      const response = await fetch('https://tourscanner.com/interview/images');
      const data = await response.json();
      this.data=data;
      return data;
    },
    async bookmark(id) {
      const response = await fetch(`https://tourscanner.com/interview/save_image/${id}`);
      this.messageActive = true;
      this.bookmarked = await response.json();
      this.currImage = id;
    },
    closeMessage() {
      this.messageActive = false;
    },
    async submitPicture(formValue) {
      const id = formValue.currImage;
      this.folderName = formValue.folderName;

      if(this.folderName.length > 0) {
        const response = await fetch(`https://tourscanner.com/interview/save_image/${id}`, {
          method: 'POST',
        });
        const res = await response.json();
        if (res.success === 1) {
          this.selectedImages.push(id);
          if(this.folders[this.folderName]) {
            this.folders[this.folderName].push(id);
          } else {
            this.folders[this.folderName] = [(id)];
          }
          localStorage.setItem("storedSelectedImages", JSON.stringify(this.selectedImages));
          localStorage.setItem("storedFolders", JSON.stringify(this.folders));
          this.folderName = "";
        }
        this.messageActive = false;
      } else {
        this.validationMessage = "Please choose a name for the folder!"
      }
    },
    async filterValues(images){
      let allImgs = await this.fetch();
      let newArr = [];
      images.forEach((id) => {
        newArr.push(allImgs.filter((img) => img.image_id === id)[0])
      })
      this.data = newArr;
    }
  },
  mounted() {
    this.fetch();
    let storedSelectedImages = JSON.parse(localStorage.getItem("storedSelectedImages"));
    if(storedSelectedImages.length > 0 || storedSelectedImages !== null) {
      this.selectedImages = storedSelectedImages;
    }
    this.folders = JSON.parse(localStorage.getItem("storedFolders"));
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
  flex-wrap: wrap;
}
.image-container {
  display: flex;
  flex-direction: column;
  margin-right: 20px;
  margin-bottom: 20px;
  width: 300px;
}
.image-title {
  height: calc(16px * 3);
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;  
  overflow: hidden;
  margin-top: 10px;
  line-height: 16px;
  font-size: 13px;
}
.image-title-saved {
  font-weight: 900;
  color: rgb(111, 213, 144);;
}
.image {
  width: 100%;
  height: 272px;
  object-fit: cover;
}
.message {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0,0,0,0.5);
}
.message-container {
  display: flex; 
  align-items: flex-start;
  flex-direction: column;
  padding: 20px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 10px;
}
.image-container-saved {
  box-shadow: 0px 0px 5px 1px black;
}
.message-bookmarked {
  color: rgba(0, 0, 0, 1);
}
.message-form {
  margin-top: 10px;
  text-align: left;
}
.message-form > input[type=submit] {
  margin-top: 10px;
}
.message-input {
  width: 100%;
  margin-top: 10px;
}
.close-btn {
  align-self: flex-end;
}
.tab-container {
  display: flex;
  width: 100%;
  justify-content: center;
  flex-wrap: wrap;
}
.tab-container > button {
  margin-right: 10px;
  margin-bottom: 10px;
} 
.validation-message {
  color: red;
  display: block;
}
</style>
