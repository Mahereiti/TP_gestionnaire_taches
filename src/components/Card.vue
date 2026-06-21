<script>
export default {
  props: {
    tache: Object,
  },

  emits: ["supprimerTache", "clicModifierTache"],

  methods: {
    supprimerTache() {
      this.$emit("supprimerTache", this.tache)
    },
    clicModifierTache() {
      this.$emit("clicModifierTache", this.tache)
    }
  }
}
</script>

<template>
  <tr @click="clicModifierTache">
    <td>{{ this.tache.titre }}</td>
    <td>{{ this.tache.description }}</td>
    <td><span :class="this.tache.priorite">{{ this.tache.priorite }}</span></td>
    <td>{{ this.tache.statut }}</td>
    <td v-if="this.tache.statut !== 'Terminée'"><button @click.stop="supprimerTache">X</button></td>
  </tr>
</template>

<style>
  .Haute, .Moyenne, .Basse {
    padding: 5px 10px;
    border-radius: 999px;
    font-size: 0.8rem;
    text-align: center;
  }

  .Haute {
    background: #fec7c7;
    color: #920e0e;
  }
  .Moyenne {
    background: #fef3c7;
    color: #92400e;
  }
  .Basse {
    background: #dcfce7;
    color: #166534;
  }
</style>
