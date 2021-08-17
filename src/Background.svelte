
<script>

    export let iframeUrl;

    let backgroundTerm = new URLSearchParams(location.search).get("bg_search") || "purple art";
    let backgroundUrl = new URLSearchParams(location.search).get("bg") || `https://source.unsplash.com/random?${encodeURIComponent(backgroundTerm)}`
    let opacity = 1;

    setInterval(() => {
        if (!new URLSearchParams(location.search).get("bg") && new URLSearchParams(location.search).get("bg_search")) {
            opacity = 0;
            backgroundUrl = `https://source.unsplash.com/random?${encodeURIComponent(backgroundTerm)}&${new Date().getTime()}`;
        }
    }, 30000);

</script>

<div
    class="background"
    style="opacity: {opacity}"
>

    {#if iframeUrl}
        <iframe src={iframeUrl + "?autoplay=1"} title="Video iFrame" />
    {/if}

    {#if !iframeUrl}
        <img on:load={() => opacity = 1} alt="Background" src={backgroundUrl} />
    {/if}

</div>

<style>

    .background {

        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -2;

        overflow: hidden;
        transition: opacity 0.5s;

    }

    img {
        object-fit: cover;
        width: 100%;
        height: 100%;
        filter: blur(7px);
    }

    iframe {
        width: 100%;
        height: 100%;

        transform: scale(1.01);
    }

</style>