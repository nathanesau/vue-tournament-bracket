<template>
    <div class="vtb-wrapper" v-if="recursiveBracket">
        <bracket-node
            :bracket-node="recursiveBracket"
            @onSelectedPlayer="highlightPlayer"
            @onDeselectedPlayer="unhighlightPlayer"
            @onClickPlayer="clickPlayer"
            :highlighted-player-id="highlightedPlayerId"
        >
            <template #player="{ player }">
                <slot name="player" :player="player" />
            </template>
            <template #player-extension-bottom="{ match }">
                <slot name="player-extension-bottom" :match="match" />
            </template>
        </bracket-node>
    </div>
</template>

<script>
    import BracketNode from "./components/BracketNode";
    import recursiveBracket from "./components/recursiveBracket";

    export default {
        name: "bracket",
        components: {
            "bracket-node": BracketNode,
        },
        props: ["rounds", "flatTree", "customizable"],
        data() {
            return {
                highlightedPlayerId: null,
            };
        },
        computed: {
            recursiveBracket() {
                if (this.flatTree) {
                    return recursiveBracket.transformFlatTree(this.flatTree);
                }

                return recursiveBracket.transform(this.rounds);
            },
        },
        methods: {
            highlightPlayer(player) {
                this.highlightedPlayerId = player.id;
            },
            unhighlightPlayer() {
                this.highlightedPlayerId = null;
            },
            clickPlayer(player, opponent, round_num) {
                if (!this.customizable) {
                    return;
                }
                
                if (opponent.winner) {
                    opponent.winner = false;
                    player.winner = true;
                }

                // modify entries in future rounds
                for (var round of this.rounds) {
                    for (var game of round.games) {

                        if (game.round_num < round_num) {
                            continue;
                        }
    
                        // update winner for current round
                        if (game.player1.id === player.id && game.player2.id === opponent.id ||
                            game.player2.id === player.id && game.player1.id === opponent.id ) {
                                
                            if (game.player1.id === opponent.id) {
                                game.player2.winner = true;
                                game.player1.winner = false;
                            } else { // game.player2.id === opponent.id
                                game.player1.winner = true;
                                game.player2.winner = false;
                            }

                            break;
                        }

                        // future matchup - replace opponent with player
                        if (game.player1.id === opponent.id) {
                            game.player1.id = player.id;
                            game.player1.name = player.name;
                        }
                        else if (game.player2.id === opponent.id) {
                            game.player2.id = player.id;
                            game.player2.name = player.name;
                        }
                    }
                }
            },
        },
    };
</script>

<style>
    .vtb-wrapper {
        display: flex;
        justify-content: center;
    }
</style>
