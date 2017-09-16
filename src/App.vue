<template>
  <div>
    <div id="app" class="wrapper" :class="fullContent ? 'full-content' : ''">
      <app-topbar></app-topbar>
      <!--<div class="main" role="main" aria-label="主体区块">
        <app-sidebar></app-sidebar>
        <div class="frame_content" :class="showSidebar ? '' : 'toggle'">
          <transition @enter="onEnter" @leave="onLeave">
            <router-view></router-view>
          </transition>
          <div class="overlay" :class="showSidebarInSmall ? 'sidebar-overlay-open' : ''"></div>
        </div>
      </div>-->
    </div>
  </div>
</template>

<script>
import AppTopbar from './views/index/AppTopbar'
import AppSidebar from './views/index/AppSidebar'

export default {
  components: {
    AppTopbar,
    AppSidebar
  },
  computed: {
  },
  data () {
    return {
      loading: false,
      showFullContent: true
    }
  },
  created () {
  },
  methods: {
    onEnter () {
      this.loading = false
    },
    onLeave () {
      this.loading = true
    }
  }
}
</script>

<style>
</style>
