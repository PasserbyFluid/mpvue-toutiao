<template>
  <div class="container" @click="clickHandle('test click', $event)">
    <!-- 顶部滑动tab -->
    <div class="tab-list">
        <!-- <span class="more-field">+更多</span> -->
        <scroll-view scroll-x="true" class="tabs" style=" white-space: nowrap; display: flex" :scroll-left='scrollLeft'>
            <ul>
                <li :data-index='index' :data-id='item.fieldId' @click="checkedItem" :class="{isChecked:activeTab == index}" v-for="(item,index) in fields" :key="index">{{item.fieldName}}</li>
            </ul>
        </scroll-view>
    </div>
    <swiper :current="activeTab"  class="swiper-box" duration="300" @change="switchTab">
        <swiper-item v-for="(item,index) in fields" :key="index">
            <!-- 当前显示的内容区 -->
            <content-list v-if="activeTab == index" :currentTab="item"></content-list>
            <!-- <div v-if="activeTab == index">{{item.fieldName}}-{{activeTab}}</div> -->
            <!-- <content-v v-if="index==currentTab" :currentTab="item"></content-v> -->
        </swiper-item>
    </swiper>
  </div>
</template>

<script>
import contentList from '@/components/content'

export default {
  data () {
    return {
        motto: 'Hello World',
        userInfo: {},
        fields:[],
        activeTab:0,
        // 超过一屏先做滑动
        scrollLeft:0
        // isChecked:0
    }
  },

  components: {
    contentList
  },

    methods: {
        clickHandle (msg, ev) {
        //   console.log('clickHandle:', msg, ev)
        },
        getFields() {
            wx.request({
                url: 'http://gateway.zdkj.com/content/quaryDataFields', 
                success: (res) => {
                    this.fields= res.data.data;
                    // console.log(this.fields)
                }
            })
        },
        checkedItem(e) {
            let index = e.target.dataset.index;
            this.activeTab = index;
            if (this.activeTab >4) {
                this.scrollLeft = (this.activeTab-4)*61.5
            }else{
                this.scrollLeft = 0;
            }
            // console.log(e.target.dataset.id)
        },
        switchTab(e) {
            // this.activeTab = e.detail.current;
            this.activeTab = e.mp.detail.current;
            if (this.activeTab >4) {
                this.scrollLeft = (this.activeTab-4)*61.5
            }else{
                this.scrollLeft = 0;
            }
            // console.log(e.mp.detail)
        },
    },

    created () {
        // 调用应用实例的方法获取全局数据
        // this.getUserInfo();
        this.getFields();
        // 根据初始activeTab值判断向左移动距离
        if (this.activeTab > 5) {
           this.scrollLeft = (this.activeTab-4)*61.5
        }
            // console.log(this.scrollLeft)
    }
}
</script>

<style scoped>
/* .tab-list{
    width: 100%;
    height: 40px;
    background: #fff;
    position: fixed;
    z-index: 999;
} */
.tabs{
    margin-left: 3px;
    position: fixed;
    z-index: 999;
    width: 100%;
    /* top: 0; */
}
.swiper-box{
    padding-top: 50px;
}
li{
    display: inline-block;
    padding: 0 12px;
    height: 40px;
    line-height: 40px;
    background: #fff;
    font-size: 16px;
    box-sizing: border-box;
}
.isChecked{
    color: red;
    border-bottom: 2px solid red;
}
swiper {
    width: 100%;
    height: 100%;
}
view {
    display:block;
    text-align:left;
}
.more-field{
    display: inline-block;
    position: fixed;
    right: 0;
    top: 0;
    width: 61.5px;
    height: 40px;
    line-height: 40px;
    background: #fff;
}

</style>
