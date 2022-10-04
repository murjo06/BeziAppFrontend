<style>
    .coolTable {
        border-spacing: 5px;
        text-align: center;
        width: 100%;
    }

    a:link {
        text-decoration: none;
    }
</style>

<script lang="ts">
    import {
        startOfWeek,
        endOfWeek
    } from "date-fns";
    import IconButton from "@smui/icon-button";
    import {makeRequest} from "../constants";
    import isMobile from "is-mobile";
    import MeetingCard from "../MeetingCard.svelte";

    export let date: Date = new Date();
    let currentDate = new Date(date);

    let start: Date = startOfWeek(currentDate, {weekStartsOn: 1})
    let end: Date = endOfWeek(currentDate, {weekStartsOn: 1})

    const mobile: boolean = isMobile();

    function remakeCalendar() {
        start = startOfWeek(currentDate, {weekStartsOn: 1})
        fmtStart = fmtDate(start);
    }

    function fmtDate(date: Date): string {
        let dd = date.getDate().toString()
        if (dd.length == 1) {
            dd = `0${dd}`
        }
        let mm = date.getMonth().toString();
        if (mm.length == 1 && (date.getMonth() + 1).toString().length == 1) {
            mm = `0${(date.getMonth() + 1)}`
        } else if ((date.getMonth() + 1).toString().length != 1) {
            mm = (date.getMonth() + 1).toString();
        }
        return `${dd} ${mm} ${date.getFullYear()}`
    }

    let fmtStart: string = fmtDate(start);

    export let classId: number;
    export let subjectId: number;
    export let teacherId: number;

    async function getTimetable() {
        console.log(fmtStart)
        console.log(classId, teacherId, subjectId)

        let r = await makeRequest(`/timetable?date=${fmtStart}`)
        mon = r["classes"][0];
        tue = r["classes"][1];
        wed = r["classes"][2];
        thu = r["classes"][3];
        fri = r["classes"][4];
        dates = r["days"];
    }

    const hours: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8];
    let dates: string[] = ["", "", "", "", "", ""]
    let mon: Meeting[] = [];
    let tue: Meeting[] = [];
    let wed: Meeting[] = [];
    let thu: Meeting[] = [];
    let fri: Meeting[] = [];
</script>

<IconButton class="material-icons" on:click={() => {
    currentDate.setDate(currentDate.getDate() - 7);
    remakeCalendar();
    getTimetable();
}}>arrow_back</IconButton>
<IconButton class="material-icons" on:click={() => {
    currentDate.setDate(currentDate.getDate() + 7);
    remakeCalendar();
    getTimetable();
}}>arrow_forward</IconButton>

{#await getTimetable()}
{:then _}
    <table class="coolTable">
        <tr>
            <th>URA</th>
            <th>{mobile ? "PON" : "PONEDELJEK"} {dates[0]}</th>
            <th>{mobile ? "TOR" : "TOREK"} {dates[1]}</th>
            <th>{mobile ? "SRE" : "SREDA"} {dates[2]}</th>
            <th>{mobile ? "ČET" : "ČETRTEK"} {dates[3]}</th>
            <th>{mobile ? "PET" : "PETEK"} {dates[4]}</th>
        </tr>
        {#each hours as m, i}
        <tr>
            <th>{i}.</th>
            <td>
                <div class="sameline">
                    {#each Array(mon[i]) as m}
                        {#if m}
                            <MeetingCard n={m} />
                        {/if}
                    {/each}
                </div>
            </td>
            <td>
                <div class="sameline">
                    {#each Array(tue[i]) as m}
                        {#if m}
                            <MeetingCard n={m} />
                        {/if}
                    {/each}
                </div>
            </td>
            <td>
                <div class="sameline">
                    {#each Array(wed[i]) as m}
                        {#if m}
                            <MeetingCard n={m} />
                        {/if}
                    {/each}
                </div>
            </td>
            <td>
                <div class="sameline">
                    {#each Array(thu[i]) as m}
                        {#if m}
                            <MeetingCard n={m} />
                        {/if}
                    {/each}
                </div>
            </td>
            <td>
                <div class="sameline">
                    {#each Array(fri[i]) as m}
                        {#if m}
                            <MeetingCard n={m} />
                        {/if}
                    {/each}
                </div>
            </td>
        </tr>
        {/each}
    </table>
{/await}