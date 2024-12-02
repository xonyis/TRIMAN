<template>
    <main>
        <div class="parent">
            <div class="div1">Player</div>
            <div class="div2">rules</div>
            <div class="div3"><DiceComponent ref="diceComponent1" @value="handleDice1"/></div>
            <div class="div4"><DiceComponent ref="diceComponent2" @value="handleDice2"/></div>
            <div class="div5">total d, gorgé {{ dice1.value }} {{ dice2.value }}</div>
            <div class="div6">Jouer
                <button @click="callRollDice">lanceer</button>
            </div>
        </div>

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
export default {
    data() {
        return {
            dice1: {
                value:null
            },
            dice2: {
                value:null
            }
        }
    },
    components: {
        DiceComponent
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
          this.$refs.diceComponent1.lancerDe();
          this.$refs.diceComponent2.lancerDe();
        }

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
    grid-template-rows: 100px repeat(2, 120px)  repeat(2, 100px);
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
    grid-area: 1 / 1 / 2 / 2;
    background: var(--blue);
   
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
    background: var(--red);
}
.div6 { 
    grid-area: 5 / 1 / 6 / 3;
    background: var(--green);
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