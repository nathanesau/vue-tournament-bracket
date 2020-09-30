<template>
    <div class="vtb-item-players">
        <div id="popup">
            <form id="form">
                <button type="button" id="close" @mousedown="hidePopupMenu">X</button>
                <b>Hello world</b>
                <ul>
                    <li>Hello world</li>
                    <img src="https://www.thehansindia.com/images/authorplaceholder.jpg" >
                </ul>
            </form>
        </div>
        <div>
            <div id="textContainer" class="bordered">
                <button class="button" @mousedown="showPopupMenu(bracketNode)" >
                    Info
                </button>
            </div>
            <div
                :class="['vtb-player', 'vtb-player1', getPlayerClass(bracketNode.player1)]"
                @mouseover="highlightPlayer(bracketNode.player1)"
                @mouseleave="unhighlightPlayer"
                @mousedown="clickPlayer(bracketNode.player1, bracketNode.player2, bracketNode.round_num)"
            >
                <slot :player="bracketNode.player1" name="player" />
            </div>

            <div
                :class="['vtb-player', 'vtb-player2', getPlayerClass(bracketNode.player2)]"
                @mouseover="highlightPlayer(bracketNode.player2)"
                @mouseleave="unhighlightPlayer"
                @mousedown="clickPlayer(bracketNode.player2, bracketNode.player1, bracketNode.round_num)"
            >
                <slot :player="bracketNode.player2" name="player" />
            </div>
        </div>
        <slot name="player-extension-bottom" :match="matchProperties" />
    </div>
</template>

<script>
    export default {
        name: "game-players",
        props: ["bracketNode", "highlightedPlayerId"],
        computed: {
            matchProperties() {
                return Object.assign({}, this.bracketNode, { games: undefined, hasParent: undefined });
            }
        },
        methods: {
            showPopupMenu(bracketNode) {
                console.log(bracketNode);
                document.getElementById("popup").style.display = "block";
            },
            hidePopupMenu() {
                document.getElementById("popup").style.display = "none";
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
        }
    };
</script>

<style scoped>
.button {
    display: block;
    width: 100%;
}
body {
  text-align: center;
}
p {
    background-color: white;
}
#popup {
  display: none;
  position: absolute;
  top: 10%;
  background-color: #ffffff;
}
#form {
  position: absolute;
  color: black;
  background-color: white;
  border: 1px solid #212121;
  border-radius: 2px;
  padding: 10px;
  width: 350px;
  left: 50%;
  margin-left: -300px;
  z-index: 4;
}
#close {
  position: absolute;
  font-weight: bold;
  margin-left: 331px;
  margin-top: -11px;
  width: 30px;
  height: 27px;
}
input {
  width: 250px;
  margin-bottom: 20px;
}
.cont {
  border-radius: 2px;
  padding: 20px 50px;
  width: 350px;
  margin: 10px auto 10px auto;
  text-align: left;
  overflow: auto;
}
#add {
  margin: 10px;
}
</style>