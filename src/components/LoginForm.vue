<template>
  <div>
    <card class="card">
      <template #title> Sign In </template>
      <template #content>
        <div class="p-grid">
          <div class="p-col-6">
            <Message
              v-if="error"
              severity="error"
              :sticky="true"
              @close="resetError"
              >{{ error.message }}</Message
            >
            <form @submit="sendData" @input="resetError">
              <div class="p-fluid">
                <div class="p-field">
                  <label for="username"
                    >Username
                    <span style="color: var(--pink-600)">*</span></label
                  >
                  <input-text id="username" type="text" v-model="username" />
                </div>
                <div class="p-field">
                  <label for="password"
                    >Password
                    <span style="color: var(--pink-600)">*</span></label
                  >
                  <input-text
                    id="password"
                    type="password"
                    v-model="password"
                  />
                </div>

                <Button type="submit" label="Sign In" class="p-mt-5" />
              </div>
            </form>
          </div>
          <div class="p-col p-d-flex p-jc-center p-ai-center">
            <Image
              src="https://www.nicepng.com/png/full/239-2398411_clipart-freeuse-download-collection-of-hardware-store-hardware.png"
              alt="Logo"
              width="300"
            />
          </div>
        </div>
      </template>
    </card>
  </div>
</template>
<script>
import { ref } from 'vue';
import Card from 'primevue/card';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import Image from 'primevue/image';
import Message from 'primevue/message';
export default {
  components: {
    Card,
    InputText,
    Button,
    Image,
    Message,
  },
  setup() {
    let username = ref('');
    let password = ref('');
    let error = ref(null);

    let sendData = (e) => {
      e.preventDefault();
      let fetchOptions = {
        method: 'POST',
        body: JSON.stringify({
          username: username.value,
          password: password.value,
        }),
      };
      fetch(`${process.env.VUE_APP_API}/user/login`, fetchOptions)
        .then(async (response) => {
          if (response.status == 400) {
            error.value = { message: 'All fields marked with * are required.' };
          } else if (response.status == 404) {
            error.value = { message: 'User does not exist.' };
          } else if (response.status == 403) {
            error.value = { message: 'Incorrect credentials.' };
          } else {
            let data = await response.json();
            sessionStorage.setItem('authToken', data.token)
          }
        })
        .catch((error) => alert(error));
    };

    let resetError = () => {
      error.value = null;
    };

    return { username, password, sendData, error, resetError };
  },
};
</script>
<style scoped>
card {
  background: var(--surface-a);
}
</style>
