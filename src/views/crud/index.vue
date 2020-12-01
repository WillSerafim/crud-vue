<template>
    <section class="columns is-gapless">
      <div class="column">
        <button class="button field is-primary" @click="selected = null"
            :disabled="!selected">
            <b-icon icon="close"></b-icon>
            <span>Limpar</span>
        </button>

        <b-tabs>
            <b-tab-item label="Tabela">
                <b-table
                    :data="data"
                    :columns="columns"
                    :selected.sync="selected"
                    focusable>
                </b-table>
            </b-tab-item>

            <b-tab-item label="Usúario selecionado">
                <pre>{{ selected }}</pre>
            </b-tab-item>
        </b-tabs>
      </div>

      <div class="column">
        <b-field label="Nome">
            <b-input v-model="name" type="text"></b-input>
        </b-field>

        <b-field label="E-mail">
            <b-input v-model="email" type="email"></b-input>
        </b-field>

        <div class="field">
            <b-checkbox v-model="status">Status</b-checkbox>
        </div>

        <div class="field">
            <b-checkbox v-model="provider">Prestador de serviços</b-checkbox>
        </div>

        <b-button class="mb-2" type="is-success"  @click="submit">Salvar</b-button>
      </div>
    </section>
</template>

<script>
export default {
  data() {
    const data = [];
    return {
      data,
      selected: data[1],
      name: '',
      email: '',
      provider: false,
      status: false,
      columns: [
        {
          field: 'id',
          label: 'ID',
          width: '40',
          numeric: true,
        },
        {
          field: 'name',
          label: 'Nome',
        },
        {
          field: 'email',
          label: 'E-mail',
        },
        {
          field: 'provider',
          label: 'Barbeiro',
          centered: true,
        },
        {
          field: 'status',
          label: 'Status',
          centered: true,
        },
      ],
    };
  },
  created() {
    this.getUsers();
  },
  watch: {
    selected() {
      if (this.selected !== null) {
        this.name = this.selected.name;
        this.email = this.selected.email;
        this.provider = this.selected.provider;
        this.status = this.selected.status;
      } else {
        this.name = '';
        this.email = '';
        this.provider = false;
        this.status = false;
      }
    },
  },
  methods: {
    getUsers() {
      const url = 'http://localhost:3333/users';

      fetch(url, {
        method: 'get',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${localStorage.getItem('token')}`,
        },
      }).then((response) => {
        if (response.ok) {
          response.json().then((data) => {
            this.data = data.users.filter(((item) => item.status !== false));
            console.log(data);
          });
        }
      });
    },
    submit() {
      const url = `http://localhost:3333/user/${this.selected.id}/update`;

      const data = {
        name: this.name,
        email: this.email,
        provider: this.provider,
        status: this.status,
      };

      fetch(url, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${localStorage.getItem('token')}`,
        },
        body: JSON.stringify(data),
      }).then((response) => {
        if (response.ok) {
          response.json().then((dataResponse) => {
            this.getUsers();
            console.log(dataResponse);
          });
        }
      });
    },
  },
};
</script>
