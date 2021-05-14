<template>
  <header class="header">
    <!-- <div class="iconfont" v-if="true">&#xe7c2;</div> -->
    <img src="../assets/底部芯片.png" class="header__logo" alt="logo" />
    <div class="header__title">{{ title }}</div>
    <div>
      <!-- <div>
        <span>文件</span>
        <BoneMenu :menu-template="items" />
      </div>-->
    </div>
    <div class="draggable"></div>
    <BoneElectronControls />
  </header>
</template>

<script lang="ts">
import { onMounted, ref } from "vue";
import BoneElectronControls from "../components/BoneElectronControls.vue";
// import BoneMenu from "../components/BoneMenu.vue";

export default {
  name: "TheHeader",
  components: {
    BoneElectronControls,
    // BoneMenu
  },
  props: {
    title: {
      type: String,
      default: null
    },
  },
  setup(props) {
    const title = ref(props.title)
    onMounted(() => {

      if (props.title == null) {
        title.value = document.title
      }
    });
    return {
      title, items: [{
        label: '配置接口',
        click: () => {
        }
      }, {
        label: '配置接口',
        click: () => {
        }
      }, {
        label: '菜单2',
        submenu: [
          { label: '菜单2子项1' },
          { label: '菜单2子项2' },
          { label: '菜单2子项3' },
          { label: '菜单2子项4' }
        ]
      }
      ]
    }
  },
};
</script>
<style lang="scss" scoped>
@import "../assets/style/variable.scss";
.draggable {
  flex-grow: 1;
  -webkit-app-region: drag;
}
.header {
  height: 30px;
  width: 100%;
  z-index: 100;
  line-height: 30px;
  // background-color: $background-color-primary;
  color: $--color-text-primary;
  backdrop-filter: blur(5px);

  display: flex;
  justify-content: space-between;
  align-items: stretch;

  .header__title {
    position: absolute;
    width: 100%;
    z-index: -1;
    text-align: center;
  }
  .header__logo {
    height: 30px;
    width: 30px;
    margin: auto 5px;
  }
}
</style>
