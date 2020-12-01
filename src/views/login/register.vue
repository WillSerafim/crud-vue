<template>
      <div class="form-register">

        <b-field label="Nome">
            <b-input v-model="name" type="text"></b-input>
        </b-field>

        <b-field label="E-mail">
            <b-input v-model="email" type="email"></b-input>
        </b-field>

        <b-field label="Senha">
            <b-input v-model="password" type="password"></b-input>
        </b-field>

        <div v-if="hasError" class="has-text-danger mb-2">
          <h3>{{messageError}}</h3>
        </div>

        <div class="field">
            <b-checkbox v-model="provider">Prestador de serviços</b-checkbox>
        </div>

        <b-button class="mb-2" type="is-success" @click="submit">Cadastrar</b-button>
      </div>
</template>

<script>
export default {
  data() {
    return {
      hasError: false,
      name: '',
      email: '',
      password: '',
      messageError: '',
      provider: false,
    };
  },
  methods: {
    submit() {
      const url = 'http://localhost:3333/user/create';

      const data = {
        name: this.name,
        email: this.email,
        password: this.password,
        provider: this.provider,
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
          this.$router.push('/');
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
  .form-register {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  }

</style>
