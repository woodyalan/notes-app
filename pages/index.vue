<template>
  <div>
    <b-navbar type="dark" variant="dark">
      <b-navbar-brand href="/">Notes App</b-navbar-brand>
      <b-navbar-toggle target="notesBar"></b-navbar-toggle>

      <b-collapse id="notesBar" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown :text="primeiroNome" right>
            <b-dropdown-item href="perfil">Perfil</b-dropdown-item>
            <b-dropdown-item href="#" @click.prevent="logout"
              >Sair</b-dropdown-item
            >
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <div class="container">
      <p v-if="$fetchState.pending" class="text-center">Carregando...</p>
      <p v-else-if="$fetchState.error" class="text-center">
        Ocorreu um erro :(
      </p>
      <div v-else class="row">
        <div v-for="nota of notas" :key="nota.id" class="col-md-3">
          <div class="card bg-warning my-3">
            <div class="card-body">
              <h5 class="card-title">{{ nota.titulo }}</h5>
              <p v-if="nota.descricao" class="card-text">
                {{ nota.descricao }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  middleware: "auth",
  data() {
    return {
      notas: []
    };
  },
  async fetch() {
    const { data } = await this.$axios.get(`nota/usuario/${this.usuario.id}`);
    this.notas = data;
  },
  computed: {
    usuario() {
      return this.$store.state.auth.user;
    },
    primeiroNome() {
      const [nome] = this.usuario.nome.split(" ");

      return nome;
    }
  },
  methods: {
    async logout() {
      await this.$auth.logout();
      this.$router.push("/login");
    }
  }
};
</script>

<style>
.card {
  min-height: 15rem;
}
</style>
