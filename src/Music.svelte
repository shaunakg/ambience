<script>

    let videoURL = "https://www.youtube.com/watch?v=tNkZsRW7h2c";
    let error = false;
    let showVideo = true;

    const getEmbedFromVideo = v => v.replace("watch?v=", "embed/") + "?autoplay=1";

    const setVideoURL = e => {

        if (e.key === "Enter") {

            try {

                const testUrl = new URL(e.target.value);

                if (![
                    "www.youtube.com",
                    "youtube.com",
                    "youtu.be",
                ].includes(testUrl.hostname)) { throw new Error("Invalid URL"); }

            } catch (e) {
                return error = true;
            }

            videoURL = e.target.value;

        }
    }

</script>

<div class="music-div" style="background-color: {showVideo ? 'white':'#ADF6B1'}">

    <h2>Music & media</h2>
    {#if error}
        <b class="error">Unable to load video</b><br/>
    {/if}

    <iframe
        title="Embedded video"
        src={getEmbedFromVideo(videoURL)}
        frameBorder="0"
        allowFullScreen
        on:load={() => error = false}
        on:error={() => error = true}
        style="display: {showVideo ? 'unset':'none'}"

    /><br/>

    <input style="border-top: {showVideo ? 'none':'2px solid gray'}" class="video" placeholder="YouTube video URL" type="url" value={videoURL} on:keyup={setVideoURL} /><br/>
    <label style="margin-top:15px;display: block;"><input type="checkbox" bind:checked={showVideo}> show video</label>

</div>

<style>
    .music-div {
		color: #333;
		padding: 20px;
		border-radius: 5px;
		background: white;
        box-shadow: 10px 10px 103px -50px rgba(0,0,0,0.75);
        transition: 0.25s;
	}

    b.error {
        display: block;
        background-color: coral;
        color: white;
        background-color: 10px;
        padding: 10px;
        border-radius: 3px;
    }

    input {
        background-color: transparent;
    }

    input.video {
        width: 100%;
        padding: 10px;

        border: 2px solid gray;
        border-top: none;
        margin-top: -5px;

        border-radius: 3px;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }

    iframe {
        width: 450px;
        height: 250px;
        margin-bottom: -2px;
        border-radius: 3px;

        border-bottom-left-radius: 0;
        border-bottom-right-radius: 0;
    }

    h2 {
        margin-top: 0;
        margin-bottom: 5px;
    }

</style>