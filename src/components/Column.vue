<script>
import Card from "./Card.vue"

export default {
  components: {
    Card
  },

  props: {
    taches: Array,
    toutesTaches: Array,
    statut: String
  },

  emits: ["clicModifierTache"],

  methods: {
    supprimerTache(tache) {
      //supprime tache dans la colonne
      const id = this.taches.findIndex((t) => t.id === tache.id)
      if (id !== -1) {
        this.taches.splice(id, 1)
      }
      //supprime tache dans la colonne toutes les taches
      const id2 = this.toutesTaches.findIndex((t) => t.id === tache.id)
      if (id2 !== -1) {
        this.toutesTaches.splice(id2, 1)
      }
    },

    clicModifierTache(tache) {
      this.$emit("clicModifierTache", tache)
    }
  }
}

</script>
<template>
  <div class="colonne">
    <table>
      <thead>
        <tr>
          <th colSpan="5" class="titre-statut">{{ statut }}</th>
        </tr>
        <tr>
          <th>Titre</th>
          <th>Description</th>
          <th>Priorité</th>
          <th>Statut</th>
        </tr>
      </thead>
      <tbody>
        <template v-if="taches.length>0">
          <Card v-for="tache in taches" :key="tache.id"
            :tache="tache"
            @supprimerTache="supprimerTache" @clicModifierTache="clicModifierTache"
          />
        </template>
        <tr v-else>
          <td colspan="5" class="ligneSeule">Pas de tâches</td>
        </tr>
      </tbody>
    </table>
    
    <!-- Stats -->
    <div class="stats">
      <span v-if="this.statut==='Toutes'" class="stat-badge total">Nombre de tâches total : {{ this.taches.length }}</span>
      <span v-if="this.statut==='A faire'" class="stat-badge a-faire">Tâches à faire : {{ this.taches.length }} / {{ this.toutesTaches.length }}</span>
      <span v-if="this.statut==='En cours'" class="stat-badge en-cours">Tâches en cours : {{ this.taches.length }} / {{ this.toutesTaches.length }}</span>
      <span v-if="this.statut==='Terminées'" class="stat-badge terminee">Tâches terminées : {{ this.taches.length }} / {{ this.toutesTaches.length }}</span>
    </div>
  </div>
</template>

<style>
  .colonne {
    display: flex;
    flex-direction: column;
    flex: 1;
    min-width: 400px;
  }

  .titre-statut {
    text-align: center;
    margin-top: 0;
    margin-bottom: 15px;
    font-size: 1.25rem;
    border-bottom: 2px solid #000000;
    padding-bottom: 8px;
  }

  table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    margin-bottom: 18px;
  }

  th, td {
    padding: 14px 10px;
    text-align: center;
    font-size: 0.92rem;
    vertical-align: middle;
    word-break: break-word;
  }

  th {
    font-weight: 700;
    text-transform: uppercase;
    font-size: 0.78rem;
    letter-spacing: 0.8px;
  }

  .ligneSeule {
    font-style: italic;
    text-align: center;
  }

  .stats {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-top: auto;
  }

  .stat-badge {
    padding: 10px 16px;
    border-radius: 999px;
    background: #f1f5f9;
    color: #475569;
    font-size: 0.9rem;
    font-weight: 600;
    text-align: center;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
    transition: all 0.25s ease;
  }

  .stat-badge.total {
    background: linear-gradient(135deg, #3b82f6, #6366f1);
    color: white;
    font-weight: 700;
    box-shadow: 0 8px 20px rgba(99, 102, 241, 0.3);
  }

  .stat-badge.a-faire {
    background: #fef3c7;
    color: #92400e;
  }

  .stat-badge.en-cours {
    background: #dbeafe;
    color: #1e40af;
  }

  .stat-badge.terminee {
    background: #dcfce7;
    color: #166534;
  }
</style>
