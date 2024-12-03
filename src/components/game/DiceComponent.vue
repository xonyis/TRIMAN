<template>
    <div>
        
        <img v-if="dice.isRolling" :src="dice.gifPath" @load="gifLoaded" alt="gif lancé dés" />
        <img v-if="!dice.isRolling && dice.value" :src="dice.imgPath" alt="image finale du lancé" />
        <img v-if="!dice.isRolling && !dice.value" src="/assets/diceGif/dice-1.gif" alt="image finale du lancé" />
        <div v-if="!dice.isRolling" :style="dynamicStyle">{{ dice.value }}</div>
   </div>
</template>
<script>
export default {
    data() {
        return {
            dice : {
                gifPath:null,
                imgPath: '/assets/diceGif/dice-6.gif',
                value: null,
                isRolling: false
            }
        }
    },
    props: {
        value: {
          type: Number,
          required: true
        }
    },
    computed: {
        dynamicStyle() {
            return {
              color: this.dice.value === 3 ? 'var(--red)' : "var(--white-text)",
            }
        }
    },
    methods: {
        // Fonction pour lancer un dé à N faces
        lancerDe() {
            this.dice.value = Math.floor(Math.random() * 6) + 1;
            this.dice.isRolling = true;  // Activer le GIF animé

            // on fait une var pour le gif et une autre pour l'img, pour que lorsque le gif est terminé on mette l'img a la place 
            switch (this.dice.value) {
              case 1:
                this.dice.gifPath = '/assets/diceGif/dice-1-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-1.gif';
                break;
              case 2:
                this.dice.gifPath = '/assets/diceGif/dice-2-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-2.gif';
                break;
              case 3:
                this.dice.gifPath = '/assets/diceGif/dice-3-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-3.gif';
                break;
              case 4:
                this.dice.gifPath = '/assets/diceGif/dice-4-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-4.gif';
                break;
              case 5:
                this.dice.gifPath = '/assets/diceGif/dice-5-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-5.gif';
                break;
              case 6:
                this.dice.gifPath = '/assets/diceGif/dice-6-roll.gif';
                this.dice.imgPath ='/assets/diceGif/dice-6.gif';
                break;
              default:
                break;
            }
            
            this.$emit('value', this.dice.value)
        },
        // Méthode pour gérer la fin de l'animation du GIF
        gifLoaded() {
            setTimeout(() => {
              this.dice.isRolling = false;  // Activer le GIF animé
              // Retire le GIF après qu'il soit chargé pour ne pas le répéter
            }, 1500);     // Ajustez le délai pour correspondre à la durée du GIF (en millisecondes)
        }
    }
}
</script>
<style scoped>
img {
  width: 125px; /* Ajustez la taille du GIF */
  margin-left: -10px;
}
div {
    font-size: 1.75em;
    text-align: center;
    color: var(--vt-c-white-mute);
}
</style>