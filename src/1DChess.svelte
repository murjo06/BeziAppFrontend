<script lang="ts">
import Chess from "./Widgets/1DChessBoard.svelte";
window.addEventListener("load", function() {
        let resetButton = document.querySelector("#reset-game");
        resetButton.addEventListener("click", function() {
            resetGame();
        });
});
</script>
<Chess></Chess>
<button class="mdc-button mdc-button--raised mdc-ripple-upgraded" id="reset-game">Ponovi Igro</button>