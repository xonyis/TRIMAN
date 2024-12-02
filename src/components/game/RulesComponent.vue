<template>
    <div class="rule-container" v-if="result">
        <p v-if="rules.rule1">{{ rules.rule1 }}</p>
        <p v-if="rules.rule2">{{ rules.rule2 }}</p>
    </div>
</template>
<script>
import { RulesEnum } from '@/enums/RulesEnum';


export default {
    data() {
        return {
            rules: {},
            whoTurn:null
        }    
    },
    props: {
        result: {
          type: Object,
          required: true, // Le prop result est obligatoire
        },
        whoTurn: {
          type: String,
          required: true, // Le prop result est obligatoire
        },
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
  },
    methods: {
        handelResult() {
            this.rule = {}
            if (this.result) {
                console.log(this.result);
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
                    this.$emit('isRule', false)

                }
                
            }
            
        }
    }
}
</script>
<style scoped>
div {
    text-align: center;
    font-size: 1.2em;
    position: relative;
    top: -5px;
    color: var(--white-text);   
}

</style>