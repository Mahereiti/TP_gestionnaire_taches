<script>
import Column from "./Column.vue"

export default {
  data() {
    return {
      recherche: "",
      filtrePriorite: "",
    }
  },

  components: {
    Column
  },

  props: {
    taches: Array,
  },

  computed: {
    //filtre base sur recherche et priorite
    tachesFiltrees() {
      return this.taches.filter((t) =>
        (t.titre.toLowerCase().includes(this.recherche.toLowerCase())
          || t.description.toLowerCase().includes(this.recherche.toLowerCase()))
        && (this.filtrePriorite == "" || t.priorite == this.filtrePriorite))
    },

    //filtre par colonnes
    tachesAFaire() {
      return this.tachesFiltrees.filter((t) => t.statut == "A faire")
    },
    tachesEnCours() {
      return this.tachesFiltrees.filter((t) => t.statut == "En cours")
    },
    tachesTerminees() {
      return this.tachesFiltrees.filter((t) => t.statut == "Terminée")
    }
  },

  emits: ["clicModifierTache"],

  methods: {
    clicModifierTache(tache) {
      this.$emit("clicModifierTache", tache)
    }
  }
}
</script>

<template>
  <h2>Tableau de bord</h2>
  <div class="filtres">
    <input v-model="recherche" placeholder="Rechercher une tâche" />

    <select v-model="filtrePriorite">
      <option value="">Toutes priorités</option>
      <option>Haute</option>
      <option>Moyenne</option>
      <option>Basse</option>
    </select>
  </div>
  <div class="colonnes">
    <Column
      :taches="tachesFiltrees" :toutesTaches="taches" statut="Toutes"
      @clicModifierTache="clicModifierTache"/>
    <Column
      :taches="this.tachesAFaire" :toutesTaches="taches" statut="A faire"
      @clicModifierTache="clicModifierTache"/>
    <Column
      :taches="this.tachesEnCours" :toutesTaches="taches" statut="En cours"
      @clicModifierTache="clicModifierTache"/>
    <Column
      :taches="this.tachesTerminees" :toutesTaches="taches" statut="Terminées"
      @clicModifierTache="clicModifierTache"/>
  </div>
</template>

<style scoped>
  .colonnes {
    display: flex;
    flex-direction: row;
    margin-bottom: 24px;
  }
</style>
