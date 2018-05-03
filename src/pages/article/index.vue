<template>
    <div class="article">
        <div class="content">
            <!-- 视频 -->
            <div class="art-content" v-if="articleType == 2">
                <video :src="artCon.url"   controls autoplay @fullscreenchange='fullscreenchange'></video>
                <!-- <div v-html="artCon.content"></div> -->
            </div>
            <h3 class="title">{{artCon.title}}</h3>
            <div class="user">
                <div class="user-avatar">
                    <img :src="artCon.userReadUserInfo.photo" alt="">
                </div>
                <div class="info">
                    <p class="name">{{artCon.userReadUserInfo.nickName}}</p>
                    <p class="time">{{artCon.createTime}}</p>
                </div>
            </div>
            <!-- 文章 -->
            <div class="art-content" v-if="articleType == 0">
                <div v-html="artCon.content"></div>
            </div>
            <!-- 图集 -->
            <div class="art-content" v-if="articleType == 1">
                <div class="item" v-for="(item,index) in artCon.imgList" :key="index">
                    <img :src="item.photo" alt="">
                    <div class="introduction">{{item.introduction}}</div>
                </div>
                <!-- <div v-html="artCon.content"></div> -->
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            artCon:{
                title:'',
                content:'',
                userReadUserInfo:{
                    photo:'',
                    nickName:''
                },
                url:'',
                imgList:[]
            },
            articleType:0
        }
    },
    onLoad(){
        this.articleType = this.$root.$mp.query.articleType;
        this.getDetail(this.$root.$mp.query)
    },
    created(){
    },
    methods:{
        getDetail(query) {
            // articleType:0文章1图集2视频 ;fieldId是当前文章图集视频的领域id
            if (query.articleType == 0) {
                // 文章
                wx.request({
                    url: 'http://gateway.zdkj.com/content/view/article/'+query.id, 
                    success: (res) => {
                        this.artCon= res.data.data;
                        this.artCon.title = res.data.data.title;
                        // this.artCon.content = 
                        let str = res.data.data.content;
                        const reg = /<img/ig ;
                        str = str.replace(reg,'<img style="width:100%"');
                        this.artCon.content = str;
                        console.log(this.artCon)
                    }
                })
                
            }else if( query.articleType == 1){
                // 图集
                wx.request({
                    url: 'http://gateway.zdkj.com/content/view/atlas/'+query.id, 
                    success: (res) => {
                        this.artCon= res.data.data;
                        this.artCon.title = res.data.data.title;
                        this.artCon.imgList = res.data.data.listNewsAtlasImage;
                        console.log(res)
                    }
                })
            }else {
                // 视频
                wx.request({
                    url: 'http://gateway.zdkj.com/content/view/video/'+query.id, 
                    success: (res) => {
                        this.artCon= res.data.data;
                        this.artCon.title = res.data.data.title;
                        this.artCon.url = res.data.data.listNewsVideoStore[0].url;
                        console.log(res)
                    }
                })
            }
        },
        // 全屏
        fullscreenchange(e){
            console.log(e.detail)
        }
        
    }
}
</script>
<style scoped>
.content{
    padding: 15px;
}
.user{
    margin-top: 20px;
}
.user-avatar{
    width: 50px;
    height: 50px;
    border-radius: 50%;
    overflow: hidden;
    float: left;
}
.user-avatar img{
    width: 100%;
}
.user .info{
    margin-left: 60px
}
.name{
    font-size: 16px;
    color: #333333;
} 
.time{
    font-size: 12px;
    margin-top: 10px;
    color: #999999;
}
.art-content{
    margin-top: 15px;
    font-size: 16px;
    line-height: 20px;
}
video{
    width: 100%;
}
image{
    width: 100%;
}
.introduction{
    padding: 5px 0;
}
p>img{
    width: 100%;
}

</style>
