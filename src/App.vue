<template>
    <bracket :rounds="rounds" :customizable="true">
        <template #player="{ player }">
            <span v-if="player.id === player.original_id"> {{ player.name }} </span>
            <span v-else> {{ player.name }}* </span>
        </template>
    </bracket>
</template>

<script>
    import Bracket from "./Bracket";

    const rounds = [
        //Quarter
        {
            games: [
                {
                    player1: { id: "1", original_id: "1", name: "Competitor 1", winner: true },
                    player2: { id: "2", original_id: "2", name: "Competitor 2", winner: false },
                    round_num: 1
                },
                {
                    player1: { id: "3", original_id: "3", name: "Competitor 3", winner: false },
                    player2: { id: "4", original_id: "4", name: "Competitor 4", winner: true },
                    round_num: 1
                },
                {
                    player1: { id: "5", original_id: "5", name: "Competitor 5", winner: true },
                    player2: { id: "6", original_id: "6", name: "Competitor 6", winner: false },
                    round_num: 1
                },
                {
                    player1: { id: "7", original_id: "7", name: "Competitor 7", winner: false },
                    player2: { id: "8", original_id: "8", name: "Competitor 8", winner: true },
                    round_num: 1
                }
            ]
        },
        //Semi
        {
            games: [
                {
                    player1: { id: "1", original_id: "1", name: "Competitor 1", winner: false },
                    player2: { id: "4", original_id: "4", name: "Competitor 4", winner: true },
                    round_num: 2
                },
                {
                    player1: { id: "5", original_id: "5", name: "Competitor 5", winner: false },
                    player2: { id: "8", original_id: "8", name: "Competitor 8", winner: true },
                    round_num: 2
                }
            ]
        },
        //Final
        {
            round_num: 3,
            games: [
                {
                    player1: { id: "4", original_id: "4", name: "Competitor 4", winner: false },
                    player2: { id: "8", original_id: "8", name: "Competitor 8", winner: true },
                    round_num: 3
                }
            ]
        }
    ];

    export default {
        name: "app",
        components: {
            Bracket
        },
        data() {
            return {
                rounds: rounds
            };
        }
    };
</script>
