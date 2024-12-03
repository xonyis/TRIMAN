<template>
    <main>
        <!-- Formulaire pour choisir le nombre de joueurs -->
        <div v-if="!game.nbrPlayer" class="form-container">
            <HelloWorld msg="Triman !" />
          <h2>Choisissez le nombre de joueurs</h2>
          <form @submit.prevent="setNumberOfPlayers">
            <label for="nbrPlayer">Nombre de joueurs (1-8) :</label>
            <input
              type="number"
              id="nbrPlayer"
              v-model.number="playerInput"
              min="2"
              max="8"
              required
            /> <br>
            <label for="nbrPlayer">Règles ignorance:</label>
            <input type="checkbox" v-model="game.ignorance" id="checkboxIgnorance">
            <button type="submit">Commencer</button>
          </form>
        </div>

        <div class="parent" v-else>
            <div class="div1">{{game.players[playerTurn].name}}:<RulesComponent v-if="!isCooldown && !game.ignorance" @isRule="changePlayer" :result="result"/>
            </div>
            
            <div class="div3"><DiceComponent ref="diceComponent1" @value="handleDice1"/></div>
            <div class="div4"><DiceComponent ref="diceComponent2" @value="handleDice2"/></div>
            <div class="div5">
                <span>Total :</span>
                <p v-if="!isCooldown && totalDice" :style="dynamicStyleDice">{{ totalDice }}</p>
                
            </div>
            <div class="div6" @click="callRollDice" :disabled="isCooldown" 
                :style="{ background: progressBackground }">
                Jouer
            </div>
        </div>
        <!-- 8 Player max -->
        <!-- <div class="parent2">
            <div class="div12">a </div>
            <div class="div22">a </div>
            <div class="div32">a </div>
            <div class="div42">a </div>
            <div class="div52">a </div>
        </div> -->
    </main>
</template>
<script>
import DiceComponent from '@/components/game/DiceComponent.vue';
import RulesComponent from '@/components/game/RulesComponent.vue';
import HelloWorld from '../components/HelloWorld.vue'
// AJOUTER PLAYER N C43EST TON TOUR AU CHANGMT | ET METTRE LES COULEUR | LANCEMENT v1
// aJOUT STAT PLUS NBR DE MANCHE DANS LES PARAMS AVANCER AU LANCEMENT DE LA PARTIE | CHOIX NOM | SAVE POUR LES | LANCEMENT v1
export default {
    data() {
        return {
            game: {
                nbrPlayer: null,
                players: [],
                ignorance: false
            },
            dice1: {
                value:null
            },
            dice2: {
                value:null
            },
            isCooldown: false, // État de cooldown
            progress: 0, // Progression de la barre
            cooldownTime: 1500, // Temps de cooldown en millisecondes
            interval: null,
            playerInput: null,
            playerTurn: 0
        }
    },
    computed: {
    progressBackground() {
      // Crée une superposition sombre en fonction de la progression
      return `linear-gradient(to right, rgba(0, 198, 94, 0.6) ${this.progress}%, rgba(0, 189, 94, 1) ${this.progress}%)`;
    },
    totalDice() {
        return this.dice1.value + this.dice2.value 
    },
    dynamicStyleDice() {
        return {
          color: this.totalDice === 3 ? 'var(--red)' : "var(--white-text)",
          lineHeight : '1em'
        }
    }
  },
    components: {
        DiceComponent,
        RulesComponent,
        HelloWorld
    },
    methods: {
        handleDice1(value) {
          this.dice1.value = value
        },
        handleDice2(value) {
          this.dice2.value = value
        },
        callRollDice() {
          // Accéder à la méthode rollDice dans l'enfant
          if (this.isCooldown != true) {
            this.startCooldown()
            this.$refs.diceComponent1.lancerDe();
            this.$refs.diceComponent2.lancerDe();
          }
          this.result = {
                total: this.totalDice,
                de1: this.dice1.value,
                de2: this.dice2.value
            }
          return this.result
        },
        startCooldown() {
            this.isCooldown = true;
            this.progress = 0;
            this.startTime = performance.now();
            
            const animate = (time) => {
              const elapsed = time - this.startTime;
              this.progress = Math.min((elapsed / this.cooldownTime) * 100, 100);
            
              if (elapsed < this.cooldownTime) {
                requestAnimationFrame(animate);
              } else {
                this.isCooldown = false; // Réactiver le bouton une fois terminé
                this.progress = 0;
              }
            };
        
            requestAnimationFrame(animate);
            
        },
        changePlayer() {
            if (this.playerTurn < this.game.players.length) {
                this.playerTurn++
                if (this.playerTurn === this.game.players.length) {
                    this.playerTurn = 0
                }
            }
        },
        setNumberOfPlayers() {
          if (this.playerInput >= 1 && this.playerInput <= 8) {
            this.game.nbrPlayer = this.playerInput; // Met à jour le nombre de joueurs
            
            // Créer le tableau des joueurs
            this.game.players = Array.from({ length: this.playerInput }, (v, i) => ({
                id: i + 1,
                name: `Player ${i + 1}`,
              }));
              
            } else {
              alert("Le nombre de joueurs doit être entre 1 et 8.");
            }
            console.log(this.game.ignorance);
            
        },
    }
    
}
</script>
<style scoped>
main {
    margin-top: 3rem;
}

