<template>
    <div class="vtb-item" v-if="playersArePresent">
        <div :class="getBracketNodeClass(bracketNode)">
            <game-players
                :bracket-node="bracketNode"
                :highlighted-player-id="highlightedPlayerId"
                @onSelectedPlayer="highlightPlayer"
                @onDeselectedPlayer="unhighlightPlayer"
                @onClickPlayer="clickPlayer"
            >
                <template #player="{ player }">
                    <slot name="player" :player="player" />
                </template>
                <template #player-extension-bottom="{ match }">
                    <slot name="player-extension-bottom" :match="match" />
                </template>
            </game-players>
        </div>

        <div v-if="bracketNode.games[0] || bracketNode.games[1]" class="vtb-item-children">
            <div class="vtb-item-child" v-if="bracketNode.games[0]">
                <bracket-node
                    :bracket-node="bracketNode.games[0]"
                    :highlighted-player-id="highlightedPlayerId"
                    @onSelectedPlayer="highlightPlayer"
                    @onDeselectedPlayer="unhighlightPlayer"
                    @onClickPlayer="clickPlayer"
                >
                    <template #player="{ player }">
                        <slot name="player" :player="player" />
                    </template>
                    <template #player-extension-bottom="{ match }">
                        <slot name="player-extension-bottom" :match="match" />
                    </template>
                </bracket-node>
            </div>
            <div class="vtb-item-child" v-if="bracketNode.games[1]">
                <bracket-node
                    :bracket-node="bracketNode.games[1]"
                    :highlighted-player-id="highlightedPlayerId"
                    @onSelectedPlayer="highlightPlayer"
                    @onDeselectedPlayer="unhighlightPlayer"
                    @onClickPlayer="clickPlayer"
                >
                    <template #player="{ player }">
                        <slot name="player" :player="player" />
                    </template>
                    <template #player-extension-bottom="{ match }">
                        <slot name="player-extension-bottom" :match="match" />
                    </template>
                </bracket-node>
            </div>
        </div>
    </div>
</template>

<script>
    import GamePlayers from "./GamePlayers";

    export default {
        name: "bracket-node",
        components: { GamePlayers },
        props: ["bracketNode", "highlightedPlayerId"],
        computed: {
            playersArePresent() {
                return this.bracketNode.player1 && this.bracketNode.player1;
            },
        },
        methods: {
            getBracketNodeClass(bracketNode) {
                if (bracketNode.games[0] || bracketNode.games[1]) {
                    return "vtb-item-parent";
                }

                if (bracketNode.hasParent) {
                    return "vtb-item-child";
                }

                return "";
            },
            getPlayerClass(player) {
                if (player.winner === null || player.winner === undefined) {
                    return "";
                }

                let clazz = player.winner ? "winner" : "defeated";

                if (this.highlightedPlayerId === player.id) {
                    clazz += " highlight";
                }

                return clazz;
            },
            highlightPlayer(player) {
                this.$emit("onSelectedPlayer", player);
            },
            unhighlightPlayer() {
                this.$emit("onDeselectedPlayer");
            },
            clickPlayer(player, opponent, round_num) {
                this.$emit("onClickPlayer", player, opponent, round_num);
            }
        },
    };
</script>

<style>
    .vtb-item {
        display: flex;
        flex-direction: row-reverse;
    }

    .vtb-item p {
        padding: 20px;
        margin: 0;
        background-color: #999999;
    }

    .vtb-item-parent {
        position: relative;
        margin-left: 50px;
        display: flex;
        align-items: center;
    }

    .vtb-item-players {
        flex-direction: column;
        background-color: #999999;
        margin: 0;
        color: white;
    }

    .vtb-item-players .vtb-player {
        padding: 10px;
    }

    .vtb-item-players .winner {
        background-color: darkgreen;
    }

    .vtb-item-players .defeated {
        background-color: firebrick;
    }

    .vtb-item-players .winner.highlight {
        background-color: darkseagreen;
    }

    .vtb-item-players .defeated.highlight {
        background-color: indianred;
    }

    .vtb-item-parent:after {
        position: absolute;
        content: "";
        width: 25px;
        height: 2px;
        left: 0;
        top: 50%;
        background-color: gray;
        transform: translateX(-100%);
    }

    .vtb-item-children {
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    .vtb-item-child {
        display: flex;
        align-items: flex-start;
        justify-content: flex-end;
        margin-top: 10px;
        margin-bottom: 10px;
        position: relative;
    }

    .vtb-item-child:before {
        content: "";
        position: absolute;
        background-color: gray;
        right: 0;
        top: 50%;
        transform: translateX(100%);
        width: 25px;
        height: 2px;
    }

    .vtb-item-child:after {
        content: "";
        position: absolute;
        background-color: gray;
        right: -25px;
        height: calc(50% + 22px);
        width: 2px;
        top: 50%;
    }

    .vtb-item-child:last-child:after {
        transform: translateY(-100%);
    }

    .vtb-item-child:only-child:after {
        display: none;
    }
</style>
