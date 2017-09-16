<template>
  <div class="side-nav" :class="sidenavClass">
    <div id="data_mr_l3">
      <ul>
        <toggle-tab v-for="(menu, index) in menus" :key="index">
          <span class="fa fa-caret-down has-tip  tip-right radius" title="Dashboard"  data-tooltip="" aria-haspopup="true" data-options="show_on:large"></span><span>{{menu.name}}</span>
          <span class="fa fa-cog has-tip  tip-right radius"  title="自定义Dashboard" data-reveal-id="myModal"  data-tooltip aria-haspopup="true" data-options="disable_for_touch:true"></span>
          <ul slot="content">
            <li v-for="item in menu.menus" :key="item.path" :class="{ curr_target: matchPath(item.path) }">
              <a v-bind:to="item.path" data-href="media-manage.html"><span class="fa fa-sitemap has-tip  tip-right radius" title="Dashbord1" data-tooltip aria-haspopup="true" data-options="show_on:large"></span><span>{{item.name}}</span></a>
            </li>
          </ul>
        </toggle-tab>
      </ul>
    </div>
    <div class="overlay"></div>
  </div>
</template>

<script>
import ToggleTab from 'components/ToggleTab'

export default {
  components: {
    ToggleTab
  },
  computed: {
    showSidebar () {
      return this.$store.state.showSidebar
    },
    showSidebarInSmall () {
      return this.$store.state.showSidebarInSmall
    },
    sidenavClass () {
      return {
        toggle: !this.showSidebar,
        open: this.showSidebarInSmall
      }
    }
  },
  data () {
    return {
      menus: [
        {
          name: '平台管理',
          menus: [
            {
              name: '内容管理',
              path: '/dept'
            }
          ]
        }
      ],
      itemShow: true
    }
  },
  created () {
  },
  methods: {
    handleclose () {
    },
    handleopen () {
    }
  }
}
</script>

<style>
</style>
