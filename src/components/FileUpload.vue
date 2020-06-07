<template>
  <div class="fileUpload">
    <div id="example">
      <h5>Upload Files</h5>
      <input name="1" type="radio" id="100" value="100" v-model="upFiles.epoch"/>
      <label for="one">100</label>
      <input name="1" type="radio" id="200" value="200" v-model="upFiles.epoch" />
      <label for="two">200</label>
      <input name="1" type="radio" id="300" value="300" v-model="upFiles.epoch" />
      <label for="one">300</label>
      <input name="1" type="radio" id="400" value="400" v-model="upFiles.epoch" />
      <label for="two">400</label>
      <input name="1" type="radio" id="500" value="500" v-model="upFiles.epoch" />
      <label for="two">500</label>
      <p>epoch：{{ upFiles["epoch"] }}</p>
      <input type="file" webkitdirectory v-on:change="fileList" />
    </div>
  </div>
</template>
<script>
export default {
  name: "fileUpload",
  created() {},
  methods: {
    fileList: function fileListDirectory(e) {
      this.upFiles["files"] = e.target.files;
      this.$emit("file", this.upFiles);
    },
    setEpoch: function set(value){
      this.upFiles["epoch"] = value;
    }
  },
  data: function() {
    return {
      upFiles: {
        epoch: null,
        files: null
      }
    };
  }
};
</script>
<style>
#example {
  width: 350px;
  padding: 10px;
  /* display: inline-block; */
}
#example label {
  margin: 8px;
  /* display: inline-block; */
}
.fileUpload {
  height: 100%;
  margin-left: 20px;
  background-color: #1D1D1D;
  border-left: 2px solid white;
}
</style>
