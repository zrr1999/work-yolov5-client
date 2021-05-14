<template>
  <TheHeader></TheHeader>
  <!-- <div class="title">YOLOv5 图像理解与计算机视觉</div> -->

  <div class="container">
    <div class="main-body">
      <span
        :class="Math.random() < 0.3 ? 'animate1' : Math.random() > 0.6 ? 'animate2' : 'animate3'"
        :style="{ opacity: Math.random() }"
        v-for="i in 100"
      ></span>
    </div>
    <div class="img-container">
      <img class="yolo-result" :src="src" fit="contain" />
      <div class="labels">
        <BoneButton width="80px" type="icon" v-for="(item, index) in state" :key="index">{{ item }}</BoneButton>
      </div>
    </div>
    <div class="btn-container">
      <BoneButton v-show="!running" @click="funcRun" width="72px" class="btn">&#xe7db; 开始</BoneButton>
      <BoneButton v-show="running" @click="funcStop" width="72px" class="btn">终止</BoneButton>
    </div>
  </div>
</template>

<script lang="ts">
import TheHeader from "../components/TheHeader.vue";
import BoneButton from "../components/BoneButton.vue";
import { reactive, computed, ref } from "vue";
const version = require("process").versions.electron;
export default {
  name: "Index",
  components: {
    TheHeader,
    BoneButton
  },
  setup() {
    let running = ref(false);
    const src = ref("../assets/default.jpg");
    const state = ref([]);

    let ws: WebSocket;


    function funcRun() {
      running.value = true
      ws = new WebSocket("ws://127.0.0.1:6954/user");

      ws.onmessage = function(evt) {
        const data = JSON.parse(evt.data)

        src.value = "data:image/png;base64," + data.src
        state.value = data.pred
        ws.send("success")
      };
      ws.onclose = function() {
        // 关闭 websocket
        console.log("连接已关闭...");

      };
    }
    function funcStop() {
      running.value = false
      ws.send("close")
      ws.close()
    }

    return {
      src,
      funcRun, funcStop, running,
      state
    };
  },
};
</script>

<style lang="scss" scoped>
.main-body {
  position: absolute;
  background-position: center;
}
.main-body span {
  float: left;
  width: 70px;
  height: 70px;
  margin: 5px;
}
.animate1 {
  animation: shineSquare1 8s linear infinite;
}
.animate2 {
  animation: shineSquare2 8s linear infinite;
}
.animate3 {
  animation: shineSquare3 8s linear infinite;
}
@keyframes shineSquare1 {
  from {
    background: #0990cb;
  }
  20% {
    background: #123496;
  }
  40% {
    background: #003366;
  }
  60% {
    background: #2345d7;
  }
  80% {
    background: #044d9b;
  }
  to {
    background: #003366;
  }
}
@keyframes shineSquare2 {
  from {
    background: #003366;
  }
  20% {
    background: #6666ff;
  }
  40% {
    background: #003366;
  }
  60% {
    background: #0990cb;
  }
  80% {
    background: #044d9b;
  }
  to {
    background: #003366;
  }
}
@keyframes shineSquare3 {
  from {
    background: #003366;
  }
  20% {
    background: #0990cb;
  }
  40% {
    background: #003366;
  }
  60% {
    background: #044d9b;
  }
  80% {
    background: #2277dd;
  }
  to {
    background: #003366;
  }
}

.bg-img {
  position: absolute;
}
.container {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;
  background-color: #224499;
  .btn-container {
    width: 94px;
    margin: auto;
    z-index: 1;
    .bone-button {
      background-color: #4990cb;
      &:hover {
        background-color: #444466;
      }
    }
  }
  .img-container {
    display: flex;
    margin: auto;
    z-index: 1;

    // height: 72px;
    .labels {
      display: flex;
      flex-direction: column;
      margin: 10px;
    }
    .yolo-result {
      // height: 72px;
      width: 512px;
    }
  }
}
</style>

