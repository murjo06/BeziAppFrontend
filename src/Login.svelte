<script lang="ts">
    import Paper from "@smui/paper";
    import Textfield from "@smui/textfield";

    import Icon from '@smui/textfield/icon';
    import Button, {Label} from "@smui/button";
    import IconButton from "@smui/icon-button";

    import { navigate, Link } from "svelte-navigator";

    import Snackbar, {Actions} from "@smui/snackbar";
    import type { SnackbarComponentDev } from '@smui/snackbar';
    import * as constants from "./constants";
    import Cookies from "js-cookie";

    async function login() {
        let fd = new FormData();
        fd.append("username", email);
        fd.append("password", password);

        localStorage.setItem(`${loginType}_username`, email);
        localStorage.setItem(`${loginType}_password`, password);

        try {
            let r = await fetch(`${constants.baseurl}/${loginType}/login`, {body: fd, method: "POST", headers: {"Authorization": loginType === "lopolis" ? Cookies.get("key") : ""}})
            let response = await r.json();
            if (r.status === 200) {
                if (loginType === "gimsis") {
                    Cookies.set("key", response["session"], {sameSite: "strict"});
                    navigate("/")
                } else {
                    navigate("/lopolis");
                }
            } else {
                snackbarWithClose.open();
            }
        } catch (e) {
            console.log(e)
            snackbarWithClose.open();
        }
    }

    let url = localStorage.getItem("baseurl")

    let email = "";
    let password = "";

    export let loginType = "gimsis";

    let snackbarWithClose: SnackbarComponentDev;
</script>

<style>
    .center {
        margin: 0;
        position: absolute;
        top: 50%;
        left: 50%;
        width: 35%;
        -ms-transform: translate(-50%, -50%);
        transform: translate(-50%, -50%);
        text-align: center;
    }
</style>

<main>
    <Snackbar bind:this={snackbarWithClose}>
        <Label>Login failed.</Label>
        <Actions>
            <IconButton class="material-icons" title="Dismiss">close</IconButton>
        </Actions>
    </Snackbar>
    <div class="center">
        <Paper>
            <h1>Prijava v {#if loginType === "gimsis"}BežiApp (GimSIS){:else}Lo.Polis{/if}</h1>
            <Textfield type="text" bind:value={email} label="Uporabniško ime">
                <Icon class="material-icons" slot="leadingIcon">person</Icon>
            </Textfield>
            <p />
            <Textfield on:keypress={(key) => {
                if (key.key === "Enter") {
                    login();
                }
            }} type="password" bind:value={password} label="Geslo">
                <Icon class="material-icons" slot="leadingIcon">password</Icon>
            </Textfield>
            <p />
            <Button on:click={async () => await login()} variant="raised">
                <Label>PRIJAVA</Label>
            </Button>
            <p />
            {#if constants.isTauri}
                <Textfield on:change={() => {
                    setTimeout(() => {
                        localStorage.setItem("baseurl", url);
                        constants.baseurl = url;
                    }, 200)
                }} type="url" bind:value={url} label="Povezava do strežnika">
                    <Icon class="material-icons" slot="leadingIcon">link</Icon>
                </Textfield>
            {/if}
        </Paper>
    </div>
</main>
