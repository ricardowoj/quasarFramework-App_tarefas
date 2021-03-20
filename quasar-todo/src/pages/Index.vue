<template>
  <q-page padding>
    <div class="row q-mb-sm">
      <q-input
        v-model="novaTarefa"
        label="Nova tarefa"
        class="col"
        @keyup.enter="addTarefa"
      />
    </div>
    <div class="q-mb-lg">
      <q-btn
        size="md"
        color="primary"
        label="Adicionar"
        @click.native="addTarefa"
      />
    </div>

    <div class="row q-mb-lg">
      <q-list bordered class="col">
        <q-item-label header>Pendentes</q-item-label>
        <q-item
          v-for="(tarefa, index) in tarefasPendentes"
          :key="index"
          clickable
          v-ripple
          class="row"
        >
          <q-item-section avatar>
            <q-icon name="cached" color="blue" />
          </q-item-section>
          <q-item-section>
            {{ tarefa }}
          </q-item-section>
          <q-item-section avatar>
            <q-icon
              name="check_circle_outline"
              color="green"
              title="Finalizar tarefa"
              @click.native="addFinalizada(index)"
            />
          </q-item-section>
        </q-item>
      </q-list>
    </div>

    <div class="row">
      <q-list bordered class="col">
        <q-item-label header>Finalizadas</q-item-label>
        <q-item
          v-for="(tarefa, index) in tarefasFinalizadas"
          :key="index"
          clickable
          v-ripple
          class="row"
        >
          <q-item-section avatar>
            <q-icon name="done_outline" color="green" />
          </q-item-section>
          <q-item-section>{{ tarefa }}</q-item-section>
          <q-item-section side>
            <q-icon
              name="delete"
              color="red"
              @click.native="deletarTarefa(index)"
            />
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      tarefasPendentes: [],
      tarefasFinalizadas: [],
      novaTarefa: ""
    };
  },
  methods: {
    addTarefa() {
      if (this.novaTarefa === "") {
        this.$q.dialog({
          title: "Alerta!",
          message: "Digite uma nova tarefa para adicionar."
        });
      } else {
        this.tarefasPendentes.push(this.novaTarefa);
        this.novaTarefa = "";
      }
    },
    editarTarefa(index) {},
    addFinalizada(index) {
      this.tarefasFinalizadas.push(this.tarefasPendentes[index]);
      this.tarefasPendentes.splice(index, 1);
    },
    deletarTarefa(index) {
      this.$q
        .dialog({
          title: "Alerta!",
          message: "Deseja realmente remover?",
          cancel: true,
          persistent: true,
          ok: "Sim",
          cancel: "Voltar"
        })
        .onOk(() => {
          this.tarefasFinalizadas.splice(index, 1);
          this.$q.notify("Removido!");
        })
        .onCancel(() => {
          // console.log('>>>> Cancel')
        });
    }
  }
};
</script>
