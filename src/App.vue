<template>
  <div id="app">
    <CustomForm
      :form-settings="formSettings"
      @hideForm="isSuccess = true"
      v-if="!isSuccess"
    />
    <SuccessPopup v-else></SuccessPopup>
  </div>
</template>

<script>
import CustomForm from "@/components/CustomForm";
import SuccessPopup from "@/components/SuccessPopup";

export default {
  name: "App",
  components: { CustomForm, SuccessPopup },
  data() {
    return {
      isSuccess: false,
    };
  },
  computed: {
    formSettings() {
      return {
        type: "registration",
        fields: [
          {
            type: "string",
            placeholder: "Имя",
            name: "name",
          },
          {
            type: "string",
            placeholder: "Email",
            name: "email",
          },
          {
            type: "password",
            placeholder: "Пароль",
            name: "password",
          },
          {
            type: "password",
            placeholder: "Повторите пароль",
            name: "repeatPassword",
          },
          {
            type: "array",
            placeholder: "Должность",
            name: "position",
            options: [
              { value: 1, name: "Директор" },
              { value: 2, name: "Разработчик" },
              { value: 3, name: "Глава отдела разработки" },
            ],
          },
        ],
      };
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: Montserrat, sans-serif;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.field-error {
  border: 1px solid red !important;
}
.error {
  color: red;
  font-size: 10px;
}
</style>
