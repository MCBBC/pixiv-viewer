<template>
  <div id="app">
    <Preload />
    <router-view />
    <transition name="fade">
      <div class="back-top" @click="toTop" v-show="isTopShow">
        <Icon class="icon-top" name="top"></Icon>
      </div>
    </transition>
  </div>
</template>

<script>
import Preload from "@/components/Preload";
import { mapState, mapActions } from "vuex";

export default {
  name: "App",
  data() {
    return {
      isTopShow: false,
    };
  },
  computed: {
    ...mapState(["SETTING"]),
  },
  methods: {
    ...mapActions(["saveSETTING"]),
    toTop() {
      document.documentElement.scrollTo({ top: 0, behavior: "smooth" });
    },
    scrollHandler() {
      if (document.documentElement.scrollTop > 1200) {
        this.isTopShow = true;
      } else {
        this.isTopShow = false;
      }
    },
  },
  beforeMount() {
    const { r18 } = this.$route.query;

    if (+r18 === 1) {
      this.saveSETTING({
        ...this.SETTING,
        r18: true,
      });
    }
  },
  mounted() {
    window.addEventListener("scroll", this.scrollHandler);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.scrollHandler);
  },
  components: {
    Preload,
  },
};
</script>

<style lang="stylus" scoped>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 750px;
  margin: 0 auto;

  &.show-nav {
    .back-top {
      bottom: 130px;
      bottom: calc(130px + env(safe-area-inset-bottom));
    }
  }

  .back-top {
    position: fixed;
    right: 40px;
    bottom: 40px;
    bottom: calc(40px + env(safe-area-inset-bottom));
    cursor: pointer;

    .icon-top {
      width: 100px;
      height: 100px;
    }
  }
}

@media screen and (min-width: 768px) {
  #app {
    max-width: 1200px;
  }
}

@media screen and (min-width: 1700px) {
  #app {
    max-width: 1600px;
  }
}
</style>
