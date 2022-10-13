<script lang="ts">
    import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';
    import {makeRequest} from "./constants";
    import Grade from "./Widgets/Grade.svelte";

    let open = false;
    let grades = [];

    async function getGrades() {
        let r = await makeRequest("/grades");
        grades = r["grades"];
    }

    getGrades();
</script>

<DataTable table$aria-label="People list" style="width: 100%;">
    <Head>
        <Row>
            <Cell>Predmet</Cell>
            <Cell>1. ocenjevalno obdobje</Cell>
            <Cell>2. ocenjevalno obdobje</Cell>
            <Cell>Spomladanski izpitni rok</Cell>
            <Cell>Jesenski izpitni rok</Cell>
        </Row>
    </Head>
    <Body>
        {#each grades as subject}
            <Row>
                <Cell class="sameline">
                    <div style="display:inline-block;">{subject.name}</div>
                    <!--<div style="display:inline-block; font-size: 20px; float:right; color: gray;">{user.Average.toFixed(2)}</div>-->
                </Cell>
                <Cell class="sameline">
                    {#each subject[0] as grade}
                        <Grade grade={grade} />
                    {/each}
                </Cell>
                <Cell class="sameline">
                    {#each subject[1] as grade}
                        <Grade grade={grade} />
                    {/each}
                </Cell>
                <Cell class="sameline">
                    {#each subject[2] as grade}
                        <Grade grade={grade} />
                    {/each}
                </Cell>
                <Cell class="sameline">
                    {#each subject[3] as grade}
                        <Grade grade={grade} />
                    {/each}
                </Cell>
            </Row>
        {/each}
    </Body>
</DataTable>