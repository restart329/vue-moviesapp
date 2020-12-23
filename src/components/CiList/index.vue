<template>
    <div class="cinema_body">
      <Loading v-if="isloading"/>
      <Scroller v-else>
                    <ul>
                        <li v-for="data in cinemalist" :key="data.cinemaId" @click="handleChangePage(data)">
                            <div class="cinemanames">
                                <span class="cinemaname">{{data.name}}</span>
                                <span class="q"><span class="price">{{data.lowPrice/100}}</span> 元起</span>
                            </div>
                            <div class="address">
                                <span class="address1">{{data.address}}</span>
                                <span class="distance">{{data.Distance | distancefilter}}km</span>
                            </div>
                            <div class="card">
                                <div>小吃</div>
                                <div>折扣卡</div>
                            </div>
                        </li>
                    </ul>
      </Scroller>
                </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
Vue.filter('distancefilter', function (data) {
  return data.toFixed(2)
})
export default {
  name: 'CiList',
  data () {
    return {
      cinemalist: [],
      isloading: true,
      prevCityId: -1
    }
  },
  activated () {
    var cityId = this.$store.state.city.id
    if (this.prevCityId === cityId) {
      return
    }
    this.isloading = true
    axios({
      url: ` https://m.maizuo.com/gateway?cityId=${cityId}`,
      headers: {
        'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"1605253643172292613079041","bc":"110100"}',
        'X-Host': 'mall.film-ticket.cinema.list'
      }
    }).then(res => {
      var msg = res.data.msg
      if (msg === 'ok') {
        this.cinemalist = res.data.data.cinemas
        this.isloading = false
        this.prevCityId = cityId
      }
    })
  }
}
</script>

<style scoped>
#content .cinema_body{ flex:1; overflow:auto;}
.cinema_body ul{ padding:20px;}
.cinema_body li{  border-bottom:1px solid #e6e6e6; margin-bottom: 20px;}
.cinema_body div{ margin-bottom: 10px;}
.cinema_body .q{ font-size: 11px; color:#f03d37;}
.cinema_body .price{ font-size: 18px;}
.cinema_body .address{ font-size: 13px; color:#666;}
.cinema_body .address .address1{display: inline-block; width: 150px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;}
.cinema_body .cinemaname{display: inline-block; width: 200px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;}
.cinema_body .cinemanames{position: relative;}
.cinema_body .cinemanames .q{position: absolute;right: 0px;}
.cinema_body .address span:nth-of-type(2){ float:right; }
.cinema_body .card{ display: flex;}
.cinema_body .card div{ padding: 0 3px; height: 15px; line-height: 15px; border-radius: 2px; color: #f90; border: 1px solid #f90; font-size: 13px; margin-right: 5px;}
.cinema_body .card div.or{ color: #f90; border: 1px solid #f90;}
.cinema_body .card div.bl{ color: #589daf; border: 1px solid #589daf;}
</style>
