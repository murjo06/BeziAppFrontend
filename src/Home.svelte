<script lang="ts">
    import Timetable from "./Widgets/Timetable.svelte";
    import isMobile from "is-mobile";
    import Cookies from "js-cookie";

    const token = Cookies.get("key");
    if (token === null || token === undefined) {
        document.cookie = "";
        window.location.href = "/login";
    }

    const mobile = isMobile();
</script>

<Timetable />

