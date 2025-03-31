<script setup>
import zss from '../assets/zss.txt?raw'
import {TinyLayout, TinyRow, TinyCol, TinyButton, TinyDivider} from '@opentiny/vue'
import {iconShare, iconConmentRefresh, iconLoadingShadow} from '@opentiny/vue-icon'

import {ref} from "vue";

const zssList = zss.split('\n').filter(s => !s || s.trim().length !== 0)
const zssStr = ref(randomZss())
const reloading = ref(false)

const LoadingIcon = iconLoadingShadow()
const ReplaceIcon = iconConmentRefresh()
const GitHubIcon = iconShare()

function randomZss() {
  return zssList[Math.floor(Math.random() * zssList.length)]
}

async function handleClick() {
  reloading.value = true
  zssStr.value = randomZss()
}

function afterEnter() {
  reloading.value = false
}

</script>

<template>
  <div>
    <tiny-layout class="zss-layout">
      <tiny-row class="head-row" :gutter="10" :align="'middle'" :flex="true">
        <tiny-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
        </tiny-col>
      </tiny-row>
      <tiny-row class="zss-row" :gutter="10" :align="'middle'" :flex="true">
        <tiny-col class="col" :xs="2" :sm="2" :md="2" :lg="3" :xl="1">
        </tiny-col>
        <tiny-col class="col" :xs="8" :sm="8" :md="8" :lg="6" :xl="10">
          <p class="zss-tip">你的ZSS是：</p>
          <Transition name="roll" mode="out-in" :on-after-enter="afterEnter">
            <h1 class="zss-h1" :key="zssStr" v-text="zssStr"></h1>
          </Transition>
          <tiny-layout>
            <tiny-row>
              <tiny-col>
                <tiny-button
                    type="info"
                    :reset-time="0"
                    size="large"
                    :loading="reloading"
                    :icon="ReplaceIcon"
                    @click="handleClick">
                  随机ZSS
                </tiny-button>

                <tiny-divider direction="vertical"></tiny-divider>

                <tiny-button
                    ghost
                    type="default"
                    :reset-time="0"
                    size="large"
                    :icon="GitHubIcon"
                    @click=""> 贡献ZSS
                </tiny-button>
              </tiny-col>
            </tiny-row>
          </tiny-layout>

        </tiny-col>
        <tiny-col class="col" :xs="2" :sm="2" :md="2" :lg="3" :xl="1">
        </tiny-col>
      </tiny-row>
    </tiny-layout>
  </div>
</template>

<style scoped>
/* 新增动画效果 */
.roll-enter-active {
  animation: roll-in 0.21s cubic-bezier(0.22, 0.61, 0.36, 1);
}
.roll-leave-active {
  animation: roll-out 0.24s cubic-bezier(0.55, 0.06, 0.68, 0.19);
}

@keyframes roll-in {
  from {
    transform: translateY(120%) rotateX(90deg);
    opacity: 0;
  }
  to {
    transform: translateY(0) rotateX(0);
    opacity: 1;
  }
}

@keyframes roll-out {
  from {
    transform: translateY(0) rotateX(0);
    opacity: 1;
  }
  to {
    transform: translateY(-120%) rotateX(-90deg);
    opacity: 0;
  }
}

/* 保证动画元素定位 */
.zss-h1 {
  display: inline-block;
  position: relative;
  /* 原有样式保持不变 */
}

/* ---- */

.head-row {
  min-height: 120px;
  height: 25vh;
}

.zss-row {
  text-align: center;
}

.zss-tip {
  font-size: 1.2em;
}

.zss-h1 {
  font-size: 5.2em;
  line-height: 1.5;
}
</style>
