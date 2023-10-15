<template>
  <div class="grid place-items-center">
    <img alt="Vue logo" src="./assets/logo.png" class="mb-10" />

    <otp v-model="pin" :count="7" ref="otpComponent">
      <template v-slot="{ digits, onInput, onPaste, onBlur }">
        <div class="flex items-center space-x-4">
          <otp-group class="flex space-x-2 p-2">
            <template v-slot="{ focusNext, focusPrev }">
              <otp-group-input
                v-for="(digit, index) in digits"
                :key="index"
                :value="digit"
                autofocus
                placeholder="*"
                @blur="onBlur"
                @next="focusNext(index)"
                @prev="focusPrev(index)"
                @paste="onPaste(index, $event)"
                @input="onInput(index, $event)"
                class="h-12 w-12 rounded-xl text-3xl text-center text-slate-700 caret-teal-400 border-2 border-slate-300 placeholder:text-slate-400 focus:outline-none focus:border-teal-400"
              />
            </template>
          </otp-group>

          <button
            class="w-8 h-8 rounded-full transition-color text-slate-700 hover:text-teal-500"
            @click="clearPin" 
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </button>
        </div>
      </template>

      <template v-slot:error="{ message, hasError }">
        <otp-error-message :message="message" :hasError="hasError" />
      </template>
    </otp>
  </div>
</template>

<script>
import { defineComponent } from "vue";

// COMPONENTS
import { Otp, OtpGroup, OtpGroupInput, OtpErrorMessage } from "@/components/otp";

export default defineComponent({
  name: "App",

  components: {
    Otp,
    OtpGroup,
    OtpGroupInput,
    OtpErrorMessage,
  },

  data() {
    return {
      pin: "",
    };
  },

  watch: {
    pin(pin) {
      console.log({ pin });
    },
  },

  methods: {
    clearPin() {
      this.pin = "";
      this.$refs.otpComponent.clearDigits();
    },
  },
});
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
