<template>
    <div class="rule-container" v-if="result">
        <p v-if="displayTurn == true">{{ players.name  }}</p>

        <p v-if="rules.rule1">{{ rules.rule1 }}</p>
        <p v-if="rules.rule2">{{ rules.rule2 }}</p>
        <p v-if="isChange">{{ rules.change }}</p>

    </div>
    <div class="rule-container" v-if="!result">
        <p>Player 1</p>
        <p>A toi de jouer !</p>
    </div>
</template>
<script>
import { RulesEnum } from '@/enums/RulesEnum';


export default {
    data() {
        return {
            rules: {},
            isChange: false,
            displayTurn: true,
            newtrimman: false
        }    
    },
    props: {
        result: {
          type: Object,
          required: true, // Le prop result est obligatoire
        },
        players: {
          type: Object,
          required: true, // Le prop result est obligatoire
        },
        changeMessage: {
            type: String,
        },
        isTriman: {
            type: Boolean,
            required: true
        }
  },
  watch: {
    // Watcher sur le prop `result`
    result: {
      handler() {
        this.handelResult()
      },
      immediate: true, // Si vous voulez que le watcher se déclenche dès le montage du composant
      deep: true, // Permet de surveiller les propriétés imbriquées de `result`
    },
    changeMessage: {
        handler() {
            this.rules.change = this.changeMessage;
      },
      immediate: true, // Si vous voulez que le watcher se déclenche dès le montage du composant
      deep: true,
    }
  },
    methods: {
        handelResult() {
            this.displayTurn = true
            
            this.rule = {rule1 : '', rule2 : ''}
            

            if (this.result) {



                if (this.isTriman === false) {
                    if (this.result.de1 === 3 || this.result.de2 === 3 || this.result.total === 3) {
                        this.rules.rule1 = this.players.name + ' est triman'
                        
                        this.$emit('isRule', false)
                        this.isChange = true
                        this.displayTurn = false
                        this.newtrimman = true
                        this.$emit('triman', this.players, this.newtrimman )
                       
                    } else {
                        this.isChange = false
                        this.$emit('isRule', false)
                        this.rules.rule1 = 'On cherche un triman'
                    
                    }                    
                }
                
                
                if(this.isTriman === true) {
                    this.isChange = false
                    if (this.result.de1 === 3 || this.result.de2 === 3 || this.result.total === 3 ) {
                    this.rules.rule2 = RulesEnum.TRIMAN_BOIT.rule
                }
                if ( this.result.de1 === this.result.de2) {
                    this.rules.rule1 = `${RulesEnum.DOUBLE.rule} ${this.result.de1} gorgées`
                }
                if (this.result.de1 === 3 && this.result.de2  === 3 ) {
                    this.rules.rule1 = 'Le triman boit 2 fois'
                    this.rules.rule2 = `${RulesEnum.DOUBLE.rule} ${this.result.de1} gorgées`
                }
                if (this.result.de1 === 5 && this.result.de2  === 5 ) {
                    this.rules.rule1 = RulesEnum.TU_BOIS.rule
                    this.rules.rule2 = `${RulesEnum.DOUBLE.rule} ${this.result.de1} gorgées`
                } 
                if (this.result.total === 9 ) {
                    this.rules.rule1 = RulesEnum.GAUCHE_BOIT.rule
                    if (this.result.de1 === 3 || this.result.de1 === 2 ) {
                        this.rules.rule2 = RulesEnum.TRIMAN_BOIT.rule
                    }
                } 
                if (this.result.total === 10 ) {
                    this.rules.rule1 = RulesEnum.TU_BOIS.rule
                    if (this.result.de1 === 3 || this.result.de1 === 2 ) {
                        this.rules.rule2 = RulesEnum.TRIMAN_BOIT.rule
                    }
                } 
                if (this.result.total === 11 ) {
                    this.rules.rule1 = RulesEnum.DROITE_BOIT.rule
                    if (this.result.de1 === 3 || this.result.de1 === 2 ) {
                        this.rules.rule2 = RulesEnum.TRIMAN_BOIT.rule
                    }
                } if(!this.rules.rule2 && !this.rules.rule1) {
                    this.isChange = true
                    this.displayTurn = false
                    this.$emit('isRule', false)
                }
                }
                
            }
            
        },
        updateRule() {
            
            this.rules.rule1 = this.changeMessage; // Met à jour rule1 avec le message du parent
        }
    }
}
</script>
<style scoped>
div {
    height: 100%;
    text-align: center;
    font-size: 1.2em;
    position: relative;
    top: -5px;
    color: var(--white-text);   
    display: flex; 
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

</style>