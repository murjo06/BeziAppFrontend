<script lang="ts">
    import Tooltip, { Wrapper } from '@smui/tooltip';
    import { link } from "svelte-navigator";
    import isMobile from "is-mobile";
    import randomColor from "randomcolor";

    export let n;

    function getPreferredColorScheme() {
        if (window.matchMedia) {
            if(window.matchMedia('(prefers-color-scheme: dark)').matches){
                return 'dark';
            } else {
                return 'light';
            }
        }
        return 'light';
    }

    let preferred = getPreferredColorScheme();

    const mobile: boolean = isMobile();
</script>

<style>
    a { color: white; }

    .triangle {
        width: 0;
        height: 0;
        position: absolute;
        top: 0;
        border-left: 20px solid transparent;
        right: 0;
    }

    .classroom {
        position: absolute;
        bottom: 6px;
        right: 5px;
        font-size: 10px;
    }

    .substitution { border-top: 20px solid red; }
    .sharepoint-substitution { border-top: 20px solid yellow; }

    a:link { text-decoration: none; }

    a:visited { text-decoration: none; }

    a:hover { text-decoration: none; }

    a:active { text-decoration: none; }

    .pos { position: relative; }
</style>

<div style="padding: 8px; min-width: 95%" class="inline">
    <Wrapper>
        <span tabindex="0" style="background-color: {randomColor({luminosity: preferred})}; padding: 5px; display: flex; width: 100%; display: inline-block; height: 40px; text-align: left;" class="pos">
            <span style="font-size: 12px; font-weight: 700;">{n.kratko_ime}</span><br>
            <span style="font-size: 10px;">
                {#each n.profesor.split(" ") as t, i}
                    {#if n.profesor.split(" ").length === i+1 && !mobile}
                        {t[0].toUpperCase()}{t.substring(1)}
                    {:else}
                        {t[0].toUpperCase()}.
                    {/if}
                {/each}
            </span>
            <div
                    class="triangle {n.vpisano_nadomescanje ? 'substitution' : ''} {n.fixed_by_sharepoint ? 'sharepoint-substitution' : ''}"
            ></div>
            <span class="classroom">
                {#if mobile}
                    {n.ucilnica.replace("Učilnica ", "").replace("Telovadnica", "T").replace("Predavalnica", "P")}
                {:else}
                    {n.ucilnica}
                {/if}
            </span>
        </span>
        <Tooltip unbounded hideDelay={0}>
            <h1>{n.kratko_ime}</h1>
            Predmet: <b>{n.ime}</b><br>
            Profesor: <b>{n.profesor}</b><br>
            Razred: <b>{n.razred}</b><br>
            Dan: <b>{n.dan}</b><br>
            Ura: <b>{n.ura}</b><br>

            {#if n.dnevniski_zapis}
                <b>Dnevniški zapis obstaja.</b><br>
            {:else}
                <b>Dnevniški zapis NE obstaja.</b><br>
            {/if}
            {#if n.vpisano_nadomescanje}
                <b>Nadomeščanje je vpisano v GimSIS-u.</b><br>
            {/if}
            {#if n.fixed_by_sharepoint}
                <b>BežiApp je združil nadomeščanja na tej uri preko intraneta in GimSIS-a.</b><br>
                Tip izostanka profesorja: <b>{n.tip_izostanka}</b><br>
                Tip nadomeščanja: <b>{n.opis}</b>
            {/if}
        </Tooltip>
    </Wrapper>
</div>