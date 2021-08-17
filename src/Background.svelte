
<script>

    let backgroundTerm = new URLSearchParams(location.search).get("bg_search") || "rain";
    let backgroundUrl = new URLSearchParams(location.search).get("bg") || `https://source.unsplash.com/random?${encodeURIComponent(backgroundTerm)}`
    let opacity = 0;

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
    <img on:load={() => opacity = 1} alt="Background" src={backgroundUrl} />
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
        filter: blur(7px) scale(1.01);
    }

</style>