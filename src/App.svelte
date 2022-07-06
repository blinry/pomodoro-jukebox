<script>
    let tracks = []
    let current

    fetch("https://chaos.social/api/v1/timelines/tag/pomodoromusic?limit=2000")
        .then((response) => response.json())
        .then((data) => {
            tracks = data
                .filter(
                    (track) =>
                        track.card &&
                        track.card.url &&
                        track.card.url.includes("youtube.com"),
                )
                .map((track) => {
                    track.card.id = track.card.url.split("=")[1]
                    track.created_at = track.created_at.split("T")[0]
                    return track
                })
        })
</script>

<h1>The #pomodoromusic Jukebox 🍅🎶</h1>
<div id="intro">
    Use this for dancing inspiration during short work breaks!<br />
    Add to the playlist by tooting using the hashtag "#pomodoromusic"!<br />
    Whipped up by
    <a href="https://chaos.social/@blinry">@blinry@chaos.social</a>
    in an hour. If you wanna improve this project, see the
    <a href="https://github.com/blinry/pomodoro-jukebox" target="_blank"
        >source code!</a
    >
</div>
<main>
    {#each tracks as track}
        <div
            class="track"
            class:current={current === track}
            on:click={() => {
                current = track
            }}
        >
            <div class="content">
                <div class="author">@{track.account.acct}</div>
                <div>{track.card.title}</div>
                <div class="fill" />
                <div>{track.created_at}</div>
            </div>
            {#if current && current === track}
                <div id="player">
                    <iframe
                        src="https://www.youtube.com/embed/{current.card
                            .id}?feature=oembed&autoplay=1"
                        width="100%"
                        height="100%"
                        allow="autoplay"
                        frameborder="0"
                    />
                </div>
            {/if}
        </div>
    {/each}
</main>

<style>
    main {
        display: flex;
        gap: 1rem;
        flex-direction: column;
    }
    #intro {
        font-size: 120%;
        margin-bottom: 2rem;
        line-height: 1.8;
    }
    #player {
        height: 20rem;
    }
    iframe {
        width: 100%;
        height: 100%;
    }
    .author {
        font-weight: bold;
    }
    .track {
        background: #eee;
        padding: 1rem;
        display: flex;
        flex-direction: column;
        gap: 1rem;
        cursor: pointer;
        max-width: 100rem;
    }
    .content {
        display: flex;
        gap: 1rem;
    }
    .current {
        background: #ddd;
    }
    .track:hover {
        background: #ddd;
    }
    .fill {
        flex: 1;
    }
</style>
