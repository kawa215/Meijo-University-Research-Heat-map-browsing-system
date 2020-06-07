<template>
  <div class="hello">
    <!-- <div id="display" class="red" v-html="content">結果がここに表示されます。</div> -->

    <div class="row" v-if="ok">
      <div
        class="col-sm-6 col-md-3 col-xl-4"
        v-for="(result, index) in processedHeatData"
        :key="result.name"
      >
        <a @click="imgClick(result.name)" class="card" href="#">
          <h5 class="card-title">{{ index }}:{{ result.name }}</h5>
          <img class="card-img" :src="result.img" :alt="result.name" />
        </a>
      </div>
    </div>
    <div class="heatArea" v-if="okheat">
      <div class="heatNameArea">
        <div class="heatName" v-for="(name, index) in methodList" :key="name">
          <div class="heatNameItem">
            <h5>{{ name }}</h5>
          </div>
          <div class="heatNmaeItem">
            <button
              v-if="index !== 0"
              @click="btnClick(name, 0, 1)"
              class="btn-flat-border"
            >
              epoch
            </button>
          </div>
        </div>
      </div>
      <div class="heatImageArea">
        <div
          class="img"
          v-for="result in processedImgHeatData"
          :key="result.name"
        >
          <a :href="result.img" :key="result.name">
            <!-- <h5 class="card-title">{{ result.name }}</h5> -->
            <img class="card-img" :src="result.img" :alt="result.name" />
            <!-- <img
              v-if="adaptCheck(results.name) === index"
              class="card-img"
              :src="results.img"
              :alt="results.name"
            /> -->
          </a>
        </div>
      </div>
    </div>
    <!-- <button @click="okepoch = true">ok</button> -->

    <div class="epochArea" v-if="okepoch">
      <div
        v-for="(result, index) in processedWayData"
        :key="result"
        class="itemGroupArea"
      >
        <div class="epochNameArea">
          <img :src="NewnumHeatData[0].img" :key="NewnumHeatData[0].img" />
          <h5>{{ result }}</h5>
        </div>
        <div class="epochTransitionArea">
          
            <img
              :src="images[index]['heatmap'][indexx].img"
              :key="images[index]['heatmap'][indexx].img"
              class="slideshow"
            />
          
          <!-- <button @click="this.indexx = 0">res</button> -->
          <h5>{{ images[index]["heatmap"][indexx].name }}</h5>
        </div>
        <div class="epochLastArea">
          <img
            :src="getLastHeatImage(images, index)"
            :key="getLastHeatImage(images, index)"
          />
          <h5>{{ getLastHeatName(images, index) }}</h5>
        </div>
      </div>
    </div>

    <!-- <div class="row" v-if="okheat">
      <div
        class="col-sm-6 col-md-3 col-xl-2"
        v-for="(result, index) in processedImgHeatData"
        :key="result.name"
      >
        <a class="card" :href="result.img">
          <h5 class="card-title">{{ index }}:{{ result.name }}</h5>
          <img class="card-img" :src="result.img" :alt="result.name" />
        </a>
      </div>
    </div> -->

    <!-- <div>
      <ul>
        <li>
          <ol>
            <li>画像</li>
            <li>画像</li>
            <li>画像</li>
          </ol>
        </li>
        <li>
          <ol>
            <li>画像</li>
            <li>画像</li>
            <li>画像</li>
            <li>画像</li>
            <li>画像</li>
            <li>画像</li>
          </ol>
        </li>
      </ul>
    </div> -->
  </div>
</template>

