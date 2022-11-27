<template>
  <div class="text-center">
    <div class="led text-red-500">
      <span v-for="(item, index) in timeSplit" :key="index" class="w-[50px] inline-block">{{ item }}</span>
    </div>
    
    
    <button class="mx-3 text-blue-500 border border-blue-500 w-[54px] h-[54px] inline-flex items-center justify-center rounded-1/2" @click="reset">
      <Icon icon="fluent:arrow-reset-24-filled" width="28" height="28" />
    </button>
    <button class="mx-3 text-pink-500 border border-pink-500 w-[54px] h-[54px] inline-flex items-center justify-center rounded-1/2" @click="playing ? pause() : play()">
      <Icon v-if="!playing" icon="fluent:play-28-filled" width="28" height="28" />
      <Icon v-else icon="fluent:pause-24-filled" width="28" height="28" />
    </button>
  </div>
</template>

<script lang="ts" setup>
  import { Icon } from "@iconify/vue"
  import { ref, watch, computed } from "vue"
  
  const playing = ref(false)
  const time = ref(0)
  const timeSplit = computed(() => {
    const arr = [];
    let miliseconds = time.value
    
      const seconds = ~~(miliseconds/1e3)
      miliseconds %= 1e3
      // 60fps
      
      if (seconds < 10) arr.push("0")
      arr.push(...(seconds.toString()).split(""))
      
      miliseconds = Math.round(miliseconds / 10)
      
      arr.push(":")
      
      if (miliseconds < 10) arr.push("0")
      arr.push(...miliseconds.toString().split(""))
      
      if (arr.join("") === "10:00") return "09:99".split("")
      
      return arr; 
  })
  
  let requestId: number | null = null
  watch(playing, playing => {
    if (playing) {
      let last = performance.now()
      const handler = () => {
        const now = performance.now()
        time.value += (now - last)
         last = now
        requestId = requestAnimationFrame(handler)
      }
      handler()
      return
    }
    
    if (requestId) cancelAnimationFrame(requestId)
  })
  
  function reset() {
    playing.value = false
    time.value = 0
  }
  function play() {
    playing.value =  true
    
    
  }
  function pause() {
    playing.value = false
  }
  
  
  // get now 
  </script>

<style lang="scss" scoped>
  @font-face {
    font-family: "Digital-7";
    src: url("src/assets/digital_7/digital-7.ttf");
  }
  .led {
    font-family: "Digital-7";
    font-size: 100px;
    line-height: 1.5;
  }
  </style>
  