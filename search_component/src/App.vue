<template>
  <div id="app">
    <div class="container"></div>
    <!-- <search /> -->
    <!-- 组件设计时，会使用哪一些iview组件 -->
    <!-- Tabs(容器组件)>TabPane(每个) -->
    <div class="item-intro-detail" ref="itemIntroDetail">
      <div class="item-intro-nav item-tabs">
        <Tabs>
          <TabPane label="商品介绍">
            <div class="remarks-title">
              <span>商品介绍</span>
            </div>
            <div class="item-intro-img" ref="itemIntroGoods">
              <img :src="item" alt=""
              v-for="(item,index) in goodsInfo.goodsImg" 
              :key="index">
            </div>
          </TabPane>
          <TabPane label="商品评价">
            <div class="remarks-container">
              <div class="remarks-title">
                <span>商品评价</span>
              </div>
              <div class="remarks-analyse-box">
                <div class="remarks-analyse-goods">
                  <i-circle :percent="goodsInfo.remarks.goodAnalyse" stroke-color="#e4393c">
                    <span class="remarks-analyse-num">{{goodsInfo.remarks.goodAnalyse}}%</span>
                    <p class="remarks-analyse-title">好评率</p>
                  </i-circle>
                </div>
                <div class="remarks-analyse-tags">
                  <Tag checkable :color="tagsColor[index % 4]" v-for="(item,index) in goodsInfo.remarks.remarksTags" :key="index">{{item}}</Tag>
                </div>
              </div>
              <div class="remarks-bar">
                <span>追评({{goodsInfo.remarks.remarksNumDetail[0]}})</span>
                <span>好评({{goodsInfo.remarks.remarksNumDetail[1]}})</span>
                <span>中评({{goodsInfo.remarks.remarksNumDetail[2]}})</span>
                <span>差评({{goodsInfo.remarks.remarksNumDetail[3]}})</span>
              </div>
              <div class="remarks-box" v-for="(item,index) in goodsInfo.remarks.detail" :key="index">
                <div class="remarks-user">
                  <Avatar icon="person" />
                  <span class="remarks-user-name">{{item.username}}</span>
                </div>
                <div class="remarks-content-box">
                  <p>
                    <Rate disabled :value="item.values" allow-half class="remarks-star"></Rate>
                  </p>
                  <p class="remarks-content">{{item.content}}</p>
                  <p class="remarks-sub">
                    <span class="remarks-item">{{item.goods}}</span>
                    <span class="remarks-time">{{item.time}}</span>
                  </p>
                </div>
              </div>
              <div class="remarks-page">
                <Page :total="40" size="small" show-elevator show-sizer></Page>
              </div>
            </div>
            </TabPane>
        </Tabs>
      </div>
    </div>
    <router-view/>
  </div>
</template>
<script>
// import Search from './components/Search';
import api from './api/index';  /*api模块：前后端分离的点 */
export default {
  data() {
    return {
      // 复杂页面：有组件来细化
      // 聚焦于数据设计；data--数据驱动页面
      goodsInfo: {
        goodsImg: [],
        param: {},   /*json 商品参数*/
        remarks:{    //json
          detail: [],  /*评论列表*/ 
          remarksNumDetail: [], /*好评数量 */
          remarksTags: [],/*评价标签 */
          goodAnalyse:  59  /*好评率 */
        }
      }
    }
  },
  mounted () {
    // 发送ajax请求；将模板没有数据的部分（静态）尽快挂载，如container布局；
    // 可以让用户快速的看到结构（占位符作用），同时立即启动数据通信，把数据通过MVVM显示到已挂载的组件上
    // 生命周期，组件挂载到页面上后（节点），利用来请求数据
    api
      .getGoodsInfo()
      .then(data => {
        console.log(data);
        this.getGoodsInfo = data;
      })
  },
  components:{
    // Search
  }
}
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  padding: 0 15px;
}
.item-intro-detail{
  margin-left: 30px;
  width: calc(80vw - 300px);
}
.remarks-title {
  padding-left: 15px;
  height: 36px;
  font-size: 16px;
  font-weight: bolder;
  line-height: 36px;
  color: #666666;
  background-color: #F7F7F7;
}
/* 改变便签页样式 */

.item-tabs > .ivu-tabs > .ivu-tabs-bar .ivu-tabs-tab{
  border-radius: 0px;
  color: #999;
  height: 38px;
  background: #F7F7F7;
}
.item-tabs > .ivu-tabs > .ivu-tabs-bar .ivu-tabs-tab-active{
  color: #fff;
  background-color: #E4393C;
}
.item-tabs > .ivu-tabs > .ivu-tabs-bar .ivu-tabs-tab-active:before{
  content: '';
  display: block;
  width: 100%;
  height: 1px;
  color: #fff;
  background: #F7F7F7;
  position: absolute;
  top: 0;
  left: 0;
}
</style>