<script>
export default {
  name: "Imagev",
  props: {
    epoch: {
      ep: "",
      file: ""
    },
    methodd: {
      sel: ""
    }
  },
  data() {
    return {
      indexx: 0,
      e: [],
      images: [
        {
          first: {},
          heatmap: []
        },
        {
          first: {},
          heatmap: []
        },
        {
          first: {},
          heatmap: []
        }
      ],
      wayName: [],
      fileName: "",
      // info: null,
      val: this.epoch,
      valMethod: this.methodd,
      // content: "",
      files: null,
      // checkResult: [],
      heatData: [
        {
          title: "cifar10",
          child: []
        }
      ],
      classData: [
        //0
        {
          // 入力画像関連 png
          input: [],
          // モデル関連
          model: [
            {
              // モデルの構造を表現する画像 arch.png
              arch: null,
              // 最終エポック後のモデル  h5
              heatmap: [],
              // 指定されたエポック後のモデル h5
              modelEpoch: []
            }
          ],
          // 学習関連
          learning: [
            // 学習中の損失及び正解率の変化
            {
              history: null,
              loss: null,
              acc: null
            }
          ],
          //評価関連 テストデータ損失と正解率
          evaluation: null,
          //可視化関連 指定されたエポック後のモデルに対する可視化結果
          visualize: [{}]
        }
      ],
      ok: false,
      okheat: false,
      okepoch: false,
      numHeatData: [],
      methodList: [
        "Deconvnet",
        "GuidedBackprop",
        "Gradient",
        "SmoothGrad",
        "IntegratedGradients",
        "Input*Gradient",
        "LRP-Epsilon",
        "DeepTaylor",
        "LRP-PresetA"
      ],
      NewnumHeatData: []
      // numsortResult: []
    };
  },
  watch: {
    "val.file": function(val) {
      // this.ok = val;
      this.ok = false;
      this.okheat = false;
      this.okepoch = false;
      this.numHeatData.length = 0;
      this.arrayClear();
      if (val.file !== null) {
        this.fileList();
      }
    },
    "valMethod.sel": function(val) {
      // this.ok = val;
      var index = 0;

      switch (this.wayName.length) {
        case 1:
          index = 1;
          this.wayName.push(val);
          this.btnClick(val, index, 0);
          break;
        case 2:
          index = 2;
          this.wayName.push(val);
          this.btnClick(val, index, 0);
          break;
        case 3:
          index = 1;
          this.wayName.length = 1;
          this.wayName.push(val);
          this.btnClick(val, index, 0);
          break;
        default:
          break;
      }
    }
    // valMethod : function(val) {
    // if(this.wayName.length === 3) {
    //   this.wayName.length = 0;
    // }
    // this.wayName.push(val)
    // }
  },
  // mounted: {
  //   setInterval: (function()  {
  //       this.indexx = this.indexx < this.images.length - 1 ? this.indexx + 1 : 0;
  //   }, 3000),
  // },
  mounted() {
    setInterval(() => {
      this.indexx =
        this.indexx < this.images[0]["heatmap"].length - 1
          ? this.indexx + 1
          : 0;
    }, 1000);
  },
  computed: {
    processedHeatData: function() {
      let resultsHeat = this.classData[0]["input"];
      // console.log("a");
      return resultsHeat;
    },
    processedImgHeatData: function() {
      let resultsHeat = this.NewnumHeatData;
      // console.log("a");
      // resultsHeat.sort(this.filesort);
      return resultsHeat;
    },
    processedWayData: function() {
      let resultsWay = this.wayName;
      // console.log("a");
      // resultsHeat.sort(this.filesort);
      return resultsWay;
    }
    // processedImgTest: function() {}
  },

  methods: {
    fileList: function fileListDirectory() {
      //ファイル取得
      // const files = e.target.files;
      const files = this.val.file;
      this.files = files;
      var numF = files.length;
      var nameF = new Array();
      var path = new Array();

      for (var i = 0; i < numF; i++) {
        //ファイル名
        nameF[i] = files[i].name;
        path[i] = files[i].webkitRelativePath; // 行Ａ
        //パスvar fileName = 'test.csv';

        var check = this.check(nameF[i]);
        // this.checkResult.push(nameF[i]);
        // this.checkResult.push(check);

        switch (check) {
          case 1:
            this.classData[0]["input"].push({
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            });
            break;
          case 2:
            this.classData[0]["model"][0]["arch"] = {
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            };
            break;
          case 3:
            this.classData[0]["model"][0]["heatmap"].push({
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            });
            break;
          case 4:
            this.classData[0]["model"][0]["modelEpoch"].push({
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            });
            break;
          case 5:
            this.classData[0]["learning"][0]["history"] = {
              name: nameF[i],
              img:
                "../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i]
            };
            break;
          case 6:
            this.classData[0]["learning"][0]["loss"] = {
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            };
            break;
          case 7:
            this.classData[0]["learning"][0]["acc"] = {
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            };
            break;
          case 8:
            this.classData[0]["evaluation"] = {
              name: nameF[i],
              img:
                "../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i]
            };
            break;
          case 9:
            this.classData[0]["visualize"].push({
              name: nameF[i],
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                nameF[i])
            });
            break;
          default:
            break;
        }

        // this.classData[0]["input"]["name"].sort();

        this.classData[0]["input"].sort(this.compare);

        var type = nameF[i].split(".");

        var r = "cifar10-ep" + this.val.ep + "-epi10-img5/";

        if (type[type.length - 1].toLowerCase() == "png") {
          this.heatData[0]["child"].push({
            name: nameF[i],
            img: require("../../dataset/" + r + nameF[i])
          });
        }

        //先頭文字チャック
      }

      for (i = 0; i < numF; i++) {
        if (nameF[i] == ".") {
          // 行Ｂ
          this.content += "folder：";
        } else {
          this.content += "file：";
        }
        this.content += path[i] + "<br>";
      }

      this.ok = true;
    },
    test: function(messeage) {
      this.content = messeage;
    },
    split: function(fileName) {
      return fileName.split(/\.(?=[^.]+$)-/);
    },
    check: function(file) {
      switch (true) {
        case /^img/.test(file):
          return 1;
        case /arch$/.test(file):
          return 2;
        case /last$/.test(file):
          return 3;
        case file.indexOf("epoch") !== -1:
          return 4;
        case file.indexOf("history") !== -1:
          return 5;
        case file.indexOf("loss") !== -1:
          return 6;
        case file.indexOf("acc") !== -1:
          return 7;
        case file.indexOf("eval") !== -1:
          return 8;
        case file.indexOf("heatmap") !== -1:
          return 9;
        default:
          break;
      }
    },
    compare: function(a, b) {
      return String(a["name"]).match(/\d+/) - String(b["name"]).match(/\d+/);
    },
    // filesort: function(a, b) {
    //   const a1 = parseInt(a["name"].replace(/[^0-9]/g, ""), 10);
    //   const b1 = parseInt(b["name"].replace(/[^0-9]/g, ""), 10);
    //   const a2 = a1 !== a1 ? 0 : a1;
    //   const b2 = b1 !== b1 ? 0 : b1;

    //   if (a2 > b2) {
    //     return 1;
    //   } else if (a2 < b2) {
    //     return -1;
    //   }
    //   return 0;
    // },

    filesort: function(numHeatData) {
      // const numcopy = numHeatData;
      this.NewnumHeatData.length = 10;
      // var flag1 = false;
      // var flag2 = false;
      for (var i = 0; i < numHeatData.length; i++) {
        switch (true) {
          case /^img/.test(numHeatData[i].name):
            this.NewnumHeatData[0] = {
              img: numHeatData[i].img,
              name: "0"
            };
            break;
          case numHeatData[i].name.indexOf("Deconvnet") !== -1:
            this.NewnumHeatData[1] = {
              img: numHeatData[i].img,
              name: "1"
            };
            break;
          case numHeatData[i].name.indexOf("GuidedBackprop") !== -1:
            this.NewnumHeatData[2] = {
              img: numHeatData[i].img,
              name: "2"
            };
            break;
          case numHeatData[i].name.indexOf("IntegratedGradients") !== -1:
            this.NewnumHeatData[6] = {
              img: numHeatData[i].img,
              name: "6"
            };
            break;
          case numHeatData[i].name.indexOf("Input*Gradient") !== -1:
            this.NewnumHeatData[7] = {
              img: numHeatData[i].img,
              name: "7"
            };
            break;
          case numHeatData[i].name.indexOf("Gradient") !== -1:
            this.NewnumHeatData[3] = {
              img: numHeatData[i].img,
              name: "3"
            };
            break;
          case numHeatData[i].name.indexOf("SmoothGrad") !== -1:
            this.NewnumHeatData[4] = {
              img: numHeatData[i].img,
              name: "4"
            };
            break;
          case numHeatData[i].name.indexOf("LRP-Epsilon") !== -1:
            this.NewnumHeatData[5] = {
              img: numHeatData[i].img,
              name: "5"
            };
            break;

          case numHeatData[i].name.indexOf("DeepTaylor") !== -1:
            this.NewnumHeatData[8] = {
              img: numHeatData[i].img,
              name: "8"
            };
            break;
          case numHeatData[i].name.indexOf("LRP-PresetA") !== -1:
            this.NewnumHeatData[9] = {
              img: numHeatData[i].img,
              name: "9"
            };
            break;
          default:
            break;
        }
      }
    },

    arrayClear: function() {
      this.heatData[0]["child"].length = 0;
      this.classData[0]["input"].length = 0;
      this.classData[0]["model"][0]["arch"] = null;
      this.classData[0]["model"][0]["heatmap"].length = 0;
      this.classData[0]["model"][0]["modelEpoch"].length = 0;
      this.classData[0]["learning"][0]["history"] = null;
      this.classData[0]["learning"][0]["loss"] = null;
      this.classData[0]["learning"][0]["acc"] = null;
      this.classData[0]["evaluation"] = null;
      this.classData[0]["visualize"].length = 0;
    },
    imgClick: function(name) {
      this.ok = false;
      this.okheat = false;
      this.indexx = 0;
      this.numHeatData.length = 0;

      if (this.methodList[0].indexOf("img") === 0) {
        this.methodList.shift();
      }
      this.methodList.unshift(name);

      //画像番号の画像たちをかきあつメル
      var reg = /(.*)(?:\.([^.]+$))/;
      var imgNum = name.match(reg)[1];
      // チェックする
      var r = "cifar10-ep" + this.val.ep + "-epi10-img5/";
      for (var i = 0; i < this.heatData[0]["child"].length; i++) {
        if (
          this.heatData[0]["child"][i].name.indexOf(imgNum) !== -1 &&
          this.heatData[0]["child"][i].name.indexOf("last") !== -1
        ) {
          this.numHeatData.push({
            name: this.heatData[0]["child"][i].name,
            img: require("../../dataset/" +
              r +
              this.heatData[0]["child"][i].name)
          });
        }
      }

      for (var f = 0; f < this.classData[0]["input"].length; f++) {
        if (this.classData[0]["input"][f].name.indexOf(imgNum) !== -1) {
          this.numHeatData.push({
            name: this.classData[0]["input"][f].name,
            img: require("../../dataset/" +
              r +
              this.classData[0]["input"][f].name)
          });
        }
      }
      //ソート
      // this.numcopy = this.numHeatData
      this.filesort(this.numHeatData);
      this.okheat = true;
    },

    btnClick: function(wayName, index, btn) {
      //クリック時は1 それ以外は0
      if (btn === 1) {
        this.wayName[0] = wayName;
        this.wayName.length = 1;
        this.okheat = false;
      }
      // this.wayName[0] = wayName;
      // this.e.push("クリック");
      // this.okheat = false;

      //ファイル名表示
      // this.fileName = this.methodList[0];
      //長さ確保
      this.images[index]["first"] = null;
      this.images[index]["heatmap"].length = 0;

      //どこまでのエポックを見るか
      // switch (this.val.ep) {
      //   case 100:
      //     this.images.length = 11;
      //     break;
      //   case 200:
      //     this.images.length = 21;
      //     break;
      //   case 300:
      //     this.images.length = 31;
      //     break;
      //   case 400:
      //     this.images.length = 41;
      //     break;
      //   case 500:
      //     this.images.length = 51;
      //     break;
      //   default:
      //     break;
      // }

      //最初の画像
      this.images[index]["first"] = {
        img: require("../../dataset/cifar10-ep" +
          this.val.ep +
          "-epi10-img5/" +
          this.methodList[0]),
        name: this.methodList[0]
      };

      //選択された画像番号
      var imgNum = this.getBaseName(this.methodList[0]);

      // 選択された手法の遷移画像を配列で集める (last覗く)
      for (var ep = 10; ep < this.val.ep; ep = ep + 10) {
        // epochごと
        for (
          var r = 0;
          r < this.classData[0]["model"][0]["modelEpoch"].length;
          r++
        ) {
          //gradients回避しないといけない
          if (
            //方法
            this.classData[0]["model"][0]["modelEpoch"][r].name.indexOf(
              wayName
            ) !== -1 &&
            //エポック数
            this.classData[0]["model"][0]["modelEpoch"][r].name.indexOf(
              "epoch" + this.zeroPadding(ep, 3)
            ) !== -1 &&
            // 画像番号
            this.classData[0]["model"][0]["modelEpoch"][r].name.indexOf(
              imgNum
            ) !== -1
          ) {
            if (wayName === "Gradient") {
              //この二つはパス
              if (
                this.classData[0]["model"][0]["modelEpoch"][r].name.indexOf(
                  "Input*Gradient"
                ) !== -1 ||
                this.classData[0]["model"][0]["modelEpoch"][r].name.indexOf(
                  "IntegratedGradients"
                ) !== -1
              ) {
                continue;
              }
            }

            this.images[index]["heatmap"].push({
              img: require("../../dataset/cifar10-ep" +
                this.val.ep +
                "-epi10-img5/" +
                this.classData[0]["model"][0]["modelEpoch"][r].name),
              name: "epoch :" + this.zeroPadding(ep, 3)
            });
          }
        }
      }

      //last push
      for (var e = 0; e < this.classData[0]["visualize"].length; e++) {
        if (
          this.classData[0]["visualize"][e].name.indexOf(wayName) !== -1 &&
          this.classData[0]["visualize"][e].name.indexOf(imgNum) !== -1 &&
          this.classData[0]["visualize"][e].name.indexOf("last") !== -1
        ) {
          if (wayName === "Gradient") {
            //この二つはパス
            if (
              this.classData[0]["visualize"][e].name.indexOf(
                "Input*Gradient"
              ) !== -1 ||
              this.classData[0]["visualize"][e].name.indexOf(
                "IntegratedGradients"
              ) !== -1
            ) {
              continue;
            }
          }
          this.images[index]["heatmap"].push({
            img: require("../../dataset/cifar10-ep" +
              this.val.ep +
              "-epi10-img5/" +
              this.classData[0]["visualize"][e].name),
            name: "epoch :" + this.val.ep,
            match: "e=" + e + "name:" + this.classData[0]["visualize"][e].name
          });
        }
      }

      // this.images.sort(this.compare);
      this.images[index]["heatmap"].unshift(this.images[index]["first"]);
      if (btn === 1) this.okepoch = true;
    },

    adaptCheck: function(fileName) {
      if (fileName.indexOf("img") === 0) {
        return 0;
      }
      for (var r = 0; r <= this.methodList.length; r++) {
        if (fileName.indexOf(this.methodList[r]) !== -1) return r;
      }
    },
    zeroPadding: function(NUM, LEN) {
      return (Array(LEN).join("0") + NUM).slice(-LEN);
    },
    getBaseName: function(str) {
      var base = new String(str).substring(str.lastIndexOf("/") + 1);
      if (base.lastIndexOf(".") != -1)
        base = base.substring(0, base.lastIndexOf("."));
      return base;
    },
    getLastHeatImage: function(images, index) {
      return images[index]["heatmap"][images[index]["heatmap"].length - 1].img;
    },
    getLastHeatName: function(images, index) {
      return images[index]["heatmap"][images[index]["heatmap"].length - 1].name;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  color: #42b983;
}
.hello {
}
.heatArea {
  width: 450px;
  display: -webkit-box;
  margin-left: 100px;
  /* background-color: aliceblue; */
}
.heatImageArea {
  display: inline-block;
  border: 2px solid whitesmoke;
  border-radius: 10px; /* CSS3草案 */
  -webkit-border-radius: 10px; /* Safari,Google Chrome用 */
  -moz-border-radius: 10px;
}
.img {
  height: 200px;
  width: 200px;
}
.heatNameArea {
  width: 200px;
  border: 2px solid whitesmoke;
  border-radius: 10px; /* CSS3草案 */
  -webkit-border-radius: 10px; /* Safari,Google Chrome用 */
  -moz-border-radius: 10px;
}
.heatName {
  height: 200px;
  display: block;
  align-items: center;
  border-bottom: 0.1px solid whitesmoke;
}

.btn-flat-border {
  width: 80px;
  height: 40px;
  display: inline-block;
  padding: 0.3em 1em;
  text-decoration: none;
  color: green;
  border: solid 2px #67c5ff;
  border-radius: 3px;
  transition: 0.4s;
  background: snow;
  margin-top: 20px;
}

.btn-flat-border:hover {
  background: #67c5ff;
  color: white;
}

.card {
  color: green;
}
.card :hover {
  color: black;
  background: aquamarine;
  border: 2px solid aquamarine;
}
.heatNameItem {
  width: 100%;
  height: 30%;
  padding-top: 30px;
}
.epochArea {
  padding: 10px;
  height: 650px;
  width: 600px;
  border: 2px solid whitesmoke;
  border-radius: 10px; /* CSS3草案 */
  -webkit-border-radius: 10px; /* Safari,Google Chrome用 */
  -moz-border-radius: 10px;
}
.itemGroupArea {
  width: 100%;
  height: 200px;
  border-bottom: 5px solid teal;
  display: -webkit-box;
}
.epochNameArea {
  width: 30%;
  height: 100%;
  
}
.epochNameArea img {
  width: 100%;
  height: 80%;
  /* border: 1px solid whitesmoke; */
}
.epochTransitionArea {
  width: 40%;
  height: 100%;

  display: block;
}
.epochTransitionArea img{
  border: 1px solid whitesmoke;
}
.epochLastArea {
  width: 30%;
  height: 100%;
 
}
.epochLastArea img {
  width: 100%;
  height: 80%;
  border: 1px solid whitesmoke;
}
.slideshow {
  width: 80%;
  height: 80%;
}
</style>
