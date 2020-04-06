<template>
    <div class="games">
        <app-game v-for="game in filteredGames" :key="game.appid" :game="game" class="game">
        </app-game>
    </div>
</template>

<script>
import Game from './Game.vue'
export default {
    components: {
        "app-game": Game
    },
    data: function() {
        return {
        }
    },
    methods: {

    },
    props: {
        games: {
            type: Array
        },
        matchedGames: {
            type: Array
        }
    },
    computed: {
        filteredGames(){
            let filteredGames = this.games.filter((game) => {
                return this.matchedGames.includes(game.appid.toString());
            })
            let orderedGames = filteredGames.sort((a, b) => {
                return a.name > b.name ? 1 : -1
            })
            return orderedGames;
        }
    }
}
</script>

<style>
    .games {
     display: flex;
     flex-direction: row;
     flex-wrap: wrap;
    }
</style>