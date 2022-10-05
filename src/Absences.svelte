<script lang="ts">
    import {makeRequest} from "./constants";
    import IconButton from "@smui/icon-button";
    import {onMount} from "svelte";
    import {navigate} from "svelte-navigator";
    import Accordion, { Panel, Header, Content } from '@smui-extra/accordion';
    import Select, { Option } from '@smui/select';
    import Button, {Icon, Label} from "@smui/button";
    import insane from "insane";
    import FormField from "@smui/form-field";
    import Switch from "@smui/switch";
    import Datepicker from "svelte-calendar-resurrected";
    import dayjs from "dayjs";

    let first = dayjs(new Date()).subtract(1, "month").toDate();
    let last = new Date();
    let startStore;
    let endStore;

    let niObdelano = true;
    let opraviceno = true;
    let neopraviceno = true;
    let neSteje = true;

    let absences;
    let open = {};

    const datePickerTheme = {
        "calendar": {
            "width": "300px",
            "maxWidth": "100vw",
            "legend": {
                "height": "45px"
            },
            "shadow": "0px 10px 26px rgba(0, 0, 0, 0.25)",
            "colors": {
                "text": {
                    "primary": "#eee",
                    "highlight": "#fff"
                },
                "background": {
                    "primary": "#333",
                    "highlight": "#5829d6",
                    "hover": "#222"
                },
                "border": "#222"
            },
            "font": {
                "regular": "1.5em",
                "large": "37em"
            },
            "grid": {
                "disabledOpacity": ".5",
                "outsiderOpacity": ".7"
            }
        }
    };

    async function getAbsences() {
        absences = await makeRequest(`/absences?from_date=${startStore}&to_date=${endStore}&ni_obdelano=${niObdelano}&opraviceno=${opraviceno}&neopraviceno=${neopraviceno}&ne_steje=${neSteje}`)
        for (let i in absences.absences) {
            open[i] = [];
        }
    }

    onMount(async () => {
        try {
            setTimeout(getAbsences, 200);
        } catch (e) {
            console.log(e)
            navigate("/login")
        }
    })
</script>

<div style="display: flex;">
    <h3>Izberite začetni datum: </h3>
    <Datepicker bind:formattedSelected={startStore} startOfWeekIndex={1} selected={first} format={"#{j}.#{m}.#{Y}"} theme={datePickerTheme} />
</div>
<div style="display: flex;">
    <h3>Izberite končni datum: </h3>
    <Datepicker bind:formattedSelected={endStore} startOfWeekIndex={1} selected={last} format={"#{j}.#{m}.#{Y}"} theme={datePickerTheme} />
</div>
<FormField>
    <Switch bind:checked={niObdelano} />
    <span slot="label">Ni obdelano</span>
</FormField><br>
<FormField>
    <Switch bind:checked={opraviceno} />
    <span slot="label">Opravičeno</span>
</FormField><br>
<FormField>
    <Switch bind:checked={neopraviceno} />
    <span slot="label">Ne opravičeno</span>
</FormField><br>
<FormField>
    <Switch bind:checked={neSteje} />
    <span slot="label">Ne šteje</span>
</FormField><p/>
<Button on:click={async () => await getAbsences()}>
    <Icon class="material-icons">refresh</Icon>
    <Label>Ponovno naložite</Label>
</Button>
{#if absences}
    <Accordion>
        {#each Object.entries(absences.absences) as [k, v]}
            <h2>{k}</h2>
            {#each v as absence, i}
                <Panel bind:open={open[k][i]}>
                    <Header>
                        {absence.predmet} - {absence.ura} - {absence.status}
                        <IconButton slot="icon" toggle pressed={open[k][i]}>
                            <Icon class="material-icons" on>expand_less</Icon>
                            <Icon class="material-icons">expand_more</Icon>
                        </IconButton>
                    </Header>
                    <Content>
                        {absence.opomba}
                    </Content>
                </Panel>
            {/each}
        {/each}
    </Accordion>
{/if}
