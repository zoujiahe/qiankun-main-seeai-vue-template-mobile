<template>
  <div class="home">
    <h1>This is an app-main home page</h1>
    <h1>Welcome To Use SEEAI Template</h1>
    <div>
      <img
        :src="imgUrl"
        alt=""
      >
    </div>
    <br>
    <van-button
      style="margin-bottom:20px;"
      @click="loadSubPart($event)"
    >
      load app-sub part
    </van-button>
    <br>
    <van-button
      style="margin-bottom:20px;"
      @click="loadOtherSubPart($event)"
    >
      load app-other-sub part
    </van-button>
    <br>
    <van-button @click="unMountPart($event)">
      unmout app-sub&app-other-sub part
    </van-button>
  </div>
</template>

<script lang="ts">
import { defineComponent, onUnmounted, getCurrentInstance } from 'vue'
import { loadMicroApp } from 'qiankun'

import { loadScript } from '@/common/utils'
import { Shared } from '@/common/services/qiankun-share'
import { Toast } from 'vant'

const { microAppSetting } = require('../../package.json')
const microApp1Config = microAppSetting[process.env.VUE_APP_environment][0]
const microApp2Config = microAppSetting[process.env.VUE_APP_environment][1]

export default defineComponent({
  name: 'Home',
  setup () {
    const instance = getCurrentInstance()

    const unMountPart = () => {
      if (instance) {
        const micro = instance.appContext.config.globalProperties.micro
        if (micro.microApp1 && micro.microApp1.getStatus() === 'MOUNTED') {
          micro.microApp1.unmount().then(() => {
            const child1 = document.getElementById('microApp1')
            if (child1) {
              document.body.removeChild(child1)
            }
          })
        }
        if (micro.microApp2 && micro.microApp2.getStatus() === 'MOUNTED') {
          micro.microApp2.unmount().then(() => {
            const child2 = document.getElementById('microApp2')
            if (child2) {
              document.body.removeChild(child2)
            }
          })
        }
      }
    }
    const loadOtherSubPart = (e) => {
      if (instance) {
        const micro = instance.appContext.config.globalProperties.micro
        const divDom2 = document.createElement('div')
        divDom2.id = 'microApp2'
        divDom2.style.width = '80%'
        divDom2.style.height = '40%'
        divDom2.style.position = 'fixed'
        divDom2.style.zIndex = '99999'
        divDom2.style.overflow = 'auto'
        divDom2.style.left = '50%'
        divDom2.style.top = '30%'
        divDom2.style.backgroundColor = 'white'
        divDom2.style.transform = 'translate(-50%,-50%)'
        divDom2.style.boxShadow = '0 0 10px grey'
        document.body.appendChild(divDom2)
        // micro.microApp2 = loadMicroApp({
        //   name: microApp2Config.name,
        //   entry: `${microApp2Config.host}:${microApp2Config.port}/${microApp2Config.name}/#/${microApp2Config.name}/home`,
        //   container: '#microApp2',
        //   props: {
        //     name: microApp2Config.name
        //   }
        // }, {
        //   singular: false
        // })
        const shared = new Shared('two')
        shared.setFilePreviewState({
          polywayId: '',
          share: '0',
          furl: '/data/courseware/case/doc/s1hs19kvlrc2bfxe1603440271989/0n8qj0e382xdyona1603440593259.ppt',
          native: '1',
          ow365: '0',
          viewerId: shared.getUserInfo().id,
          orgCode: 'cjsd',
          showTimer: '1',
          filePreviewCallBack () {
            alert('阅读完成了')
          }
        })
        micro.microApp2 = loadMicroApp({
          name: 'xxx2',
          entry: process.env.VUE_APP_commonViewer,
          container: '#microApp2',
          props: {
            name: 'xxx2',
            shared
          }
        }, {
          sandbox: {
            experimentalStyleIsolation: true
          }
        })
      }
    }
    const loadSubPart = (e) => {
      if (instance) {
        const micro = instance.appContext.config.globalProperties.micro
        const divDom1 = document.createElement('microApp1')
        divDom1.id = 'microApp1'
        divDom1.style.width = '80%'
        divDom1.style.height = '40%'
        divDom1.style.position = 'fixed'
        divDom1.style.zIndex = '99999'
        divDom1.style.overflow = 'auto'
        divDom1.style.left = '50%'
        divDom1.style.top = '30%'
        divDom1.style.backgroundColor = 'white'
        divDom1.style.transform = 'translate(-50%,-50%)'
        divDom1.style.boxShadow = '0 0 10px grey'
        document.body.appendChild(divDom1)
        // micro.microApp1 = loadMicroApp({
        //   name: microApp1Config.name,
        //   entry: `${microApp1Config.host}:${microApp1Config.port}/${microApp1Config.name}/#/${microApp1Config.name}/home`,
        //   container: '#microApp1',
        //   props: {
        //     name: microApp1Config.name
        //   }
        // }, {
        //   singular: false
        // })
        loadScript('//player.polyv.net/resp/vod-player/latest/player.js').then(flag => {
          if (flag) {
            const shared = new Shared('one')
            shared.setFilePreviewState({
              polywayId: 'a647f95e6e8a407915e0d24d6b5adc48_a',
              share: '0',
              furl: '',
              native: '1',
              ow365: '0',
              viewerId: shared.getUserInfo().id,
              orgCode: 'cjsd',
              showTimer: '1',
              filePreviewCallBack () {
                alert('阅读完成了')
              }
            })
            micro.microApp1 = loadMicroApp({
              name: 'xxx',
              entry: process.env.VUE_APP_commonViewer,
              container: '#microApp1',
              props: {
                name: 'xxx',
                shared
              }
            }, {
              sandbox: {
                experimentalStyleIsolation: true
              }
            })
          }
        }).catch(() => {
          Toast.fail({
            message: '网络异常，请稍后再试！',
            icon: 'close'
          })
        })
      }
    }

    return {
      loadSubPart,
      loadOtherSubPart,
      unMountPart,
      imgUrl: require('@images/logo.png')
    }
  }
})
</script>
