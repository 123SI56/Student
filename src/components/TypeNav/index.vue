<template>
  <!-- 商品分类导航 -->
  <div class="type-nav">
    <div class="container">
      <div @mouseleave="moveOutDiv" @mouseenter="isShow = true">
        <h2 class="all">全部商品分类</h2>
        <!-- name="sort"对应的后期写过度类的名称前缀 -->
        <transition name="sort">
          <div class="sort" v-show="isShow">
            <div class="all-sort-list2" @click="toSearch">
              <div
                class="item"
                :class="{ item_on: index === currentIndex }"
                v-for="(c1, index) in categoryList"
                :key="c1.categoryId"
                @mouseenter="moveInDiv(index)"
              >
                <h3>
                  <!-- <a href="">{{ c1.categoryName }}</a> -->
                  <!-- 第一种写法：使用声明式导航 -->
                  <!-- <router-link
                  :to="{
                    name: 'search',
                    query: {
                      categoryName: c1.categoryName,
                      category1Id: c1.categoryId,
                    },
                  }"
                >{{ c1.categoryName }}</router-link> -->

                  <!-- 第二种写法：编程式导航 -->
                  <!-- <a
                  href="javascript:;"
                  @click="
                    $router.push({
                      name: 'search',
                      query: {
                        categoryName: c1.categoryName,
                        category1Id: c1.categoryId,
                      },
                    })
                  "
                  >{{ c1.categoryName }}</a -->
                  <!-- > -->
                  <!-- 第三种方式，事件委派 -->
                  <a
                    href="javascript:;"
                    :data-categoryName="c1.categoryName"
                    :data-category1Id="c1.categoryId"
                    >{{ c1.categoryName }}</a
                  >
                </h3>
                <div class="item-list clearfix">
                  <div class="subitem">
                    <dl
                      class="fore"
                      v-for="(c2, index) in c1.categoryChild"
                      :key="c2.categoryId"
                    >
                      <dt>
                        <!-- <a href="">{{ c2.categoryName }}</a> -->
                        <!-- <router-link
                        :to="{
                          name: 'search',
                          query: {
                            categoryName: c2.categoryName,
                            category2Id: c2.categoryId,
                          },
                        }"
                      >{{ c2.categoryName }}</router-link> -->

                        <!-- <a
                        href="javascript:;"
                        @click="
                          $router.push({
                            name: 'search',
                            query: {
                              categoryName: c2.categoryName,
                              category2Id: c2.categoryId,
                            },
                          })
                        "
                        >{{ c2.categoryName }}</a -->
                        <!-- > -->
                        <a
                          href="javascript:;"
                          :data-categoryName="c2.categoryName"
                          :data-category2Id="c2.categoryId"
                          >{{ c2.categoryName }}</a
                        >
                      </dt>
                      <dd>
                        <em
                          v-for="(c3, index) in c2.categoryChild"
                          :key="c3.categoryId"
                        >
                          <!-- <a href="">{{ c3.categoryName }}</a> -->
                          <!-- <router-link
                        :to="{
                          name: 'search',
                          query: {
                            categoryName: c3.categoryName,
                            category3Id: c3.categoryId,
                          },
                        }"
                      >{{ c3.categoryName }}</router-link> -->
                          <!-- <a
                          href="javascript:;"
                          @click="
                            $router.push({
                              name: 'search',
                              query: {
                                categoryName: c3.categoryName,
                                category3Id: c3.categoryId,
                              },
                            })
                          "
                          >{{ c3.categoryName }}</a
                        > -->

                          <a
                            href="javascript:;"
                            :data-categoryName="c3.categoryName"
                            :data-category3Id="c3.categoryId"
                            >{{ c3.categoryName }}</a
                          >
                        </em>
                      </dd>
                    </dl>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </transition>
      </div>
      <nav class="nav">
        <a href="###">服装城</a>
        <a href="###">美妆馆</a>
        <a href="###">尚品汇超市</a>
        <a href="###">全球购</a>
        <a href="###">闪购</a>
        <a href="###">团购</a>
        <a href="###">有趣</a>
        <a href="###">秒杀</a>
      </nav>
    </div>
  </div>
</template>



<script>
import { mapState } from "vuex";
// import _ from 'lodash'//体积大  _代表lodash内所有的功能函数组成的对象
import throttle from "lodash/throttle";

