<template>
  <form class="registration-form" @submit.prevent="submit">
    <div class="registration-form__header">Регистрация</div>
    <div class="registration-form__body">
      <div class="registration-form__hint">Заполните Ваши данные</div>
      <div class="registration-form__fields">
        <CustomInput
          v-for="field in formSettings.fields"
          :field="field"
          :validate="{
            error: v$.formValues[field.name].$error,
            dirty: v$.formValues[field.name].$dirty,
          }"
          :key="field.name"
          :model-value="formValues[field.name]"
          v-model="formValues[field.name]"
          @updatePosition="(newValue) => (formValues[field.name] = newValue)"
          @updateInput="(newValue) => (formValues[field.name] = newValue)"
        />
      </div>
    </div>
    <div class="registration-form__footer">
      <div class="privacy-mode-switcher">
        <CustomSwitcher
          :value="formValues.isPrivacy"
          @switchChange="(value) => (formValues.isPrivacy = value)"
        />
        <div class="privacy-mode-switcher__text-wrapper">
          <span class="privacy-mode-switcher__label">
            Хотите чтобы Ваш профиль видели другие участники платформы?
          </span>
          <span class="privacy-mode-switcher__description">
            Включает профиль для просмотра другими пользователями по ссылке
          </span>
        </div>
      </div>
      <div class="user-agreement-checkbox">
        <CustomCheckbox
          :value="formValues.isAgreement"
          @checkboxChange="(value) => (formValues.isAgreement = value)"
        />
        <div class="user-agreement-checkbox__text">
          Регистрируясь, Вы соглашаетесь с
          <a href="#">политикой конфиденциальности</a> и обработкой
          <a href="#">персональных данных</a>
        </div>
        <PrimaryButton />
      </div>
      <p class="error" v-if="!formValues.isAgreement">
        Необходимо Ваше согласие на обработку персональных данных
      </p>
    </div>
  </form>
</template>

<script>
import CustomInput from "@/components/CustomInput";
import CustomSwitcher from "@/components/CustomSwitcher";
import CustomCheckbox from "@/components/CustomCheckbox";
import PrimaryButton from "@/components/PrimaryButton";
import { useVuelidate } from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";

export default {
  data() {
    return {
      formValues: {
        name: "",
        email: "",
        position: {
          name: undefined,
          value: undefined,
        },
        password: "",
        repeatPassword: "",
        isPrivacy: true,
        isAgreement: true,
      },
    };
  },
  setup: () => ({ v$: useVuelidate() }),
  name: "CustomForm",
  components: { CustomInput, CustomSwitcher, CustomCheckbox, PrimaryButton },
  props: ["formSettings"],
  methods: {
    async submit() {
      const valid = await this.v$.$validate();
      if (!valid || !this.formValues.isAgreement) {
        return;
      } else {
        await fetch("someUrl", {
          method: "POST",
          body: {
            public: this.formValues.isPrivacy,
            username: this.formValues.name,
            role: this.formValues.position.value,
            email: this.formValues.email,
            password: this.formValues.password,
            password_repeat: this.formValues.repeatPassword,
          },
        }).finally(() => {
          console.log({
            public: this.formValues.isPrivacy,
            username: this.formValues.name,
            role: this.formValues.position.value,
            email: this.formValues.email,
            password: this.formValues.password,
            password_repeat: this.formValues.repeatPassword,
          });
          this.$emit("hideForm");
        });
      }
    },
  },
  validations() {
    return {
      formValues: {
        name: { required, $autoDirty: true },
        email: { required, email, $autoDirty: true },
        position: {
          value: { required, $autoDirty: true },
          name: { required, $autoDirty: true },
        },
        password: { required, $autoDirty: true },
        repeatPassword: { required, $autoDirty: true },
      },
    };
  },
};
</script>

<style scoped lang="scss">
//Если бы задача была объёмнее, я бы большинство дефолтных значений вынес в переменные
//В пределах маленькой одноразовой задачи считаю допустимым данную запись
.registration-form {
  width: 100%;
  align-self: center;
  display: flex;
  flex-direction: column;
  max-width: 960px;
  border-radius: 15px;
  //Добавил box-shadow чтобы было лучше видно границы формы
  box-shadow: 10px 10px 10px 10px #e5e5e5;

  &__header {
    padding: 17px 31px;
    border-bottom: 1px solid #d9d9d9;
    color: #000;
    font-family: Montserrat, sans-serif;
    font-size: 19px;
    font-style: normal;
    font-weight: 700;
    line-height: 27px;
    letter-spacing: -0.066px;
  }

  &__body {
    display: flex;
    flex-direction: column;
    gap: 34px;
    padding: 17px 15px 31px 30px;
    border-bottom: 1px solid #d9d9d9;
  }

  &__hint {
    color: #000;
    font-family: Montserrat, sans-serif;
    font-size: 16px;
    font-style: normal;
    font-weight: 500;
    line-height: 19px;
  }

  &__fields {
    display: grid;
    flex-wrap: wrap;
    grid-template-columns: repeat(2, 1fr);
    column-gap: 14px;
    row-gap: 31px;
  }

  &__footer {
    display: flex;
    flex-direction: column;
    padding: 23px 15px 65px 31px;
    gap: 30px;

    .privacy-mode-switcher {
      display: flex;
      flex-direction: row;
      align-items: flex-start;
      gap: 5px;

      &__text-wrapper {
        display: flex;
        flex-direction: column;
        gap: 10px;
      }

      &__label {
        color: #000;
        font-size: 16px;
        font-style: normal;
        font-weight: 500;
        line-height: 19px;
      }

      &__description {
        color: #696977;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: 19px;
        letter-spacing: -0.021px;
      }
    }

    .user-agreement-checkbox {
      display: flex;
      flex-direction: row;
      gap: 14px;

      &__text {
        color: #000;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: 19px;
        letter-spacing: -0.021px;
        max-width: 510px;
      }

      a {
        color: #3586ff;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: 19px;
        letter-spacing: -0.021px;
        text-decoration: none;
      }
    }
  }
}
</style>
