<script lang="ts">
    import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';
    import {makeRequest} from "./constants";
    import Grade from "./Widgets/Grade.svelte";
    import Meta from "@smui/list/src/Meta";
    import FormField from "@smui/form-field";
    import Switch from "@smui/switch";

    let grades = [];
    let total_average = 0.0;
    let stalne = false;

    async function getGrades() {
        let r = await makeRequest("/grades");
        grades = r["grades"]["subjects"];
        total_average = r["grades"]["average"];
    }

    getGrades();
</script>
<style>
    .zacasna {
        font-weight: 400 !important;
    }
</style>
<FormField>
    <Switch bind:checked={stalne} />
    <span slot="label">Stalne ocene</span>
</FormField>
<p/>
Učni uspeh: <span style="color: rgba(255, 255, 255, 0.5); display:inline-block;">{total_average.toFixed(2)}</span>
<p/>
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
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{stalne ? subject["perm_average"].toFixed(2) : subject["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[0]["grades"] as grade}
                        <Grade grade={grade} stalne={stalne} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{stalne ? subject[0]["perm_average"].toFixed(2) : subject[0]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[1]["grades"] as grade}
                        <Grade grade={grade} stalne={stalne} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{stalne ? subject[1]["perm_average"].toFixed(2) : subject[1]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[2]["grades"] as grade}
                        <Grade grade={grade} stalne={stalne} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{stalne ? subject[2]["perm_average"].toFixed(2) : subject[2]["average"].toFixed(2)}</Meta>
                </Cell>
                <Cell class="sameline">
                    {#each subject[3]["grades"] as grade}
                        <Grade grade={grade} stalne={stalne} />
                    {/each}
                    <Meta style="display:inline-block; font-size: 20px; float:right;">{stalne ? subject[3]["perm_average"].toFixed(2) : subject[3]["average"].toFixed(2)}</Meta>
                </Cell>
            </Row>
        {/each}
    </Body>
</DataTable>