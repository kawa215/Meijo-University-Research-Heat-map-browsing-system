<template>
  <div id="demo" :class="[{ collapsed: collapsed }]">
    <div class="demo">
      <div class="container">
        <router-view />
      </div>
      <sidebar-menu
        :menu="menu"
        :collapsed="collapsed"
        :theme="selectedTheme"
        :show-one-child="true"
        @toggle-collapse="onToggleCollapse"
        @item-click="onItemClick"
      />
    </div>
  </div>
</template>

<script>
//サイドバーの表示内容の変数
var menuStr =[
  {//0
    header: true,
    title: "Clotheset",
    hiddenOnCollapse: true
  },
  {//1
    title: "Tops",
    icon: "fa fa-list-alt",
    child: [
      {
        title: "T-shirt",
        child: [
          {
            type:"tops",
            title: "ExT1",
            image: require("../image/TOP.png")
          },
          {
            type:"tops",
            title: "ExT2",
            image: require("../image/T1.png")
          },
        ]
      },
      {
        // title: "Knitwear / Sweater",
        title: "Hoodie",
        child: [
          // {
          //   title: "Ex1",
          //   child: [
          //     {
          //       title: "Page"
          //     },
          //     {
          //       title: "Page"
          //     }
          //   ]
          // }
        ]
      },
      {
        title: "Other",
        child:[
        ]
      }
    ]
  },
  {//2
    title: "Trouser/Skirt",
    icon: "fa fa-list-alt",
    child: [
      // {
      //   title: "Skinny Pants",
      //   child: [
      //     // {
      //     //   title: "pants1"
      //     // },
      //     // {
      //     //   title: "pants2"
      //     // }
      //   ]
      // },
      {
        title: "Jeans",
        child: [
          // {
          //   title: "jeans1",
          // }
        ]
      },
      // {
      //   title: "Skinny Pants",
      //   child: [
      //     // {
      //     //   title: "ski1"
      //     // },
      //     // {
      //     //   title: "ski2"
      //     // }
      //   ]
      // },
      {
        title: "Slacks",
            child: [
              // {
              //   title: "slack1"
              // },
              // {
              //   title: "slack2"
              // }
            ]
      },
      {
        title: "Other",
        child: [
        ]
      }
    ]
  },
  {//3
    title: "Outerwear",
    icon: "fa fa-list-alt",
    child: [
      // {
      //   title: "out1"
      // },
      // {
      //   title: "out2"
      // }
    ]
  },
  {//4
    title: "Shoes",
    icon: "fa fa-list-alt",
    child: [
      {
        title: "sneakers",
        child: [
        ]
      },
      {
        title: "Other",
        child: [
        ]
      }
    ]
  },
  {//5
    header: true,
    title: "FITTING ROOM",
    hiddenOnCollapse: true
  },
  {//6
    title: "Tops",
    icon: "fa fa-list-alt",
    child: [
      {
        title: "T-shirt",
        child: [
          {
            type:"tops",
            title: "ExT1",
            image: require("../image/TOP.png")
          },
          {
            type:"tops",
            title: "ExT2",
            image: require("../image/T1.png")
          },
        ]
      },
      {
        // title: "Knitwear / Sweater",
        title: "Hoodie",
        child: [
          // {
          //   title: "Ex1",
          //   child: [
          //     {
          //       title: "Page"
          //     },
          //     {
          //       title: "Page"
          //     }
          //   ]
          // }
        ]
      },
      {
        title: "Other",
        child:[
        ]
      }
    ]
  },
  {//7
    title: "Trouser/Skirt",
    icon: "fa fa-list-alt",
    child: [
      // {
      //   title: "Skinny Pants",
      //   child: [
      //     // {
      //     //   title: "pants1"
      //     // },
      //     // {
      //     //   title: "pants2"
      //     // }
      //   ]
      // },
      {
        title: "Jeans",
        child: [
          // {
          //   title: "jeans1",
          // }
        ]
      },
      // {
      //   title: "Skinny Pants",
      //   child: [
      //     // {
      //     //   title: "ski1"
      //     // },
      //     // {
      //     //   title: "ski2"
      //     // }
      //   ]
      // },
      {
        title: "Slacks",
            child: [
              // {
              //   title: "slack1"
              // },
              // {
              //   title: "slack2"
              // }
            ]
      },
      {
        title: "Other",
        child: [
        ]
      }
    ]
  },
  {//8
    title: "Outerwear",
    icon: "fa fa-list-alt",
    child: [
      // {
      //   title: "out1"
      // },
      // {
      //   title: "out2"
      // }
    ]
  },
  {//9
    title: "Shoes",
    icon: "fa fa-list-alt",
    child: [
      {
        title: "sneakers",
        child: [
        ]
      },
      {
        title: "Other",
        child: [
        ]
      }
    ]
  }
]
export default {
  name: "App",
  methods: {
    onToggleCollapse(collapsed) {
      console.log(collapsed);
      this.collapsed = collapsed;
    },
    onItemClick(event, item) {
      console.log("onItemClick");
      // console.log(event)
      // console.log(item)
    }
  },
  created() {
    let stdData = [];
    // var menuStr = [];
    //clotheset
    fetch("https://reiwafasshion.mybluemix.net/getbought").then((rep) => {
      return rep.json();
    }).then(contents => {
      for(const content of contents) {//店頭の商品
        console.log(content)
        switch(content['category']){
          case 'tops':
            // console.log(content['category'])
            switch(content['type']){
              case 't-shirt':
                menuStr[1]['child'][0]['child'].push({type:'tops', title:'shirt1', image: require('../image/'+content['id']+'.png')})
              break;
              case 'hoodie':
                menuStr[1]['child'][1]['child'].push({type:'tops', title: 'hoodie1', image: require('../image/'+content['id']+'.png')})
              break;
              default:
                menuStr[1]['child'][2]['child'].push({type:'tops', title:content['type'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          case 'bottoms':
            // console.log(content['category'])
            switch(content['type']){
              // case 'skinny pants':
              //   menuStr[2]['child'][0]['child'].push({type: 'bottoms', title: 'Skinny'+content['id'], image: require('../image/'+content['id']+'.png')})
              // break;
              case 'jeans':
                menuStr[2]['child'][0]['child'].push({type: 'bottoms', title: 'jeans'+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
              case 'slacks':
                menuStr[2]['child'][1]['child'].push({type: 'bottoms', title: 'slacks'+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
              default:
                // console.log(context['type'])
                menuStr[2]['child'][2]['child'].push({type: 'bottoms', title:content['type']+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          case 'shoes':
            switch(content['type']){
              case 'sneakers':
                // console.log(content['category'])
                menuStr[4]['child'][0]['child'].push({type:'shoes', title:'black', image: require('../image/'+content['id']+'.png')})
              break;
              default:
                menuStr[4]['child'][1]['child'].push({type:'shoes', title:content['type'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          default:
            // console.log(content['category'])
          break;
        }
      }
    })
    //fitting
    fetch("https://reiwafasshion.mybluemix.net/gettryon").then((rep) => {
      return rep.json();
    }).then(contents => {
      for(const content of contents) {
        console.log(content)
        switch(content['category']){
          case 'tops':
            // console.log(content['category'])
            switch(content['type']){
              case 't-shirt':
                menuStr[6]['child'][0]['child'].push({type:'tops', title:'shirt1', image: require('../image/'+content['id']+'.png')})
              break;
              case 'hoodie':
                menuStr[6]['child'][1]['child'].push({type:'tops', title: 'hoodie1', image: require('../image/'+content['id']+'.png')})
              break;
              default:
                menuStr[6]['child'][2]['child'].push({type:'tops', title:content['type'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          case 'bottoms':
            // console.log(content['category'])
            switch(content['type']){
              // case 'skinny pants':
              //   menuStr[7]['child'][0]['child'].push({type: 'bottoms', title: 'Skinny'+content['id'], image: require('../image/'+content['id']+'.png')})
              // break;
              case 'jeans':
                menuStr[7]['child'][0]['child'].push({type: 'bottoms', title: 'jeans'+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
              case 'slacks':
                menuStr[7]['child'][1]['child'].push({type: 'bottoms', title: 'slacks'+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
              default:
                menuStr[7]['child'][2]['child'].push({type: 'bottoms', title:content['type']+content['id'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          case 'shoes':
            switch(content['type']){
              case 'sneakers':
                // console.log(content['category'])
                menuStr[9]['child'][0]['child'].push({type:'shoes', title:'black', image: require('../image/'+content['id']+'.png')})
              break;
              default:
                menuStr[9]['child'][1]['child'].push({type:'shoes', title:content['type'], image: require('../image/'+content['id']+'.png')})
              break;
            }
          break;
          default:
            // console.log(content['category'])
          break;
        }
      }
    })
      // this.$data.menu = stdData;
  },
  data() {
    return {
      menu: menuStr
      /*[
        {
          header: true,
          title: "Clotheset",
          hiddenOnCollapse: true
        },
        {
          title: "Top",
          icon: "fa fa-list-alt",
          child: [
            {
              title: "T-shirt",
              child: [
                {
                  type:"tops",
                  title: "ExT1",
                  image: require("../image/TOP.png")
                },
                {
                  type:"tops",
                  title: "ExT2",
                  image: require("../image/T1.png")
                },
              ]
            },
            {
              title: "Knitwear / Sweater",
              child: [
                {
                  title: "Ex1",
                  child: [
                    {
                      title: "Page"
                    },
                    {
                      title: "Page"
                    }
                  ]
                }
              ]
            }
          ]
        },
        {
          title: "Trouser/Skirt",
          icon: "fa fa-list-alt",
          child: [
            {
              title: "Skinny Pants",
              child: [
                {
                  title: "pants1"
                },
                {
                  title: "pants2"
                }
              ]
            },
            {
              title: "Jeans",
              child: [
                {
                  title: "jeans1",
                }
              ]
            },
            {
              title: "Skinny Pants",
              child: [
                {
                  title: "ski1"
                },
                {
                  title: "ski2"
                }
              ]
            },
            {
              title: "Slacks",
                  child: [
                    {
                      title: "slack1"
                    },
                    {
                      title: "slack2"
                    }
                  ]
            }
          ]
        },
        {
          title: "Outerwear",
          icon: "fa fa-list-alt",
          child: [
            {
              title: "out1"
            },
            {
              title: "out2"
            }
          ]
        },
        {
          title: "Shoes",
          icon: "fa fa-list-alt",
          child: [
            {
              title: "shoes1"
            },
            {
              title: "shoes2"
            }
          ]
        },
        {
          header: true,s
          title: "SITYAKU",
          hiddenOnCollapse: true
        },
      ]*/,
      collapsed: false,
      selectedTheme: "Default-theme"
    };
  }
  /*methods: {
    onToggleCollapse(collapsed) {
      console.log(collapsed);
      this.collapsed = collapsed;
    },
    onItemClick(event, item) {
      console.log("onItemClick");
      // console.log(event)
      // console.log(item)
    }
  },
  created() {
    let stdData = [];
    // var menuStr = [];
    fetch("https://reiwafasshion.mybluemix.net/getbought").then((rep) => {
      return rep.json();
    }).then(contents => {
      for(const content of contents) {
        console.log(content)
      }
    })
    fetch("https://reiwafasshion.mybluemix.net/gettryon").then((rep) => {
      return rep.json();
    }).then(contents => {
      for(const content of contents) {
        // console.log(content)
      }
    })
      // this.$data.menu = stdData;
  }*/
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro:400,600");
body,
html {
  margin: 0;
  padding: 0;
}
body {
  font-family: "Source Sans Pro", sans-serif;
  font-size: 18px;
  background-color: #f2f4f7;
  color: #262626;
}
#demo {
  padding-left: 350px;
}
#demo.collapsed {
  padding-left: 50px;
}
.demo {
  padding: 50px;
}
.container {
  max-width: 900px;
}
pre {
  font-family: Consolas, monospace;
  color: #000;
  background: #fff;
  border-radius: 2px;
  padding: 15px;
  line-height: 1.5;
  overflow: auto;
}
</style>