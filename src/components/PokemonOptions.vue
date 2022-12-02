<template lang="">
    <!-- <div class="options-container"> -->
    <div>
        <ul class="list-group disabled">
            <li v-for="pokemon in pokemons" :key="pokemon.id" @click="select(pokemon.id)" 
            class="list-group-item list-group-item-action"
            :class="{'active': pokemon.id == selectedId}">
                {{ pokemon.name }}
            </li>
        </ul>
    </div>
       
    <!-- </div> -->
</template>
<script>
export default {
    props: {
        pokemons: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            selectedId: null,
            isActive: true
        }
    },
    methods: {
        select(id) {
            if (this.selectedId == null) {
                this.selectedId = id
                this.$emit('selectionPokemon', id)
            }

        }
    }
}
</script>
