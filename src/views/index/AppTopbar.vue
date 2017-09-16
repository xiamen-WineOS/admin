<template>
  <div class="header">
    <div class="clearfix" id="data_header">
      <div class="block-small-only" data-set="top-search"></div>
      <a id="smallSidebar" class="smallSidebar"
        @click="sidebarSmallTogger"
        :class="showSidebarInSmall ? 'open' : ''"
      >
        <span></span>
        <span></span>
        <span></span>
      </a>
      <a class="logo" href="#" title="统一管理平台" :class="showSidebar ? '' : 'toggle'">
        <span>统一管理平台</span>
      </a>
      <a class="sidebar-toggle" @click="sidebarTogger"><span class="fa fa-dedent" :class="showSidebar ? 'fa-dedent' : 'fa-indent'"></span></a>
      <div class="float-right right navbar-top">
        <ul class="clearfix">
          <li class="hide-small-only" data-set="top-search">
            <div class="top-search" id="top-search">
              <a class="admin-search" v-show="searchShow" @click="searchToggle"><span class="fa fa-search"></span></a>
              <label v-show="!searchShow"><input type="text" placeholder="Search..." /><span class="sure-close" @click="searchToggle">×</span></label>
              <div class="sug-complete">
                <ul>
                  <li><a>根据民航局</a></li>
                  <li><a>防撒旦法</a></li>
                  <li><a>这个是欧赔</a></li>
                </ul>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <a class="subMenu-bar hide">
        <span></span>
        <span></span>
        <span></span>
      </a>
      <div class="load-line" style="width: 0;"><span></span></div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  components: {
  },
  computed: {
    
  },
  data () {
    return {
      logoClass: {
        toggle: !this.showSidebar
      },
      searchShow: true,
      userInfoShow: false
    }
  },
  methods: {
    // 映射 this.sidebarTogger() 到 this.$store.commit(SIDEBAR_TOGGER)
    ...mapMutations({
    }),
    searchToggle () {
      this.searchShow = !this.searchShow
    }
  }
}
</script>

<style>
</style>