.parent {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 100px repeat(2, 120px)  120px 100px;
    grid-column-gap: 20px;
    grid-row-gap: 20px;
    height: 650px;
}


.parent > div {
    /* border: 1px solid; */
    box-shadow: 0px 0px 10px 0px rgba(255,255,255,0.12);
    border-radius: 15px;
    padding: 1rem;
    border: .75px solid rgba(255, 255, 255, 0.178);
}

.div1 { 
    grid-area: 1 / 1 / 2 / 3;
    background: var(--blue);
    color: var(--white-text);   

}
.div2 { 
    grid-area: 1 / 2 / 2 / 3;
    background: var(--yellow);
}
.div3 { 
    grid-area: 2 / 1 / 4 / 2;
    
}
.div4 { 
    grid-area: 2 / 2 / 4 / 3;

}
.div5 { 
    grid-area: 4 / 1 / 5 / 3;
    display: flex;
    
    flex-direction:column ;
    background: var(--yellow);
}
.div5 p{ 
    font-size: 3em;
    color: var(--white-text);
    text-align: center;
    width: 100%;
}
.div5 span{ 
    font-size:16px;
    color: var(--white-text);
    position: relative;
    left: 0px;
    
   
}
.div6 { 
    grid-area: 5 / 1 / 6 / 3;
    display: flex;
    justify-content: center;
    align-items: center;
    color: var(--white-text);
    font-size: 2em;
    background: var(--green);
}

body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
  margin: 0;
}

.button-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

button {
  position: relative;
  padding: 10px 20px;
  font-size: 18px;
  color: #fff;
  background-color: #007bff; /* Couleur de base */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s;
  overflow: hidden;
}

button:disabled {
  cursor: not-allowed;
}


/* .parent2 {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 100px repeat(2, 120px)  repeat(2, 100px);
    grid-column-gap: 20px;
    grid-row-gap: 20px;
    height: 650px;
}

.parent2  div {
    border: 1px solid;
    box-shadow: 0px 0px 10px 0px rgba(255,255,255,0.12);
    border-radius: 15px;
    padding: 1rem;
    border: .75px solid rgba(255, 255, 255, 0.178);
}

.div12 {
    grid-area: 1 / 1 / 2 / 3; 
    background: var(--blue);
}
.div22 {
    grid-area: 2 / 1 / 4 / 2; }
.div32 {
    grid-area: 2 / 2 / 4 / 3; }
.div42 {
    grid-area: 4 / 1 / 5 / 3;
    background: var(--red);
}
.div52 {
    grid-area: 5 / 1 / 6 / 3; 
    background: var(--green);
} */
</style>