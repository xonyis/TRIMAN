<script >
import { ref } from 'vue'
import WelcomeItem from '../components/WelcomeItem.vue'
import DocumentationIcon from '../components/icons/IconDocumentation.vue'
import { RulesEnum } from '../enums/RulesEnum.js';

export default {
  setup() {
    // Convertir l'enum en une liste réactive
    const rules = ref(Object.entries(RulesEnum).map(([key, value]) => ({ key, ...value })));

    return { rules };
  },
  components: {
    WelcomeItem,
    DocumentationIcon
  }
};
</script>

<template>
  <div class="rules-page">
    <WelcomeItem>
      <template #icon>
        <DocumentationIcon />
      </template>
      <template #heading>Règles du Jeu</template>
    </WelcomeItem>
    
    <div class="rules-content">
        <h2 style="margin-bottom: .2em;">Comment jouer</h2>
        
        <p>- On cherche un triman :</p>
        <p class="small-text">Le premier joueur à obtenir un 3 ou dont la somme des dés est égale à 3 devient le Triman pour ce tour. (il boit une gorgé pour fêter ca !)</p>
        
        
        <p>- Déroulement du tour :</p>
        <p class="small-text">Les joueurs lancent les dés dans l'ordre, et les règles suivantes s'appliquent :</p>

        <div class="rules-table">
          <div class="table-header">
            <div>Résultat</div>
            <div>Règle</div>
            <div>Gorgées</div>
          </div>
          <div v-for="(rule, index) in rules" :key="index" class="table-row">
            <div>{{ rule.result }}</div>
            <div>{{ rule.rule }}</div>
            <div>{{ rule.drinks }}</div>
          </div>
          <p style="font-size: .7em; padding: .5em;">(En cas de double, le joueur donne le nombre de gorgées correspondant au chiffre obtenu)</p>
        </div>

        <p class="small-text">Tant que le résultat entraîne des actions, le joueur relance les dés.</p>
        <p>- Fin du tour :</p>
        <p class="small-text">Le tour se termine quand tous les joueurs ont joué, le Triman jouant toujours en dernier.</p>
        

        <div class="additional-info">
          <h3>Règles supplémentaires</h3>
          <ul>
            <li>En cas de double, le joueur donne le nombre de gorgées correspondant au chiffre obtenu.</li>
            <li>Le jeu continue jusqu'à ce que tous les joueurs aient bu suffisamment ou que vous décidiez d'arrêter.</li>
          </ul>
        </div>
      </div>
  </div>

  <!-- <p class="small-text">( une fois le triman désigné il n'y a que la personne dont c'est le tour qui a le droit de toucher les dées) </p> faire section regle annexes genre ignorance -->
</template>

<style scoped>
.rules-page {
  color: #333;
  max-width: 800px;
  margin: 0 auto;
  font-size: 14px;
}

.rules-content {
  background-color: #f9f9f9;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-top: 1em;
}

h2, h3 {
  color: #2c3e50;
  margin-top: 0;
}

.rules-table {
  width: 100%;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
  margin-top: 15px;
  margin-bottom: 5px;
}

.small-text {
  font-size: 11px;
  padding: 0.2em 1em;
  margin-bottom: .7em;
}

.table-header, .table-row {
  display: grid;
  grid-template-columns: 0.5fr 2fr 0.5fr;
  text-align: center;
}

.table-header {
  background-color: #3498db;
  color: white;
  font-weight: bold;
}

.table-header > div, .table-row > div {
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.table-row:last-child > div {
  border-bottom: none;
}

.table-row:nth-child(even) {
  background-color: #f2f2f2;
}

.additional-info {
  margin-top: 20px;
}

ul {
  padding-left: 20px;
}

li {
  margin-bottom: 10px;
}

@media (max-width: 768px) {
  .rules-page {
    padding: 10px;
  }

  .table-header, .table-row {
    font-size: 14px;
  }
}
</style>