<script>
import { useVuelidate } from "@vuelidate/core";
import addressForm from "../../view/addressForm.vue";
import passport from "../../view/passport.vue";
import general from "../../view/general.vue";
import { generalModel } from "../../models/general.js";
import { addressModel } from "../../models/address.js";
import { passportModel } from "../../models/passport.js";

export default {
  components: { general, passport, addressForm },
  setup: () => ({ v$: useVuelidate() }),
  data() {
    return {
      isSubmit: false,
    };
  },
  methods: {
    async submitForm() {
      const isFormCorrect = await this.v$.$validate();
      console.log(
        "тут должна быть функция отправки на какой-нибудь сервер",
        generalModel,
        addressModel,
        passportModel,
      );
      this.isSubmit = isFormCorrect;
    },
  },
};
</script>

<template>
  <div class="wrap-form">
    <div v-if="isSubmit"><h1>Ваша заявка находится в обработке</h1></div>
    <form v-else>
      <general />
      <addressForm />
      <passport />
      <button type="submit" @click.prevent="submitForm">Зарегистрироваться</button>
    </form>
  </div>
</template>

<style lang="sass">
.wrap-form
  display: flex
  flex-direction: column
  justify-content: center
  width: 40vw
form
  display: flex
  flex-direction: column
  gap: 20px
  width: 100%
  text-align-last: center
  button
    width: 50%
    justify-self: center
    align-self: center

@media (max-width: 800px)
  .wrap-form
    width: 80vw
form
  display: flex
  flex-direction: column
  align-items: center
  button
    width: 100%

</style>
