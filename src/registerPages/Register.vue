<template>
  <div class="register_wrapper">
    <div class="register_text_control">
      <div class="title">Давайте познакомимся</div>
      <div class="hint gray">Сообщите нам, как правильно к вам обращаться</div>
    </div>
    <div class="register_form_control">
      <form class="register_form" :model="formData">
        <input
          type="text"
          class="input"
          placeholder="Имя"
          v-model="formData.given_names"
        />
        <input
          type="text"
          class="input"
          placeholder="Фамилия"
          v-model="formData.sur_name"
        />
        <input
          type="text"
          class="input"
          placeholder="Отчества"
          v-model="formData.father_name"
        />
        <select
          v-model="formData.gender"
          name="gender"
          id="gender"
          class="select"
          placeholder="Выберите свой пол"
        >
          <option value="MALE">Мужской</option>
          <option value="FEMALE">Женщина</option>
        </select>
        <input
          type="date"
          class="input"
          placeholder="День рождения"
          v-model="formData.birthday"
        />
      </form>
      <pre>{{ formData }}</pre>
    </div>
  </div>
  <button
    class="btn main_button"
    :disabled="isLoading"
    v-if="show"
    @click="submitHandlerInRegister"
  >
    OK
  </button>
</template>

<script setup>
import { ref, watchEffect, computed } from "vue";
import { useStore } from "vuex";

const store = useStore();
const isLoading = computed(() => store.state.auth.isLoading);
const tg = window.Telegram.WebApp;
const phone = localStorage.getItem("yallavebphone");
const key = localStorage.getItem("yallavebkey");
const show = ref(false);
const formData = ref({
  phone: phone,
  given_names: "",
  sur_name: "",
  father_name: "",
  gender: "MALE",
  birthday: "",
  key: key,
});

const submitHandlerInRegister = async () => {
  try {
    await store.dispatch("registerUser", formData);
  } catch (err) {
    console.error("Error validating code:", err);
  }
};

const showButton = () => {
  const { given_names, sur_name, birthday } = formData.value;
  if (given_names && sur_name) {
    show.value = true;
  } else {
    show.value = false;
  }
};
watchEffect(() => {
  showButton();
  //   tg.MainButton.setParams({
  //     text: "OK",
  //   });
  //   tg.onEvent("mainButtonClicked", submitHandler);
});
</script>
<style scoped></style>
