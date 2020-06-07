<template>
  <div class="ImageList">
    <!-- <img src="../../../dataset/cifar10-ep100-epi10-img5/img0048.png"/> -->
    <!-- a<img src="../../../dataset/cifar10-ep100-epi10-img5/img0048.png"/> -->
    <!-- <ul> -->
    <!-- <div v-for="icon in icons" :key="icon">
      <img :src="icon" v-if="icons" />
    </div> -->
    <!-- </ul> -->
  </div>
</template>
<script>
export default {
  name: "ImageList",
  // icons: [
  //   // require('..../dataset/cifar10-ep100-epi10-img5/img0048.png'),
  //   require('../../../dataset/cifar10-ep100-epi10-img5/img0048.png'),
  // ],
  created() {
    // console.log("aaa");
  },
  data: function() {
    return {
      icons: [
        require("../../../dataset/cifar10-ep100-epi10-img5/img0048.png"),
        require("../../../dataset/cifar10-ep100-epi10-img5/img1935.png")
      ]
    };
  }
};
</script>
