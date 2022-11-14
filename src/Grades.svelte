<script lang="ts">
    import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';
    import {makeRequest} from "./constants";
    import Grade from "./Widgets/Grade.svelte";
    import Meta from "@smui/list/src/Meta";

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
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{subject.average.toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[0]["grades"] as grade}
                        <Grade grade={grade} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{subject[0]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[1]["grades"] as grade}
                        <Grade grade={grade} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{subject[1]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[2]["grades"] as grade}
                        <Grade grade={grade} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{subject[2]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[3]["grades"] as grade}
                        <Grade grade={grade} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{subject[3]["average"].toFixed(2)}</Meta>
                </Cell>
            </Row>
        {/each}
    </Body>
</DataTable>