export default {
  name: "TypeNav",
  // created也可以发请求的
  data() {
    return {
      currentIndex: -1,
      isShow: true,
    };
  },
  mounted() {
    if (this.$route.path !== "/home") {
      this.isShow = false;
    }


    // 在这里发请求不合适，home和search切换的时候，会重复发请求拿数据，而数据永远是不变的，所以没必要
    // this.$store.dispatch("getCategoryList"); //才会去触发store的action去发请求
    //dispatch之后，会调用action里面的函数，发请求，数据是存储在vuex当中‘
    //vue当中没有,你得从vuex当中把数据捞到vue当中然后才能使用
  },
  //从vuex当中把需要的数据，映射到vue组件当中
  computed: {
    // 什么时候可以传数组
    //1、不能使用模块
    //2、映射的名称必须和state当中数据的名称一致
    ...mapState({
      // state代表是总的store当中的state
      categoryList: (state) => state.home.categoryList,
    }),
  },
  methods: {
    //节流之前
    // moveInDiv(index){
    //   console.log(index)
    //   this.currentIndex = index
    // },

    // moveInDiv:function(index){
    //   console.log(index)
    //   this.currentIndex = index
    // },

    //节流之后
    moveInDiv: throttle(
      function (index) {
        console.log(index);
        this.currentIndex = index;
      },
      20,
      { trailing: false }
    ),

    //点击三级分类跳转到search页面（事件委派）
    toSearch(event) {
      //事件委派的回调函数
      // 1、用户点击的是不是a
      // 2、每个a标签传递的参数是不一样的
      let target = event.target;
      let dataset = target.dataset;
      // console.log(dataset)
      let { categoryname, category1id, category2id, category3id } = dataset;
      if (categoryname) {
        //可以判定点击的一定是a标签
        let location = {
          name: "search",
        };
        let query = {
          categoryName: categoryname,
        };
        if (category1id) {
          query.category1Id = category1id;
        } else if (category2id) {
          query.category2Id = category2id;
        } else {
          query.category3Id = category3id;
        }

        location.query = query;

        if(this.$route.params){
          location.params = this.$route.params
        }

        this.$router.push(location);
      }
    },

    moveOutDiv() {
      this.currentIndex = -1;
      if (this.$route.path !== "/home") {
        this.isShow = false;
      }
    },
  },
};
</script>

<style lang="less" scoped>
.type-nav {
  border-bottom: 2px solid #e1251b;

  .container {
    width: 1200px;
    margin: 0 auto;
    display: flex;
    position: relative;

    .all {
      width: 210px;
      height: 45px;
      background-color: #e1251b;
      line-height: 45px;
      text-align: center;
      color: #fff;
      font-size: 14px;
      font-weight: bold;
    }

    .nav {
      a {
        height: 45px;
        margin: 0 22px;
        line-height: 45px;
        font-size: 16px;
        color: #333;
      }
    }

    .sort {
      position: absolute;
      left: 0;
      top: 45px;
      width: 210px;
      height: 461px;
      position: absolute;
      background:yellowgreen;
      z-index: 999;
      

      &.sort-enter{
        opacity: 0;
        height: 0;
      }

      &.sort-enter-to{
        opacity: 1;
        height: 461px;
      }

      &.sort-enter-active{
        transition: all .5s;
      }

      .all-sort-list2 {
        .item {
          h3 {
            line-height: 30px;
            font-size: 14px;
            font-weight: 400;
            overflow: hidden;
            padding: 0 20px;
            margin: 0;

            a {
              color: #333;
            }
          }

          .item-list {
            display: none;
            position: absolute;
            width: 734px;
            min-height: 460px;
            background: #f7f7f7;
            left: 210px;
            border: 1px solid #ddd;
            top: 0;
            z-index: 9999 !important;

            .subitem {
              float: left;
              width: 650px;
              padding: 0 4px 0 8px;

              dl {
                border-top: 1px solid #eee;
                padding: 6px 0;
                overflow: hidden;
                zoom: 1;

                &.fore {
                  border-top: 0;
                }

                dt {
                  float: left;
                  width: 54px;
                  line-height: 22px;
                  text-align: right;
                  padding: 3px 6px 0 0;
                  font-weight: 700;
                }

                dd {
                  float: left;
                  width: 615px;
                  padding: 3px 0 0;
                  overflow: hidden;

                  em {
                    float: left;
                    height: 14px;
                    line-height: 14px;
                    padding: 0 8px;
                    margin-top: 5px;
                    border-left: 1px solid #ccc;
                  }
                }
              }
            }
          }

          &.item_on {
            background-color: hotpink;
            .item-list {
              display: block;
            }
          }
        }
      }
    }
  }
}
</style>
