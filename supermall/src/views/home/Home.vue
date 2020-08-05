<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <!-- <swiper>
      <swiper-item v-for="item in banners" :key="item.acm">
        <a :href="item.link">
          <img :src="item.image" alt="" >
        </a>
      </swiper-item>
    </swiper> -->
    <home-swiper :banners="banners" />
    <recommend-view :recommends="recommends" />
    <feature-view></feature-view>
    <tab-control :titles="['流行','新款','精选']"></tab-control>
    <div>内容1</div>
    <div>内容2</div>
    <div>内容3</div>
    <div>内容4</div>
    <div>内容5</div>
    <div>内容6</div>
    <div>内容7</div>
    <div>内容8</div>
    <div>内容9</div>
    <div>内容10</div>
    <div>内容11</div>
    <div>内容12</div>
    <div>内容13</div>
    <div>内容14</div>
    <div>内容15</div>
    <div>内容16</div>
    <div>内容17</div>
    <div>内容18</div>
    <div>内容19</div>
    <div>内容20</div>
    <div>内容21</div>
    <div>内容22</div>
    <div>内容23</div>
    <div>内容24</div>
    <div>内容25</div>
    <div>内容26</div>
    <div>内容27</div>
    <div>内容28</div>
    <div>内容29</div>
    <div>内容30</div>
    <div>内容31</div>
    <div>内容32</div>
    <div>内容33</div>
    <div>内容34</div>
    <div>内容35</div>
    <div>内容36</div>
    <div>内容37</div>
    <div>内容38</div>
    <div>内容39</div>
    <div>内容40</div>
    <div>内容41</div>
    <div>内容42</div>
    <div>内容43</div>
    <div>内容44</div>
    <div>内容45</div>
    <div>内容46</div>
    <div>内容47</div>
    <div>内容48</div>
    <div>内容49</div>
    <div>内容50</div>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from '../../components/common/navbar/NavBar';
import TabControl from '../../components/content/tabControl/TabControl'

import { getHomeMultidata } from "../../network/home";

export default {
  name: "Home",
  components: {
    HomeSwiper,
    FeatureView,
    RecommendView,
    NavBar,
    TabControl,
  },
  data () {
    return {
      banners: [],
      recommends: []
    }
  },
  created () {
    // 1.请求多个数据
    getHomeMultidata().then(res => {
      // this.result = res;
      this.banners = res.data.banner.list;
      this.recommends = res.data.recommend.list;
    })
  }
}
</script>

<style scoped>
#home{
  position: sticky;
  top:0;
  left: 0;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>
