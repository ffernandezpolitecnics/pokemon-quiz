<template lang="">
    <div class="container">
        <h1 v-if="!pokemon">Espere por favor ...</h1>
        <div v-else="pokemon" class="card me-auto ms-auto" style="width: 24rem;">
            <div class="card-header">
                <h3 class="fw-bold">¿Quién es este pokémon?</h3>
            </div>
            <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
            <div class="card-body">
                <PokemonOptions :pokemons="pokemonArr" @selectionPokemon="checkAnswer($event)"/>
                <template v-if="showAnswer">
                    <span class="badge text-bg-primary me-2 mt-2">Aciertos {{ aciertos }} - {{ percentA }}%</span>
                    <span class="badge text-bg-danger">Errores {{ errores }} - {{ percentE }}%</span>
                    <h3 class="fade-in mt-2">{{ message }}</h3>
                    <button  @click="newGame()" class="btn btn-primary" >
                        Nuevo Juego
                    </button>
                </template>
                
            </div>
        </div>
    </div>
   
   
    
</template>
<script>

import PokemonPicture from '@/components/PokemonPicture.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';

import getPokemonOptions from '@/helpers/getPokemonOptions'


export default {
    components: {
        PokemonPicture,
        PokemonOptions
    },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
            aciertos: 0,
            errores: 0,
            percentA: 0,
            percentE: 0
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(selectedId) {
            this.showPokemon = true
            this.showAnswer = true

            if (selectedId === this.pokemon.id) {
                this.aciertos += 1
                this.message = `Correcto, ${this.pokemon.name}`
            }
            else {
                this.errores += 1
                this.message = `Oops, era ${this.pokemon.name}`
            }

            this.percentA = Math.round(this.aciertos / (this.aciertos + this.errores) * 100)
            this.percentE = 100 - this.percentA
        },
        newGame() {
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArray()
        }
    },
    mounted() {
        this.mixPokemonArray()
    },
}
</script>
