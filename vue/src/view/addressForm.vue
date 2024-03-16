<script>
import Vue from "vue";
import layoutError from "../components/layoutError/layoutError.vue";
import numberInput from "../components/inputs/numberInput/numberInput.vue";
import textInput from "../components/inputs/textInput/textInput.vue";
import myValidation from "../validations/validation.js";
import { required, helpers } from "@vuelidate/validators";
import { addressModel } from "../models/address.js";
import { useVuelidate } from "@vuelidate/core";

export default {
  name: "formAddress",
  components: { textInput, numberInput, layoutError },
  setup: (_, { refs }) => ({ addressRef: refs, v$: useVuelidate() }),
  validations() {
    return {
      addressModel: {
        indexAddress: "",
        country: "",
        region: "",
        city: {
          required: helpers.withMessage("Поле не должно быть пустым", required),
          customValidation: helpers.withMessage(
            "Короткое название",
            myValidation.validateLength,
          ),
          $lazy: true,
        },
        street: "",
        houseNumber: "",
      },
    };
  },
  data() {
    return {
      addressModel,
      numberInputPlaceHolderIndexAddress: "Индекс",
      textInputPlaceHolderCountry: "Страна",
      textInputPlaceHolderRegion: "Область",
      textInputPlaceHolderCity: "Город*",
      textInputPlaceHolderStreet: "Улица",
      numberInputPlaceHolderHouseNumber: "Дом",
    };
  },
};
</script>

<template>
  <div class="wrap--address">
    <h2>Адрес</h2>
    <layoutError >
      <numberInput
        v-model="addressModel.indexAddress"
        :placeholder="numberInputPlaceHolderIndexAddress"
      />
    </layoutError >
    <layoutError >
      <textInput
        v-model="addressModel.country"
        :placeholder="textInputPlaceHolderCountry"
      />
    </layoutError >
    <layoutError >
      <textInput
        v-model="addressModel.region"
        :placeholder="textInputPlaceHolderRegion"
      />
    </layoutError >
    <layoutError :message="v$.addressModel.city.$errors[0]?.$message">
      <textInput
        @onChangeValidation="v$.addressModel.city.$reset()"
        v-model="addressModel.city"
        :placeholder="textInputPlaceHolderCity"
      />
    </layoutError>
    <layoutError >
      <textInput
        v-model="addressModel.street"
        :placeholder="textInputPlaceHolderStreet"
      />
    </layoutError >
    <layoutError >
      <numberInput
        v-model="addressModel.houseNumber"
        :placeholder="numberInputPlaceHolderHouseNumber"
      />
    </layoutError >
  </div>
</template>

<style lang="sass">
.wrap--address
  position: relative
  border-radius: 25px
  background-color: rgba(255, 255, 255, 0.4)
  border: 2px solid black
  padding: 20px
  justify-items: center
  gap: 20px 10px
  margin: 10px 0
  width: 100%
@media (max-width: 800px)
  .wrap--address
    display: flex
    flex-direction: column
    width: 100%
</style>
