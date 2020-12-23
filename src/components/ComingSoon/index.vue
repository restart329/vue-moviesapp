<template>
    <div class="movie_body">
      <Loading v-if="isloading"/>
      <Scroller v-else>
                    <ul>
                        <li v-for="data in cominglist" :key="data.filmId" @click="handleToDetail(data.filmId)">
                            <div class="pic_show"><img :src="data.poster"></div>
                                <div class="info_list">
                                    <h2 @click="handleToDetail(data.filmId)">{{ data.name }}
                                    <span>{{data.filmType.name}}</span>
                                    </h2>
                                    <p>观众评分： <span class="grade">{{ data.grade }}</span></p>
                                    <p>主演: {{ data.actors | actorsfilter}}</p>
                                    <p>简介：{{ data.synopsis }}</p>
                                </div>
                                <div class="btn_pre">
                                    预售
                            </div>
                        </li>
                    </ul>
      </Scroller>
                </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
Vue.filter('actorsfilter', function (data) {
  var newlist = data.map(item => item.name)
  return newlist.join(' ')
})
export default {
  name: 'ComingSoon',
  data () {
    return {
      cominglist: [],
      isloading: true,
      prevCityId: -1
    }
  },
  methods: {
    handleToDetail (filmId) {
      this.$router.push('/movie/detail/2/' + filmId)
    }
  },
  activated () {
    var cityId = this.$store.state.city.id
    if (this.prevCityId === cityId) {
      return
    }
    console.log(123)
    this.isloading = true
    axios({
      url: 'https://m.maizuo.com/gateway?cityId=110100&pageNum=1&pageSize=10&type=2&k=6316353',
      headers: {
        'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"1605253643172292613079041","bc":"110100"}',
        'X-Host': 'mall.film-ticket.film.list'
      }
    }).then(res => {
      console.log(res.data)
      this.cominglist = res.data.data.films
      this.isloading = false
      this.prevCityId = cityId
    })
  }
}
</script>

<style scoped>
#content .movie_body{ flex:1; overflow:auto;}
.movie_body ul{ margin:0 12px; overflow: hidden;}
.movie_body ul li{ margin-top:12px; display: flex; align-items:center; border-bottom: 1px #e6e6e6 solid; padding-bottom: 10px;}
.movie_body .pic_show{ width:64px; height: 90px;}
.movie_body .pic_show img{ width:100%;}
.movie_body .info_list { margin-left: 10px; flex:1; position: relative;}
.movie_body .info_list h2{ font-size: 17px; line-height: 24px; width:150px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;position: relative;}
.movie_body .info_list h2 span{
    display: inline-block;
    font-size: 9px;
    padding:0px 2px ;
    width: 18px;
    line-height: 14px;
    background-color: rgb(200, 211, 210);
    color: white;
    border-radius: 2px;
    margin-left: 4px;
    position: absolute;
    top: 6px;
}
.movie_body .info_list p{ font-size: 13px; color:#666; line-height: 22px; width:200px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
.movie_body .info_list .grade{ font-weight: 700; color: #faaf00; font-size: 15px;}
.movie_body .info_list img{ width:50px; position: absolute; right:10px; top: 5px;}
.movie_body .btn_mall , .movie_body .btn_pre{ width:47px; height:27px; line-height: 28px; text-align: center; background-color: #f03d37; color: #fff; border-radius: 4px; font-size: 12px; cursor: pointer;}
.movie_body .btn_pre{ background-color: #3c9fe6;}
</style>
