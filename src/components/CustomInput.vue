<template>
  <div class="input-wrapper">
    <VueSelect
      v-if="field.type === 'array'"
      class="form-input"
      :class="validate.error && validate.dirty ? 'field-error' : ''"
      :value="modelValue.name"
      :options="field.options"
      label="name"
      :placeholder="field.placeholder"
      @option:selected="$emit('updatePosition', $event)"
    />
    <input
      ref="stringInput"
      :type="inputType"
      :value="modelValue"
      v-else-if="field.type === 'string' || field.type === 'password'"
      :placeholder="field.placeholder"
      @change="$emit('updateInput', $event.target.value)"
      class="form-input"
      :class="validate.error && validate.dirty ? 'field-error' : ''"
      autocomplete="new-password"
    />
    <img
      v-if="inputType === 'password'"
      src="../../src/assets/hidden.svg"
      alt=""
      @click="changeInputType"
      class="password-field-img"
    />
    <img
      v-if="inputType === 'text'"
      src="../../src/assets/showed.svg"
      alt=""
      @click="changeInputType"
      class="password-field-img"
    />
    <p class="error" v-if="validate.error && validate.dirty">
      Поле "{{ field.placeholder }}" обязательно для заполнения
    </p>
  </div>
</template>

<script>
import { VueSelect } from "vue-select";

export default {
  data() {
    return {
      inputType: "",
    };
  },
  name: "CustomInput",
  props: ["field", "modelValue", "validate"],
  components: { VueSelect },
  methods: {
    changeInputType() {
      this.inputType === "text"
        ? (this.inputType = "password")
        : (this.inputType = "text");
    },
  },
  mounted() {
    this.field.type === "password" ? (this.inputType = "text") : "";
  },
};
</script>

<style scoped lang="scss">
@import "src/styles/VueSelect";
@import "~vue-select/src/css/vue-select.css";
.password-field-img {
  width: 21px;
  height: 11px;
}

.input-wrapper {
  position: relative;
  max-width: 450px;
  width: 100%;
  > div {
    padding: 0;
    margin: 0;
  }
  &:nth-child(5) {
    grid-column-start: 2;
    grid-row-start: 2;
  }
  &:nth-child(4) {
    grid-column-start: 2;
    grid-row-start: 3;
  }
  &:nth-child(3) {
    grid-column-start: 1;
    grid-row-start: 3;
  }
  .form-input {
    width: 100%;
    height: 40px;
    padding: 10px;
    border-radius: 11px;
    border: 1px solid #e6e6eb;
    font-size: 14px;
    &::placeholder {
      color: #9292a0;
      font-size: 14px;
    }
  }
  .password-field-img {
    position: absolute;
    right: 8px;
    top: 15px;
    cursor: pointer;
  }
}
</style>
