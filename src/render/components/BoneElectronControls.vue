<template>
  <div class="bone-controls-container">
    <div class="bone-controls-item bone-controls-minimize iconfont" @click="minimizeWindow">&#xe7bf;</div>
    <div
      class="bone-controls-item bone-controls-maximize iconfont"
      @click="maximizeWindow"
    >{{ maximizeIcon }}</div>
    <div class="bone-controls-item bone-controls-close iconfont" @click="closeWindow">&#xe7b4;</div>
  </div>
</template>

<script>
const { getCurrentWindow } = window.require("electron").remote;
const ipcRenderer = window.require("electron").ipcRenderer;

export default {
  name: "BoneElectronControls",
  components: {},
  setup() {
    const win = getCurrentWindow();
    return { win };
  },
  data() {
    return {
      maximizeIcon: "\ue7c1",
    };
  },
  methods: {
    minimizeWindow() {
      this.win.minimize();
      // ipcRenderer.send("hide-window");
    },
    maximizeWindow() {
      if (this.maximizeIcon === "\ue7c1") {
        this.win.maximize();
        this.win.resizable = false;
        this.maximizeIcon = "\ue7c0";
      } else {
        this.win.unmaximize();
        this.win.resizable = true;
        this.maximizeIcon = "\ue7c1";
      }
    },
    closeWindow() {
      this.win.close();
    },
  },
  computed: {},
  mounted() {
    ipcRenderer.on("main-window-max", () => {
      this.maximizeIcon = "\ue7c0";
    });
    ipcRenderer.on("main-window-unmax", () => {
      this.maximizeIcon = "\ue7c1";
    });
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/style/variable.scss";
.bone-controls-container {
  margin: 0;
  height: 30px;
  display: flex;
  flex-direction: row;
  font-weight: 900;
  color: $--color-text-primary;

  .bone-controls-item {
    height: 100%;
    width: 46px;
    text-align: center;
    line-height: 30px;
    user-select: none;
    font-size: 10px;
    transition-duration: 100ms;
  }

  .bone-controls-item:hover {
    background-color: #8884;
  }

  .bone-controls-close:hover {
    background-color: #f33;
  }
}
</style>