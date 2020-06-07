<template>
  <div id="datalist">
    <ul class="list">
      <li v-for="list in dataset" :key="list" class="list-item">
        <a
          href="#"
          v-bind:class="{ active: active === list }"
          @click="dropdown(list)"
          >{{ list }}</a
        >
        <ul v-if="active === list">
          <li v-for="item in sublistlabel" :key="item" class="sublist-item">
            <a href="#" v-on:click="datasel = item" v-bind:label="item">{{
              item
            }}</a>
          </li>
        </ul>
      </li>
    </ul>
    {{ datasel }}
    <ul class="list">
      <li v-for="list in epoch" :key="list" class="list-item">
        <a
          href="#"
          v-bind:class="{ active: active === list }"
          @click="dropdown(list)"
          >{{ list }}</a
        >
        <ul v-if="active === list">
          <li v-for="item in subepochlabel" :key="item" class="sublist-item">
            <a href="#" v-on:click="epochClick(item)" v-bind:label="item">{{
              item
            }}</a>
          </li>
        </ul>
      </li>
    </ul>
    {{ epochsel }}
    <ul class="list">
      <li v-for="list in method" :key="list" class="list-item">
        <a
          href="#"
          v-bind:class="{ active: active === list }"
          @click="dropdown(list)"
          >{{ list }}</a
        >
        <ul v-if="active === list">
          <li v-for="item in submethodlabel" :key="item" class="sublist-item">
            <a href="#" v-on:click="methodClick(item)" v-bind:label="item">{{
              item
            }}</a>
          </li>
        </ul>
      </li>
    </ul>
    {{ methodsel }}
    <!-- <ul class="list">
      <li v-for="list in img" :key="list" class="list-item">
        <a
          href="#"
          v-bind:class="{ active: active === list }"
          @click="dropdown(list)"
          >{{ list }}</a
        >
        <ul v-if="active === list">
          <li v-for="item in subimglabel" :key="item" class="sublist-item">
            <a href="#" v-on:click="imgsel = item" v-bind:label="item">{{
              item
            }}</a>
          </li>
        </ul>
      </li>
    </ul>
    {{ imgsel }} -->
  </div>
</template>

<script>
export default {
  name: "DataSelectBar",
  props: {

    num: null,
  },
  data: () => {
    return {
      // dataset: ["Mens", "Womens", "Kids"],
      propsCheck: [false,false,false,false,false],
      dataset: ["Dataset"],
      datasel: "cifar10",
      sublistlabel: ["cifar10", "imagenet", "mnist"],
      active: "",
      epoch: ["Epoch"],
      epochsel: "",
      subepochlabel: ["100", "200", "300", "400", "500"],
      method: ["method"],
      methodsel: "",
      submethodlabel: [
        "Deconvnet",
        "GuidedBackprop",
        "Gradient",
        "SmoothGrad",
        "IntegratedGradients",
        "Input*Gradient",
        "LRP-Epsilon",
        "DeepTaylor",
        "LRP-PresetA"
      ]
    };
  },
  methods: {
    dropdown(list) {
      //   console.log(this.active);
      this.active = this.active === list ? "" : list;
    },
    epochClick: function(item) {
      // 第二引数で子のデータを格納
      this.epochsel = item;
      this.$emit("epoch-event", this.epochsel);
    },
    methodClick: function(item) {
      // 第二引数で子のデータを格納
      this.methodsel = item;
      this.$emit("method-event", this.methodsel);
    },
  },
  computed: {
    // checkTrue: function() {
    //   for(var i = 0; i < 5; i++){
    //     // if(this.props.val[i].files == null) {
    //     //   this.propsCheck[i] = false;
    //     // }
    //     // else {
    //     //   this.propsCheck[i] = true;
    //     // }
    //     this.propsCheck[i] = false;
    //   };
    //   return this.propsCheck;
    // },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#datalist {
  width: 100%;
  
}
ul {
  padding: 0;
}
li {
  list-style: none;
}
a {
  color: whitesmoke;
  text-decoration: none;
}
.list {
  padding: 0;
  border: solid 1px #ccc;
}

.list-item a:hover {
  background-color: rgb(162, 179, 230);
}
.list-item a {
  position: relative;
  padding: 8px;
  border-bottom: solid 1px #ccc;
}
.list-item a {
  display: block;
}
.list-item:last-of-type a {
  border-bottom: none;
}
.list-item > a::after {
  content: "▼";
  position: absolute;
  top: 12px;
  right: 10px;
  font-size: 10px;
}
.list-item > a.active::after {
  content: "▲";
}
.sublist-item {
  font-size: 1.05rem;
}
.sublist-item :hover {
  background-color: royalblue;
}
</style>
