<template>
  <div class="form">
    <div>
      <span>Личные данные</span>
      <div>
        <custom-input
          v-model="formData.lastName"
          label="Фамилия"
          name="lastName"
        ></custom-input>
        <custom-input
          v-model="formData.firstName"
          label="Имя"
          name="firstName"
        ></custom-input>
        <custom-input
          v-model="formData.middleName"
          label="Отчество"
          name="middleName"
        ></custom-input>
      </div>
      <custom-input
        v-model="formData.birthDay"
        label="Дата рождения"
        name="birthDay"
      ></custom-input>
      <custom-input
        v-model="formData.email"
        label="Email"
        name="email"
      ></custom-input>
    </div>
    <div>
      <span>Пол</span>
      <custom-radiobutton
        v-model="formData.gender"
        value="male"
        label="Мужской"
        name="male"
      ></custom-radiobutton>
      <custom-radiobutton
        v-model="formData.gender"
        value="female"
        label="Женский"
        name="female"
      ></custom-radiobutton>
    </div>
    <div>
      <span>Паспортные данные</span>
      <custom-dropdown
        v-model="formData.citizenship"
        label="Гражданство"
        name="citizenship"
        valueField="id"
        textField="nationality"
        :items="citizenships"
      ></custom-dropdown>
      <div v-if="!isCitizenshipForeign">
        <custom-input
          v-model="formData.passportSeries"
          label="Серия паспорта"
          name="serialSeries"
        ></custom-input>
        <custom-input
          v-model="formData.passportNumber"
          label="Номер паспорта"
          name="passportNumber"
        ></custom-input>
        <custom-input
          v-model="formData.passportDate"
          label="Дата выдачи"
          name="passportDate"
        ></custom-input>
      </div>
      <div v-else>
        <custom-input
          v-model="formData.foreignLastName"
          label="Фамилия на латинице"
          name="foreignLastName"
        ></custom-input>
        <custom-input
          v-model="formData.foreignFirstName"
          label="Имя на латинице"
          name="foreignFirstName"
        ></custom-input>
        <custom-input
          v-model="formData.foreignPassportNumber"
          label="Номер паспорта"
          name="email"
        ></custom-input>
        <custom-dropdown
          v-model="formData.foreignPassportCountry"
          label="Страна выдачи"
          name="foreignPassportCountry"
          :items="citizenships"
        ></custom-dropdown>
        <custom-dropdown
          v-model="formData.foreignPassportType"
          label="Страна выдачи"
          name="foreignPassportType"
          :items="passportTypes"
        ></custom-dropdown>
      </div>
      <div>
        <span>Меняли ли вы фамилию?</span>
        <custom-radiobutton
          v-model="formData.isChangeName"
          :value="true"
          label="Да"
          name="yes"
        ></custom-radiobutton>
        <custom-radiobutton
          v-model="formData.isChangeName"
          :value="false"
          label="Нет"
          name="no"
        ></custom-radiobutton>
        <div v-if="formData.isChangeName">
          <custom-input
            v-model="formData.previousLastName"
            label="Предыдущая фамилия"
            name="previousLastName"
          ></custom-input>
          <custom-input
            v-model="formData.previousFirstName"
            label="Предыдущее имя"
            name="previousFirstName"
          ></custom-input>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import citizenships from "@/assets/data/citizenships";
import passportTypes from "@/assets/data/passport-types";

import CustomInput from "./CustomInput.vue";
import CustomRadiobutton from "./CustomRadiobutton.vue";
import CustomDropdown from "./CustomDropdown.vue";

export default {
  name: "Form",
  components: {
    CustomInput,
    CustomRadiobutton,
    CustomDropdown,
  },
  data() {
    return {
      formData: {
        lastName: "",
        firstName: "",
        middleName: "",
        birthDay: "",
        email: "",
        citizenship: null,
        passportSeries: "",
        passportNumber: "",
        foreignLastName: "",
        foreignFirstName: "",
        foreignPassportNumber: "",
        foreignPassportCountry: null,
        foreignPassportType: null,
        isChangeName: false,
        previousLastName: "",
        previousFirstName: "",
      },
      citizenships,
      passportTypes,
    };
  },
  computed: {
    isCitizenshipForeign() {
      return this.formData.citizenship?.nationality !== "Russia";
    },
  },
  methods: {},
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  padding: 1em;
}
</style>
