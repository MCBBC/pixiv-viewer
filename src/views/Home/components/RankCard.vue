<template>
  <div class="rank-card">
    <van-cell class="cell" :border="false" is-link to="/rank/daily">
      <template #title>
        <Icon class="icon crown" name="crown"></Icon>
        <span class="title">排行榜</span>
      </template>
    </van-cell>
    <div class="card-box">
      <van-swipe class="swipe-wrap" :loop="false" :show-indicators="false" :width="300">
        <van-swipe-item class="swipe-item" v-for="art in artList.slice(0, 6)" :key="art.id">
          <ImageCard mode="meta" :artwork="art" @click-card="toArtwork($event)" />
        </van-swipe-item>
        <van-swipe-item class="swipe-item more" @click.stop="$router.push('/rank/weekly')">
          <ImageSlide :images="slides">
            <div class="link">
              <Icon name="more" scale="20"></Icon>
              <div>查看更多</div>
            </div>
          </ImageSlide>
          <!-- <router-link class="rank" :to="{name: 'Rank'}">
            查看更多
            <van-icon name="arrow" />
          </router-link>-->
        </van-swipe-item>
      </van-swipe>
    </div>
  </div>
</template>

<script>
import { Cell, Swipe, SwipeItem, Icon } from "vant";
import ImageCard from "@/components/ImageCard";
import ImageSlide from "@/components/ImageSlide";
import api from "@/api";
export default {
  name: "RankCard",
  data() {
    return {
      artList: []
    };
  },
  computed: {
    slides() {
      let artList = this.artList.slice(6, 11);
      return artList.map(art => {
        return {
          title: art.title,
          src: art.images[0].m
        };
      });
    }
  },
  methods: {
    async getRankList() {
      let res = await api.getRankList("week");
      if (res.status === 0) {
        this.artList = res.data;
      } else {
        this.$toast({
          message: res.msg,
          icon: require("@/svg/error.svg")
        });
      }
    },
    toArtwork(id) {
      this.$router.push({
        name: "Artwork",
        params: { id, list: this.artList }
      });
    }
  },
  mounted() {
    this.getRankList();
  },
  components: {
    [Cell.name]: Cell,
    [Swipe.name]: Swipe,
    [SwipeItem.name]: SwipeItem,
    [Icon.name]: Icon,
    ImageCard,
    ImageSlide
  }
};
</script>

<style lang="stylus" scoped>
.rank-card {
  padding: 0 14px;
  margin: 24px 0;

  .card-box {
    // padding: 0 12px;
    height: 365px;

    .swipe-wrap {
      height: 100%;
      border-radius: 20px;
      overflow: hidden;

      .swipe-item {
        &:last-child {
          .image-card {
            margin-right: 0;
          }
        }

        .image-card {
          // width: 50vw;
          font-size: 0;
          float: left;
          margin-right: 12px;
          border: 1px solid #ebebeb;
          border-radius: 18px;
          box-sizing: border-box;
          width: calc(100% - 12px);
          height: 100% !important;
        }

        .image-slide {
          border: 1px solid #ebebeb;
          border-radius: 18px;
          box-sizing: border-box;

          .link {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: #efefef;

            &::before {
              content: '';
              position: absolute;
              top: 0;
              left: 0;
              right: 0;
              bottom: 0;
              background: rgba(#000, 0.6);
            }

            svg {
              position: absolute;
              top: 50%;
              left: 50%;
              transform: translate(-50%, -55%);
              font-size: 20em;
            }

            div {
              position: absolute;
              left: 50%;
              top: 50%;
              transform: translate(-50%, 80%);
              font-size: 34px;
              text-align: center;
              white-space: nowrap;
            }
          }
        }

        &.more {
          .rank {
            display: flex;
            height: 100%;
            justify-content: center;
            align-items: center;
          }
        }
      }
    }
  }
}
</style>