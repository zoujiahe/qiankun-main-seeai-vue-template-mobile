<template>
  <div style='height:100%;overflow: hidden;'>
    <router-view/>
    <div id="frame"
         v-if="(route?.path === '/' || !(route?.path === '/login' || route?.path.indexOf('/micro-student-main') === 0)) && microApp"></div>
  </div>
</template>
<script lang="ts">
import { defineComponent, getCurrentInstance } from 'vue'
import { win } from '@/common/base'
import { RouteLocationNormalizedLoaded, useRoute } from 'vue-router'
import { ToolsUtil } from '@/common/utils'

declare const window: win
export default defineComponent({
  name: 'App',
  setup () {
    ToolsUtil.initTheme()
    window.__platform__ = 'platform-scholar'
    const instance = getCurrentInstance()
    const microApp = instance?.appContext.config.globalProperties.microApp
    const route: RouteLocationNormalizedLoaded = useRoute()
    return {
      route,
      microApp
    }
  }
})

</script>
<style lang="scss">
  html,body {
    height:100%;
  }
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100%;
  }

  #frame {
    height: 100%;

    > div {
      height: 100%;
    }
  }

  #nav {
    padding: 30px;

    a {
      font-weight: bold;
      color: #2c3e50;

      &.router-link-exact-active {
        color: #42b983;
      }
    }
  }
</style>
