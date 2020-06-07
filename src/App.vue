<template>
  <div id="app">
    <div class="upload">
      <HelloWorld msg="Browse System" />
      <FileUpload v-on:file="parentFile" />
    </div>
    <div class="content">
      <div class="contentleft">
        <div class="dataSelectbarleft">
          <DataSelectBar
            num="1"
            v-on:epoch-event="parentMethod"
            v-on:method-event="methodEvent"
          ></DataSelectBar>
        </div>
        <div class="imagevleft">
          <Imagev
            v-on:val-event="parentTest"
            v-bind:epoch="epoch"
            v-bind:methodd="method"
          />
        </div>
      </div>
      <div class="contentright">
        <div class="imagevright">
          <Imagev
            v-on:val-event="parentTest"
            v-bind:epoch="epoch2"
            v-bind:methodd="method2"
          />
        </div>
        <div class="dataSelectbarright">
          <DataSelectBar
            num="2"
            v-on:epoch-event="parentMethod2"
            v-on:method-event="methodEvent2"
          ></DataSelectBar>
          <!-- <FileUpload v-on:file="parentFile" /> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import DataSelectBar from "./components/DataSelectBar.vue";
// import ImageList from "./components/ImageList.vue";
import Imagev from "./components/Image.vue";
import FileUpload from "./components/FileUpload.vue";

export default {
  name: "app",
  components: {
    HelloWorld,
    DataSelectBar,
    // ImageList,
    Imagev,
    FileUpload
  },
  data: () => {
    return {
      epoch: {
        ep: "",
        file: ""
      },
      epoch2: {
        ep: "",
        file: ""
      },
      method: {
        sel: ""
      },
      method2: {
        sel: ""
      },
      file: [
        {
          epoch: 100,
          files: null
        },
        {
          epoch: 200,
          files: null
        },
        {
          epoch: 300,
          files: null
        },
        {
          epoch: 400,
          files: null
        },
        {
          epoch: 500,
          files: null
        }
      ]
    };
  },
  methods: {
    parentMethod: function(fromChildVal) {
      this.epoch["ep"] = fromChildVal;
      //100-500チェック
      var b;
      for (var i = 100; i < 600; i = i + 100) {
        if (fromChildVal == String(i)) {
          i = i / 100 - 1;
          b = i;
          break;
        }
      }
      this.epoch["file"] = this.file[b]["files"];
    },
    parentMethod2: function(fromChildVal) {
      this.epoch2["ep"] = fromChildVal;
      //100-500チェック
      var b;
      for (var i = 100; i < 600; i = i + 100) {
        if (fromChildVal == String(i)) {
          i = i / 100 - 1;
          b = i;
          break;
        }
      }
      this.epoch2["file"] = this.file[b]["files"];
    },
    methodEvent: function(methodsel) {
      this.method["sel"] = methodsel;
    },
    methodEvent2: function(methodsel) {
      this.method2["sel"] = methodsel;
    },
    parentTest: function(fromChildText) {
      alert(fromChildText);
    },
    parentFile: function(fromChildVal) {
      var b = 0;
      for (var i = 100; i < 600; i = i + 100) {
        if (fromChildVal["epoch"] == String(i)) {
          this.file[b]["files"] = fromChildVal["files"];
          alert(
            "Epoch" + fromChildVal["epoch"] + "のファイルをアップロードしました"
          );
          break;
        }
        b++;
      }
      this.epoch["ep"] = fromChildVal["epoch"];
      this.epoch2["ep"] = fromChildVal["epoch"];
      b = 0;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: whitesmoke;
}
.left {
  display: flex;
}

.upload {
  height: 190px;
  width: 100%;
  display: -webkit-box;
  padding-top: 20px;
  padding-left: 20px;
  background-color: #131313;
  border: 2px solid black;
}

.dataSelectbarleft {
  height: 620px;
  width: 180px;
  margin-top: 30px;
  border: 2px solid black;
  background-color: #131313;
}
.dataSelectbarright {
  margin: 30px 0 0 auto;
  height: 620px;
  width: 180px;
  border: 2px solid black;
  background-color: #131313;
}
.content {
  display: -webkit-box;
}
.contentleft {
  width: 800px;
  background-color: #070707;
  display: -webkit-box;
}
.contentright {
  width: 800px;
  background-color: #070715;
  display: -webkit-box;
}
.imagevleft {
  width: 570px;
  /* border: 2px solid red; */
  padding: 10px;
}
.imagevright {
  width: 570px;
  /* border: 2px solid red; */
  padding: 10px;
}
</style>
