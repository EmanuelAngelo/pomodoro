<template>
  <section class="projetos">
    <form @submit.prevent="salvar">
      <div class="field">
        <label for="nomeDoProjeto" class="label"> Nome do Projeto </label>
        <input
          type="text"
          class="input"
          v-model="nomeDoProjeto"
          id="nomeDoProjet"
        />
      </div>
      <div class="field">
        <button class="button" type="submit">Salvar</button>
      </div>
    </form>
  </section>
</template>

<script lang="ts">
import { TipoNotificacao } from "@/interfaces/INotificacao";
import { useStore } from "@/store";
import { ADICIONA_PROJETO, ALTERA_PROJETO, NOTIFICAR } from "@/store/tipo-mutacoes";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Formulario",
  props: {
    id: {
      type: String
    }
  },
  mounted () {
    if(this.id){
      const projeto = this.store.state.projetos.find(proj => proj.id == this.id)
      this.nomeDoProjeto = projeto?.nome || ''
    }
  },
  data() {
    return {
      nomeDoProjeto: ""
    };
  },
  methods: {
    salvar() {
      if (this.id) {
        this.store.commit(ALTERA_PROJETO, {
          id: this.id,
          nome: this.nomeDoProjeto
        })
      } else {
        this.store.commit(ADICIONA_PROJETO, this.nomeDoProjeto)
      }
      this.nomeDoProjeto = "";
      this.store.commit(NOTIFICAR, {
        titulo: 'Novo projeto foi salvo',
        texto: 'Prontinho, seu projeto já esta disponivel',
        tipo: TipoNotificacao.SUCESSO
      })
      this.$router.push('/projetos')
    },
  },
  setup () {
    const store = useStore()
    return {
      store
    }
  }
});
</script>

<style scoped>
.projetos {
  padding: 1.25rem;
}
</style>