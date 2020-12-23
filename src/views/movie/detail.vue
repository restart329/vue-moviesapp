<template>
    <div id="detailContrainer" class="slide-enter-active">
        <Header title="影片详情">
            <i class="iconfont icon-right" @click="handleToBack"></i>
        </Header>
        <Loading v-if="isLoading" />
        <div v-else id="content" class="contentDetail">
        <div class="detail_list">
        <div class="detail_list_bg" :style="{ 'background-image' : 'url('+ detailMovie.poster +')' }"></div>
            <div class="detail_list_filter"></div>
            <div class="detail_list_content">
                <div class="detail_list_img">
                    <img :src="detailMovie.poster" alt="">
                </div>
                <div class="detail_list_info">
                    <h2>{{ detailMovie.name }}</h2>
                    <p>{{ detailMovie.grade }}分</p>
                    <p>{{ detailMovie.category }}</p>
                    <p>{{ detailMovie.nation }} / {{ detailMovie.runtime }}分钟</p>
                    <p>{{ detailMovie.premiere }}</p>
                </div>
            </div>
        </div>
        <div class="detail_intro">
            <p>
                {{ detailMovie.synopsis }}
            </p>
        </div>
        <div class="actorsimg">
          <div class="text"><span>演职人员</span></div>
          <div class="detail_actors swiper-container" ref="detail_actors">
              <ul class="swiper-wrapper">
                  <li v-for="(item,index) in detailMovie.actors" :key="index" class="swiper-slide">
                      <div>
                          <img :src="item.avatarAddress" alt="">
                      </div>
                       <span class="actorsname">{{item.name}}</span>
                       <span class="actorstype">{{item.role}}</span>
                  </li>
              </ul>
          </div>
        </div>
        <div class="filmimg">
          <div class="text"><span>剧照</span></div>
          <div class="detail_player swiper-container" ref="detail_player">
              <ul class="swiper-wrapper">
                  <li v-for="(item,index) in detailMovie.photos" :key="index" class="swiper-slide">
                      <div>
                          <img :src="item" alt="">
                      </div>
                  </li>
              </ul>
          </div>
        </div>
        <div class="btn">
          选座购票
        </div>
    </div>
</div>
</template>

<script>

import Header from '@/components/header'
import Swiper from 'swiper'
import 'swiper/swiper-bundle.css'
import axios from 'axios'

export default {
  name: 'Detail',
  data () {
    return {
      detailMovie: {},
      isLoading: false
    }
  },
  components: {
    Header
  },
  props: ['filmId'],
  methods: {
    handleToBack () {
      this.$router.back()
    }
  },
  mounted () {
    console.log(this.filmId)
    axios({
      url: `https://m.maizuo.com/gateway?filmId=${this.filmId}&k=2617724`,
      headers: {
        'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"1605253643172292613079041","bc":"110100"}',
        'X-Host': 'mall.film-ticket.film.info'
      }
    }).then((res) => {
      var msg = res.data.msg
      if (msg === 'ok') {
        console.log(res.data)
        this.detailMovie = res.data.data.film
        this.isLoading = false
        this.$nextTick(() => {
        /* eslint-disable no-new */
          var swiper1 = new Swiper(this.$refs.detail_player, {
            slidesPerView: 'auto',
            freeMode: true,
            freeModeSticky: true,
            direction: 'horizontal'
          })
          /* eslint-disable no-new */
          var swiper2 = new Swiper(this.$refs.detail_actors, {
            slidesPerView: 'auto',
            freeMode: true,
            freeModeSticky: true,
            direction: 'horizontal'
          })
        })
      }
    })
  }
}
</script>

<style scoped>
#detailContrainer{ position: absolute; left:0; top:0; z-index: 100; width:100%; min-height:100%; background:white;}
#detailContrainer.slide-enter-active{ animation:0.3s slideMove;}
@keyframes slideMove{
    0%{ transform : translateX(100%); }
    100%{ transform : translateX(0); }
}
#content.contentDetail{ display: block; margin-bottom:0;}
#content .detail_list{ height:200px; width:100%; position: relative; overflow: hidden;}
.detail_list .detail_list_bg{ width:100%; height:100%; background: 0 40%; filter: blur(20px); background-size:cover; position: absolute; left: 0; top: 0;}
.detail_list .detail_list_filter{ width:100%; height:100%; position: absolute;background-color: #40454d;opacity: .55; position: absolute; left: 0; top: 0; z-index: 1;}
.detail_list .detail_list_content{ display: flex; width:100%; height:100%; position: absolute; left: 0; top: 0; z-index: 2;}
.detail_list .detail_list_img{ width:108px; height: 150px; border: solid 1px #f0f2f3; margin:20px;}
.detail_list .detail_list_img img{ width:100%; height: 100%;}
.detail_list .detail_list_info{ margin-top: 20px;}
.detail_list .detail_list_info h2{ font-size: 20px; color:white; font-weight: normal; line-height: 40px; margin-bottom: 10px;}
.detail_list .detail_list_info p{ color:white; line-height: 20px; font-size: 14px; color:#ccc; margin-top: 10px;}
#content .detail_intro{ padding: 10px;font-size: 14px;text-indent: 2em;color: gray;margin-bottom: 40px;margin-top: 20px;}
#content .detail_player,#content .detail_actors { margin:20px;}
#content .detail_player{margin-bottom: 60px;}
.detail_player .swiper-slide{ width:150px; height: 100px; margin-right: 20px; text-align: center; font-size: 14px;}
.detail_player .swiper-slide img{ width:100%; margin-bottom: 5px;}
.detail_actors .swiper-slide{ width:85px; height: 150px; margin-right: 20px; text-align: center; font-size: 14px;position: relative;}
.detail_actors .swiper-slide img{ width:100%; margin-bottom: 5px;position: relative;top: -16px;}
#content .actorsimg{margin-bottom: 40px;}
.actorsimg .actorsname{ display:block;height: 20px;width: 100%;position: absolute;top: 105px;z-index: 100;font-size: 10px;padding: 4px;}
.actorsimg .actorstype{ display:block;height: 20px;width: 100%;position: absolute;top: 125px;font-size: 10px;padding: 4px;opacity: 0.6;}
/* .detail_actors .swiper-slide p:nth-of-type(2){ color:#999;} */
.actorsimg .text span{font-size: 16px;color: black;margin-left: 20px;}
.filmimg .text span{font-size: 16px;color: black;margin-left: 20px;}
#content .btn{line-height: 50px;width: 100%;position: fixed;bottom: 0px;background-color: #ff5f16;text-align: center;z-index: 100;font-size: 18px;color: white;}
</style>
