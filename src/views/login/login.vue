<template>
      <div class="form-login">

        <b-field label="E-mail">
            <b-input v-model="email" type="email"></b-input>
        </b-field>

        <b-field label="Senha">
            <b-input v-model="password" type="password"></b-input>
        </b-field>

        <div v-if="hasError" class="has-text-danger mb-2">
          <h3>{{messageError}}</h3>
        </div>

        <b-button class="mb-2" type="is-success" @click="submit">Entrar</b-button>

        <router-link to="/register">Registrar-se</router-link>
      </div>
</template>

<script>
export default {
  data() {
    return {
      hasError: false,
      email: '',
      password: '',
      messageError: '',
    };
  },
  methods: {
    submit() {
      const url = 'http://localhost:3333/session';

      const data = {
        email: this.email,
        password: this.password,
      };

      fetch(url, {
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(data),
      }).then((response) => {
        if (!response.ok) {
          // eslint-disable-next-line no-shadow
          response.json().then((data) => {
            this.hasError = true;
            this.messageError = data.error;
          });
        } else {
          // eslint-disable-next-line no-shadow
          response.json().then((data) => {
            localStorage.setItem('token', data.token);
          });
          this.$router.push('/crud');
        }
      }).catch((err) => {
        this.hasError = true;
        this.messageError = err;
      });
    },
  },
};
</script>

<style>
  .form-login {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  }

</style>
