<script lang="js">
    import Test from "./Widgets/Test.svelte";
    import {SUBJECTS, YEAR_ONE} from "./constants";

    let startingYear = 2017; // leto prvega testa na BežiAppu
    let usedYears = [startingYear];
    let date = new Date();
    for(let i = 0; i < date.getFullYear() - startingYear; i++) {
        usedYears.push(startingYear + i + 1);
    }

    function revealChildren(element) {
        for(let i = 0; i < element.childElementCount; i++) {
            element.children[i].style.display = "flex";
        }
    }
    function hideChildren(element) {
        for(let i = 1; i < element.childElementCount; i++) {
            element.children[i].style.display = "none";
        }
    }
    function selectRevealHide(element) {
        if(element.parentElement.children[1].style.display == "none") {
            revealChildren(element.parentElement);
        } else {
            hideChildren(element.parentElement);
        }
    }
</script>
<style>
    .tests-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start;
    }
</style>
{#each SUBJECTS as className}
<div class="class-tests-class tests-container">
    <h1 class="class-tests-class-title" on:click={(event) => selectRevealHide(event.target)}>{className}</h1>
    {#each [1, 2, 3, 4] as year}
    <div class="class-tests-year tests-container" style="display: none;">
        <h2 class="class-tests-year-title" on:click={(event) => selectRevealHide(event.target)}>{year}. letnik</h2>
        {#each usedYears as schoolyear}
        <div class="class-tests-schoolyear tests-container" style="display: none;">
            <h2 class="class-tests-schoolyear-title" on:click={(event) => selectRevealHide(event.target)}>{schoolyear}</h2>
            <Test>
                <!-- This is for images of tests, notes, etc. -->
            </Test>
        </div>
        {/each}
    </div>
    {/each}
</div>
{/each}