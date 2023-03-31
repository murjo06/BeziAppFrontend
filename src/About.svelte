<script lang="ts">
    import List, {Graphic, Item, Text} from "@smui/list";
    import PrimaryText from "@smui/list/src/PrimaryText";
    import SecondaryText from "@smui/list/src/SecondaryText";
    import Meta from "@smui/list/src/Meta";
    import {navigate} from "svelte-navigator";
    import AboutPerson from "./Widgets/AboutPerson.svelte";

    const developers = [
        {
            name: "Mitja Ševerkar",
            social: [
                {id: "github", name: "mytja"},
                {id: "discord", name: "mytja#4761"},
                {id: "mail", name: "mitja.severkar@gimb.org"}
            ],
            pfp: "https://avatars.githubusercontent.com/u/52399966?v=4",
            desc: "Razvijalec prenovljene verzije",
        },
        {
            name: "Anton Luka Šijanec",
            social: [
                {id: "mail", name: "anton@sijanec.eu"},
            ],
            pfp: "/asijanec.png",
            desc: "Razvijalec stare verzije & beta tester nove verzije",
        },
        {
            name: "Rok Štular",
            social: [
                {id: "instagram", name: "rstular"},
            ],
            pfp: "/rstular.png",
            desc: "Razvijalec stare verzije",
        },
    ];

    const betaTesters = [

    ];

    const thanks = [

    ];

    const repos = [
        {
            name: "Frontend (spletna aplikacija)",
            desc: "Frontend je spisan v Svelte frameworku",
            url: "https://github.com/mytja/BeziAppFrontend",
        },
        {
            name: "Backend (zaledni del)",
            desc: "Backend je spisan v Pythonu s FastAPI enginom",
            url: "https://github.com/mytja/BeziAppBackend",
        },
        {
            name: "docker-compose",
            desc: "docker-compose skrbi za to, da je vzdrževanje izjemno lahko",
            url: "https://github.com/mytja/BeziAppDocker",
        },
        {
            name: "GimSisAPI",
            desc: "Knjižica za pridobivanje podatkov iz GimSIS sistema Gimnazije Bežigrad. Napisano v Pythonu.",
            url: "https://github.com/mytja/GimSisAPI",
        },
        {
            name: "Lo.Polis API",
            desc: "Knjižica za pridobivanje in pošiljanje podatkov o naročilih in odjavah iz Lo.Polis sistema. Napisano v Pythonu.",
            url: "https://github.com/mytja/lopolis-api",
        },
    ];
</script>

<h1>Razvijalci</h1>
<List class="demo-list" twoLine avatarList>
    {#each developers as developer}
        <AboutPerson developer={developer}/>
    {/each}
</List>
<h1>Beta testerji</h1>
Vsi ti so mi pomagali pri testiranju zgodnjih različic BežiAppa in predlagali nove stvari.
<List class="demo-list" twoLine avatarList>
    {#each betaTesters as developer}
        <AboutPerson developer={developer}/>
    {/each}
</List>
<h1>Razne zahvale</h1>
<List class="demo-list" twoLine avatarList>
    {#each thanks as developer}
        <AboutPerson developer={developer}/>
    {/each}
</List>
<h1>Koda in repozitoriji</h1>
Koda je prosto dostopna, saj je ta sistem odprtokoden. Večina glavnih repozitorijev je licencirana pod AGPLv3 licenco.
<List class="demo-list" twoLine>
    {#each repos as repo}
        <Item>
            <Text>
                <PrimaryText>{repo.name}</PrimaryText>
                <SecondaryText>{repo.desc}</SecondaryText>
            </Text>
            <Meta class="sameline">
                <div class="inline"><svg role="img" style="fill: #888; height: 24px; padding: 6px;" on:click={() => navigate(repo.url)} viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>GitHub</title><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg></div>
            </Meta>
        </Item>
    {/each}
</List>
