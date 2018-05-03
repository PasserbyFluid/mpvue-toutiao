<template>
    <div class="content">
        <scroll-view scroll-y style="height:100%;"   @scrolltolower="toLow">
            <ul>
                <li v-for="(item,index) in artList" :key="index">
                    <!-- 无图 -->
                    <a href="/pages/article/main">
                        <div class="no-cover" v-if="!item.cover1">
                            <div class="item-content">
                                <h3 class="title">{{item.title}}</h3>
                                <div class="userinfo">
                                    <span>{{item.userReadUserInfo ? item.userReadUserInfo.nickName :'圆球天下'}}</span>
                                    <span>{{item.commentNum}}</span>
                                </div>
                            </div>
                        </div>
                    </a>
                    <!-- 单图 -->
                    <a :href="'/pages/article/main?id='+item.id+'&&articleType='+item.articleType">
                        <div class="one-cover"  v-if="item.coverType == 1">
                            <div class="item-content clearfix">
                                <div class="left">
                                    <div class="title">{{item.title}}</div>
                                    <div class="userinfo">
                                        <span>{{item.userReadUserInfo.nickName}}</span>
                                        <span>{{item.commentNum}}</span>
                                    </div>
                                </div>
                                <div class="right">
                                    <div class="img-box">
                                        <img :src="item.cover1" alt="">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </a>
                    <!-- 多图 -->
                    <a :href="'/pages/article/main?id='+item.id+'&&articleType='+item.articleType">
                        <div class="more-cover" v-if="item.coverType == 2">
                            <div class="item-content clearfix">
                                <h3 class="title">{{item.title}}</h3>
                                <div class="cover-box">
                                    <div class="cover">
                                        <img :src="item.cover1" alt="">
                                    </div>
                                    <div class="cover">
                                        <img :src="item.cover2" alt="">
                                    </div>
                                    <div class="cover">
                                        <img :src="item.cover3" alt="">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </a>
                </li>
            </ul>
            <p class="loading" v-if="onloading">加载中···</p>
        </scroll-view>
    </div>
</template>
<script>
import uuid from 'uuid/v1'
export default {
    props:['currentTab'],
    data() {
        return {
            artList:[],
            page:1,
            onloading:false
        }
    },
    methods:{
        getList() {
            this.onloading = true;
            wx.request({
                url: 'http://gateway.zdkj.com/content/view/queryArticle/123412323141234/'+this.currentTab.fieldId,
                success: (res) => {
                    this.onloading = false;
                        // console.log(res)
                    // if (res.data.code === 0) {
                        this.artList= (this.artList).concat(res.data.data.newsArticlePreviewHis);
                    // }
                },
                fail:(err) => {
                    this.onloading = false;
                }
            })
        },
        toLow(){
            console.log(2)
            this.page = this.page +1 ;
            this.getList()
        }
    },
    created() {
        this.getList()
    },
}
</script>
<style scoped>
::-webkit-scrollbar{
    width: 0;
    height: 0;
    background-color: transparent;
    /* display: none; */
}
.content{
    padding: 10px 15px;
    height:600px ;
    /* overflow-y: scroll; */
}
li{
    width: 100%;
}
.item-content{
    padding: 5px;
}
.clearfix::after{
    content: '';
    display: block;
    height: 0;
    clear: both;
}
.title{
    font-size: 16px;
    color: #333;
}
.one-cover .left{
    width: 220px;
    margin-right: 15px;
    float: left;
}
.one-cover .right{
    /* float: right;; */
    width: 100px;
    height: 65px;
    overflow: hidden;
}
.more-cover .cover-box{
    display: flex;
    align-items: center;
    justify-content: space-between
}
.more-cover .cover {
    width: 100px;
    height: 65px;
    overflow: hidden;
}
.more-cover .cover img{
    width: 100%;
}
.one-cover .right img{
    width: 100%;
    vertical-align: middle;
}
.userinfo span{
    font-size: 12px;
    color:rgb(153,153,153);
    margin-right: 15px;
}
.loading{
    font-size: 12px;
    text-align: center;
    color: #ccc;
}
</style>

