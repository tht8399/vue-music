<template>
  <div class="recommand">
    <div class="loading" v-if="loading">
      {{loadingState}}
    </div>
    <div class="content" v-if="!loading">
      <swiper :options="swiperOptionIn" class="swiper-position">
        <swiper-slide v-for="s in slider">
          <img class="recommand-swiper-img" v-lazy="s.pic">
        </swiper-slide>
        <div class="swiper-pagination-white swiper-pagination-position" slot="pagination"></div>
      </swiper>
      <div class="hotlist">
        <div class="list-title">
          <p class="title-text">热门歌单</p>
        </div>
        <div class="list-content">
          <div class="list-item"
               v-for="item in songList"
               @click="$router.push({name: 'cd', params: {id: item.dissid}})">
            <div class="list-img">
              <img v-lazy="item.imgurl">
              <div class="listen-count">
                <img src="../assets/icon-erji.svg" alt="">
                <span>{{item.listennum | listenCount}}</span>
              </div>
            </div>
            <div class="list-info">
              <p class="list-name">{{item.dissname}}</p>
              <p class="list-author">{{item.author}}</p>
            </div>
          </div>
        </div>
      </div>
      <!--<div class="hotlist">-->
      <!--<div class="list-title">-->
      <!--<p class="title-text">电台</p>-->
      <!--</div>-->
      <!--<div class="list-content">-->
      <!--<div class="list-item" v-for="item in radioList">-->
      <!--<div class="list-img">-->
      <!--<img v-lazy="item.picUrl">-->
      <!--</div>-->
      <!--<div class="list-info">-->
      <!--<p class="list-name">{{item.Ftitle}}</p>-->
      <!--</div>-->
      <!--</div>-->
      <!--</div>-->
      <!--</div>-->
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {swiper, swiperSlide} from 'vue-awesome-swiper'
  export default {
    components: {
      swiper,
      swiperSlide
    },
    data () {
      return {
        loadingState: '正在加载...',
        loading: true,
        slider: [],
        radioList: [],
        songList: [],
        swiperOptionIn: {
          pagination: '.swiper-pagination-white',
          paginationClickable: true,
          autoplay: 3000,
        }
      }
    },
    methods: {},
    created: function () {
      this.$store.dispatch('getRecommands').then((response) => {
        this.loading = false
        this.slider = response.data.data.focus
        this.songList = response.data.data.hotdiss.list
      }, (responce) => {
        this.loadingState = '加载失败'
      })
    },
    filters: {
      listenCount: num => Math.round(num / 1000) / 10 + '万'
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .recommand {
    margin-top: 50px;
  }

  .loading {
    color: #999;
    width: 100%;
    height: 150px;
    line-height: 150px;
    text-align: center;
  }

  .recommand-swiper-img {
    width: 100%;
    display: block;
  }

  .swiper-slide {

  }

  .swiper-position {
    position: relative;
  }

  .swiper-pagination-position {
    position: absolute;
    z-index: 1;
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  .hotlist {
    display: flex;
    flex-direction: column;
    background: #fff;
    margin-top: 10px;
    box-shadow: 0 0 10px #DDD;
  }

  .hotlist .list-title {
    height: 50px;
    text-align: center;
    line-height: 50px;
  }

  .hotlist .list-content {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .hotlist .list-content .list-item {
    width: 50%;
    cursor: pointer;
  }

  .hotlist .list-content .list-item .list-img{
    position:relative;
  }

  .hotlist .list-content .list-item .list-img img {
    width: 100%;
  }

  .hotlist .list-content .list-item .list-img .listen-count{
    position: absolute;
    bottom:5px;
    left:5px;
    color:#fff;
    font-size: 12px;
    display:flex;
    flex-direction:row;
    align-items: center;
  }

  .hotlist .list-content .list-item .list-img .listen-count img{
    width:15px;
    height:15px;
    display: inline-block;
    margin-right:3px;
  }

  .hotlist .list-content .list-item .list-info {
    padding: 0 5px;
    font-weight: 300;
    font-size: 14px;
    margin-bottom: 10px;
  }

  .hotlist .list-content .list-item .list-info .list-name {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

  .hotlist .list-content .list-item .list-info .list-author {
    color: #777777
  }
</style>
