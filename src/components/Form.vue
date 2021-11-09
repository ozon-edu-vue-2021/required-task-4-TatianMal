<template>
  <form class="form" @submit.prevent="saveData">
    <div class="form-section">
      <div class="sub-header">Личные данные</div>
      <div class="row">
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
    <div class="form-section">
      <div class="sub-header">Пол</div>
      <div class="radio-group">
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
    </div>
    <div class="form-section">
      <div class="sub-header">Паспортные данные</div>
      <custom-dropdown
        v-model="formData.citizenship"
        label="Гражданство"
        name="citizenship"
        valueField="id"
        textField="nationality"
        :items="citizenships"
      ></custom-dropdown>
      <div v-if="hasCitizenship && !isCitizenshipForeign" class="row">
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
      <div v-else-if="hasCitizenship">
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
          valueField="id"
          textField="nationality"
          :items="citizenships"
        ></custom-dropdown>
        <custom-dropdown
          v-model="formData.foreignPassportType"
          label="Тип паспорта"
          name="foreignPassportType"
          valueField="id"
          textField="type"
          :items="passportTypes"
        ></custom-dropdown>
      </div>
      <div>
        <span>Меняли ли вы фамилию?</span>
        <div class="radio-group">
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
        </div>
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
    <button class="btn-submit">Отправить</button>
  </form>
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
        passportDate: "",
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
    hasCitizenship() {
      return !!this.formData.citizenship?.nationality;
    },
    isCitizenshipForeign() {
      return this.formData.citizenship?.nationality !== "Russia";
    },
  },
  watch: {
    isCitizenshipForeign(value) {
      if (!value) {
        this.cleanForeignPassportFields();
        return;
      }
      this.cleanRussianPassportFields();
    },
    "formData.isChangeName"(value) {
      if (!value) {
        this.formData.previousLastName = "";
        this.formData.previousFirstName = "";
      }
    },
  },
  methods: {
    saveData() {
      console.log(this.formData);
    },
    cleanForeignPassportFields() {
      this.formData.foreignLastName = "";
      this.formData.foreignFirstName = "";
      this.formData.foreignPassportNumber = "";
      this.formData.foreignPassportCountry = null;
      this.formData.foreignPassportType = null;
    },
    cleanRussianPassportFields() {
      this.formData.passportSeries = "";
      this.formData.passportNumber = "";
      this.formData.passportDate = "";
    },
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  padding: 1em;
}
.form-section {
  padding-bottom: 0.75em;
}
.sub-header {
  font-size: 1.5em;
}
.radio-group {
  display: flex;
}
.row {
  display: flex;
  justify-content: space-between;
}
.btn-submit {
  background-color: rgb(46, 46, 228);
  color: white;
  padding: 20px;
  align-self: flex-end;
  border-radius: 8px;
}
</style>
