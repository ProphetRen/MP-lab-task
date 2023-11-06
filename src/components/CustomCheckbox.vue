<template>
  <div class="form-agreement">
    <input
      :value="value"
      type="checkbox"
      class="custom-checkbox"
      id="checkbox"
      name="checkbox"
      :checked="value"
      @change="$emit('checkboxChange', !value)"
    />
  </div>
</template>

<script>
export default {
  name: "CustomCheckbox",
  props: ["value"],
};
</script>

<style scoped lang="scss">
.custom-checkbox {
  width: 19px;
  height: 17px;
  cursor: pointer;
}

.custom-checkbox:checked + label,
.custom-checkbox:not(:checked) + label {
  display: inline-block;
  box-sizing: border-box;
  position: relative;
  padding-left: 28px;
  line-height: 18px;
  cursor: pointer;
}

.custom-checkbox:not(:checked) + label::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 16px;
  height: 16px;
  border: 1px solid #ffffff;
  background-color: #ffffff;
  border-radius: 3px;
}

.custom-checkbox:checked + label:before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 16px;
  height: 16px;
  border: 1px solid #3586ff;
  background-color: #3586ff;
  border-radius: 3px;
  background-image: url("../assets/check-mark.svg");
  background-repeat: no-repeat;
  background-position: center;
}

.custom-checkbox:checked + label:after,
.custom-checkbox:not(:checked) + label:after {
  position: absolute;
  transition: all 0.2s ease;
}
</style>
