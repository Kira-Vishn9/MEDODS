<script>
import Vue from "vue";
import DateInput from "../components/inputs/dateInput/dateInput.vue";
import ToggleInput from "../components/inputs/toggleInput/toggleInput.vue";
import mySelect from "../components/inputs/select/select.vue";
import LayoutError from "../components/layoutError/layoutError.vue";
import NumberInput from "../components/inputs/numberInput/numberInput.vue";
import Checkbox from "../components/inputs/checkbox/checkbox.vue";
import TextInput from "../components/inputs/textInput/textInput.vue";
import { useVuelidate } from "@vuelidate/core";
import { required, helpers } from "@vuelidate/validators";
import myValidation from "../validations/validation.js";
import { generalModel } from "../models/general.js";

export default {
  name: "General",
  components: {
    DateInput,
    ToggleInput,
    mySelect,
    LayoutError,
    NumberInput,
    Checkbox,
    TextInput,
  },
  setup: (_, { refs }) => ({ generalRef: refs, v$: useVuelidate() }),
  data() {
    return {
      generalModel,
      textInputPlaceHolderName: "Имя*",
      textInputPlaceHolderLastName: "Фамилия*",
      textInputPlaceHolderMiddleName: "Отчество*",
      dateInputPlaceHolderDateBirth: "Дата рождения*",
      numberInputPlaceHolderPhone: "Номер телефона*",
      arrayClients: ["VIP", "Проблемные", "ОМС"],
      arrayDoctors: ["Иванов", "Захаров", "Чернышева"],
    };
  },
  validations() {
    return {
      generalModel: {
        name: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          customValidationLength: helpers.withMessage(
            "Слишком короткое имя",
            myValidation.validateLength,
          ),
          customValidationAlphabetic: helpers.withMessage(
            "Используются неправильные символы",
            myValidation.validateAlphabeticInput,
          ),
          $lazy: true,
        },
        lastName: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          $lazy: true,
          customValidation: helpers.withMessage(
            "Слишком короткая фамилия",
            myValidation.validateLength,
          ),
          customValidationAlphabetic: helpers.withMessage(
            "Используются неправильные символы",
            myValidation.validateAlphabeticInput,
          ),
        },
        middleName: "",
        dateOfBirth: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          customValidation: helpers.withMessage(
            "Не может быть будующим числом",
            myValidation.validateToFuture,
          ),
          $lazy: true,
        },
        phoneNumber: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          customValidation: helpers.withMessage(
            "Не парвильный номер телефона",
            myValidation.validateRussianPhoneNumber,
          ),
          $lazy: true,
        },
        selectedClient: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          $lazy: true,
        },
        selectedDoctor: "",
      },
    };
  },
};
</script>

<template>
  <div class="wrap--general">
    <h2>Форма регистрации</h2>
    <div class="wrap--content">
      <layoutError :message="v$.generalModel.name.$errors[0]?.$message">
        <textInput
          @onChangeValidation="v$.generalModel.name.$reset()"
          v-model="generalModel.name"
          :placeholder="textInputPlaceHolderName"
        />
      </layoutError>

      <layoutError :message="v$.generalModel.lastName.$errors[0]?.$message">
        <textInput
          @onChangeValidation="v$.generalModel.lastName.$reset()"
          v-model="generalModel.lastName"
          :placeholder="textInputPlaceHolderLastName"
        />
      </layoutError>
      <layoutError>
        <textInput
          v-model="generalModel.middleName"
          :placeholder="textInputPlaceHolderMiddleName"
        />
      </layoutError>
      <layoutError :message="v$.generalModel.dateOfBirth.$errors[0]?.$message">
        <dateInput
          @onChangeValidation="v$.generalModel.dateOfBirth.$reset()"
          v-model="generalModel.dateOfBirth"
          :placeholder="dateInputPlaceHolderDateBirth"
        />
      </layoutError>

      <layoutError :message="v$.generalModel.phoneNumber.$errors[0]?.$message">
        <numberInput
          @onChangeValidation="v$.generalModel.phoneNumber.$reset()"
          v-model="generalModel.phoneNumber"
          :placeholder="numberInputPlaceHolderPhone"
        />
      </layoutError>

      <layoutError
        :message="v$.generalModel.selectedClient.$errors[0]?.$message"
      >
        <mySelect
          @onChangeValidation="v$.generalModel.selectedClient.$reset()"
          v-model="generalModel.selectedClient"
          :options="arrayClients"
        />
      </layoutError>
      <layoutError >
        <div class="select-row">
          <mySelect
            v-model="generalModel.selectedDoctor"
            :options="arrayDoctors"
          />
        </div>
      </layoutError>
      <layoutError >
        <toggleInput
          @update:value="
            (a) => {
              console.log((generalModel.gender = a));
            }
          "
          :checked="generalModel.gender"
        />
      </layoutError>
      <layoutError >
        <checkbox v-model="generalModel.isSubscribed" />
      </layoutError>
    </div>
  </div>
</template>

<style lang="sass">
.wrap--general
  position: relative
  border-radius: 25px
  background-color: rgba(255, 255, 255, 0.4)
  border: 2px solid black
  padding: 20px
  justify-items: center
  gap: 20px 10px
  margin: 10px 0
  width: 100%

  .wrap--content
    display: flex
    flex-direction: column
    padding: 20px
    justify-items: center
    gap: 10px
    box-sizing: border-box

  .select-row
    width: 100%
    text-align: -webkit-center

    select
      width: 100%
      display: flex
@media (max-width: 800px)
  .wrap--general
    display: flex
    flex-direction: column
    width: 100%
    .wrap--content
      display: flex
      flex-direction: column
</style>
