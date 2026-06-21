<script>
  export default {
    props: {
      tache: Object,
      modifOk: Boolean
    },

    emits: ["ajouterTache", "modifierTache"],

    methods: {
      ajouterTache() {
        this.$emit("ajouterTache", this.tache)
      },
      modifierTache() {
        this.$emit("modifierTache", this.tache)
      }
    }
  }
</script>

<template>
  <!-- Ajouter tache -->
  <h2>Formulaire des tâches (Ajouter/Modifier)</h2>
  <form v-if="!this.modifOk" @submit.prevent="ajouterTache">
    <input v-model="this.tache.titre" placeholder="Titre" required />
    <input v-model="this.tache.description" placeholder="Description" required />
    <select v-model="this.tache.priorite" required>
      <option disabled value="">Priorité</option>
      <option>Haute</option>
      <option>Moyenne</option>
      <option>Basse</option>
    </select>
    <button type="submit">Ajouter</button>
  </form>
  
  <!-- Modifier tache -->
  <form v-else @submit.prevent="modifierTache">
    <input v-model="this.tache.titre" placeholder="Titre" required />
    <input v-model="this.tache.description" placeholder="Description" required />
    <select v-model="this.tache.priorite" required>
      <option disabled value="">Priorité</option>
      <option>Haute</option>
      <option>Moyenne</option>
      <option>Basse</option>
    </select>
    <select v-model="this.tache.statut" required>
      <option disabled value="">Statut</option>
      <option>A faire</option>
      <option>En cours</option>
      <option>Terminée</option>
    </select>
    <button type="submit">Modifier</button>
  </form>
</template>

<style scoped>
  form {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding-top: 15px;
    margin: 24px;
    border-radius: 24px;
    box-shadow:
      0 10px 25px rgba(0,0,0,0.08),
      0 2px 8px rgba(0,0,0,0.04);
  }
</style>
