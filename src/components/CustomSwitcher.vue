<template>
  <label class="switch">
    <input
      type="checkbox"
      class="switch__input"
      :checked="value"
      @change="$emit('switchChange', !value)"
    />
    <span class="switch__slider"></span>
  </label>
</template>

<script>
export default {
  name: "CustomSwitcher",
  props: ["value"],
};
</script>

<style scoped lang="scss">
.switch {
  display: inline-block;
  position: relative;
  width: 40px;
  height: 20px;
  border-radius: 26px;
  background-color: #9292a0;
  &__input {
    height: 0;
    width: 0;
    opacity: 0;
    &:checked {
      + .switch__slider {
        background-color: #3586ff;
        border-radius: 26px;
        transition: 0.6s;
        &::before {
          transform: translateX(20px);
        }
      }
    }
  }
  &__slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    &:before {
      content: "";
      position: absolute;
      top: 0;
      cursor: pointer;
      height: 18px;
      width: 18px;
      border-radius: 24px;
      background-color: #fff;
      transition: 0.4s;
      border: 1px solid #d5d5d5;
    }
  }
}
</style>
