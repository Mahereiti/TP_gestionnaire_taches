<script>
import Board from "./components/Board.vue"
import Form from "./components/Form.vue"

export default {
  components: {
    Board,
    Form
  },

  data() {
    return {
      taches: JSON.parse(localStorage.getItem("taches")) || [
        { id: 1, titre: "Apprendre le HTML", description: "Cours", priorite: "Moyenne", statut: "A faire" },
        { id: 2, titre: "Apprendre le JavaScript", description: "A la maison", priorite: "Moyenne", statut: "En cours" },
        { id: 3, titre: "Apprendre Vue", description: "Voir le cours VueJS", priorite: "Haute", statut: "Terminée" }
      ],
      tache: {id: null, titre: "", description: "", priorite: "", statut: "A faire"},
      modifOk: false
    }
  },

  watch: {
    taches: {
      handler(taches) {
        localStorage.setItem("taches", JSON.stringify(taches))
      },
      deep: true
    }
  },

  methods: {
    changerTheme() {
      const html = document.documentElement;
      if (html.getAttribute("data-theme") === "dark") {
        html.setAttribute("data-theme", "light");
      } else {
        html.setAttribute("data-theme", "dark");
      }
    },

    ajouterTache(tache) {
      tache.id = Date.now()
      this.taches.push(tache)
      this.tache = {id: null, titre: "", description: "", priorite: "", statut: "A faire"}
    },

    clicModifierTache(tache) {
      this.tache = {...tache}
      this.modifOk = true
    },

    modifierTache(tache) {
      const id = this.taches.findIndex((t) => t.id === this.tache.id)
      if (id !== -1) {
        this.taches[id] = tache
      }

      this.tache = {id: null, titre: "", description: "", priorite: "", statut: "A faire"}
      this.modifOk = false
    },
  }
}
</script>

<template>
  <header>
    <div class="spacer"></div>
    <h1>Liste des tâches</h1>
    <button @click="changerTheme">Changer de thème</button>
  </header>
  <Form :tache=tache :modifOk="modifOk" @ajouterTache="ajouterTache" @modifierTache="modifierTache"/>

  <Board
    :taches="taches"
    @clicModifierTache="clicModifierTache"/>
</template>

<style>
  /* theme clair/sombre */
  :root {
    --bg-color: #ffffff;
    --text-color: #000000;
    --card-bg: #f8fafc;
    --border-color: #334155;
  }

  [data-theme='dark'] {
    --bg-color: #0f172a;
    --card-bg: #081333;
    --text-color: #e2e8f0;
    --border-color: #334155;
  }

  [data-theme='dark'] body {
    background-color: var(--bg-color);
    color: var(--text-color);
  }

  [data-theme='dark'] .colonne {
    background-color: var(--card-bg);
    border: 1px solid var(--border-color);
  }

  body {
    background-color: var(--bg-color);
    color: var(--text-color);
    transition: background-color 0.3s;
    margin: 0;
    font-family: Inter, Segoe UI, sans-serif;
  }

  /* header */
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px 32px 46px 32px;
  }

  .spacer {
    width: 150px;
  }

  h1 {
    margin: 0;
    flex-grow: 1;
    text-align: center;
    font-size: 2.5rem;
    font-weight: 800;
  }

  /* css éléments généraux (dans tous les composants) */
  button {
    padding: 14px;
    border: none;
    border-radius: 12px;
    background: linear-gradient(
      135deg,
      #3b82f6,
      #6366f1
    );
    color: white;
    font-weight: 700;
    cursor: pointer;
  }

  button:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 18px rgba(99,102,241,0.3);
  }

  h2 {
    text-align: center;
    font-size: 1.4rem;
    padding: 16px;
    letter-spacing: 0.5px;
    background: linear-gradient(135deg, #6366f1, #8b5cf6);
    color: white;
    border-radius: 14px;
    border: none;
  }

  input, select {
    background-color: var(--card-bg);
    color: var(--text-color);
    border: 1px solid var(--border-color);
    border-radius: 12px;
    padding: 10px;
    margin-left: 15px;
    margin-right: 15px;
    margin-bottom: 15px;
    font-size: 0.95rem;
    transition: 0.25s ease;
  }

  input:focus, select:focus {
    outline: 2px solid #6366f1;
  }
</style>
