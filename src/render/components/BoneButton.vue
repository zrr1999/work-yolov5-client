<template>
  <button
    class="bone-button iconfont"
    type="button"
    :class="boneButtonState.buttonType"
    :style="boneButtonState.buttonStyle"
    :disabled="disabled"
  >
    <span><slot /></span>
  </button>
</template>

<script>
import { computed, reactive } from "vue";

export default {
  name: "BoneButton",
  props: {
    type: {
      type: String,
      default: "common",
      validator: function (value) {
        return ["common", "primary", "icon"].includes(value);
      },
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    height: {
      type: String,
      default: null,
    },
    width: {
      type: String,
      default: null,
    },
  },
  setup(props) {
    const boneButtonState = reactive({
      buttonStyle: {
        width: props.width,
        height: props.height,
        lineHeight: props.height,
      },
      buttonType: computed(() => {
        return "bone-button--" + props.type;
      }),
    });
    return { boneButtonState };
  },
};
</script>

<style lang="scss">
@import "../assets/style/variable.scss";

.bone-button {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  text-align: center;
  box-sizing: border-box;
  outline: 0;
  margin: 0;
  font-weight: 500;
  font-size: 14px;
  user-select: none;

  padding: 0 10px;
  height: 48px;
  line-height: 48px;

  transition-duration: 100ms;
  &:active {
    transition-duration: 30ms;
  }
}
.bone-button--icon {
  cursor: pointer;

  color: $--color-text-regular;
  background-color: #0000;

  border-radius: 5px;
  &:hover:enabled {
    color: $--color-primary;
  }
  &:active:enabled {
    color: $--color-primary-light-2;
  }
  &:disabled {
    cursor: default;
    color: $--color-primary-light-9;
  }
}

.bone-button--common {
  cursor: default;
  box-sizing: content-box;

  background-color: $--background-color-base;
  color: $--color-text-regular;

  margin: auto 10px;
  border-radius: 30px;
  border: 2px solid $--color-gray;

  &:hover:enabled {
    background-color: mix($--color-black, $--background-color-base, 50%);
    color: $--color-primary;
    border: 2px solid $--color-primary-light-7;
    border-radius: 10px;
  }
  &:active:enabled {
    background-color: mix($--color-black, $--background-color-base, 60%);
    border: 2px solid $--color-primary;
    border-radius: 5px;
  }
  &:disabled {
    border-radius: 5px;
    box-shadow: none;
    background-color: $--color-primary-light-5;
  }
}
.bone-button--primary {
  cursor: default;
  background-color: $--color-primary;
  color: $--background-color-base;
  font-weight: bold;

  margin: auto 10px;
  border-radius: 30px;
  box-shadow: 0 0 5px 5px $--background-color-base;

  &:hover:enabled {
    background-color: $--color-primary-light-2;
    border-radius: 5px;
    box-shadow: none;
  }
  &:active:enabled {
    border-radius: 5px;
    box-shadow: 0 0 5px 5px $--color-primary inset;
  }
  &:disabled {
    border-radius: 5px;
    box-shadow: none;
    background-color: $--color-primary-light-5;
  }
}
</style>