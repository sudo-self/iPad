<script>
    import { crossfade, scale } from 'svelte/transition';
    import images from './images.js';

    const [send, receive] = crossfade({
        duration: 200,
        fallback: scale
    });

    let selected = null;
    let loading = null;
    let isDarkMode = true; // Default to dark mode

    const ASSETS = ''; // No need for ASSETS constant if images are in the public folder

    const load = (image) => {
        const timeout = setTimeout(() => (loading = image), 100);

        const img = new Image();

        img.onload = () => {
            selected = image;
            clearTimeout(timeout);
            loading = null;
        };

        img.src = `/${image.id}.png`; // Adjusted to load PNG images from the public folder
    };

    const toggleDarkMode = () => {
        isDarkMode = !isDarkMode;
    };
</script>

<div class="container" class:dark-mode={isDarkMode}>
    <div class="phone">
        <h1>JJ</h1>

        <div class="grid">
            {#each images as image}
                <div class="square">
                    {#if selected !== image}
                        <button
                            style="background-color: {image.color};"
                            on:click={() => load(image)}
                            in:receive={{ key: image.id }}
                            out:send={{ key: image.id }}>{loading === image ? '...' : image.id}</button
                        >
                    {/if}
                </div>
            {/each}
        </div>

        <button class="toggle-mode" on:click={toggleDarkMode}>
            {isDarkMode ? "Color Mode" : "jj-junior.vercel.app"}
        </button>

        {#if selected}
            {#await selected then d}
                <div class="photo" in:receive|global={{ key: d.id }} out:send|global={{ key: d.id }}>
                    <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
                    <img alt={d.alt} src={`/${d.id}.png`} on:click={() => (selected = null)} /> <!-- Adjusted to load PNG images from the public folder -->

                    <p class="credit">
                        <a target="_blank" rel="noreferrer" href="https://jessejesse.com"
                            >JesseJesse.com</a
                        >
                        <a target="_blank" rel="noreferrer" href={d.license.url}>{d.license.name}</a>
                    </p>
                </div>
            {/await}
        {/if}
    </div>
</div>

<style>
    .container {
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
    }

    .phone {
        position: relative;
        display: flex;
        flex-direction: column;
        width: 52vmin;
        height: 76vmin;
        border: 2vmin solid #ccc;
        border-bottom-width: 10vmin;
        padding: 3vmin;
        border-radius: 2vmin;
    }

    h1 {
        font-weight: 300;
        text-transform: uppercase;
        font-size: 5vmin;
        margin: 0.2em 0 0.5em 0;
    }

    .grid {
        display: grid;
        flex: 1;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(4, 1fr);
        grid-gap: 2vmin;
    }

    button {
        border-radius: 8px;
        border: 1px solid transparent;
        padding: 0.6em 1.2em;
        font-size: 1em;
        font-weight: 500;
        font-family: inherit;
        background-color: #1a1a1a;
        cursor: pointer;
        transition: border-color 0.25s;
    }

    .toggle-mode {
        margin-top: 1em;
    }

    .photo,
    img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

    .photo {
        display: flex;
        align-items: stretch;
        justify-content: flex-end;
        flex-direction: column;
        will-change: transform;
    }

    img {
        object-fit: cover;
        cursor: pointer;
    }

    .credit {
        text-align: right;
        font-size: 2.5vmin;
        padding: 1em;
        margin: 0;
        color: white;
        font-weight: bold;
        opacity: 0.6;
        background: rgba(0, 0, 0, 0.4);
    }

    .credit a,
    .credit a:visited {
        color: white;
    }

    /* Dark mode styles */
    .dark-mode {
        color: #213547;
        background-color: #ffffff;
    }
    .dark-mode a:hover {
        color: #747bff;
    }
   .dark-mode button {
        background-color: #1a1a1a; /* Adjusted background color */
        color: #ffffff; /* Adjusted text color */
        
    }
    
    @media only screen and (max-width: 600px) {
        /* Mobile styles */
        :root {
            /* Light mode */
            color: #213547;
            background-color: #ffffff;
        }

        a:hover {
            color: #747bff;
        }

        button {
            background-color: #f9f9f9;
        }

        /* Dark mode */
        .dark-mode button {
            background-color: #1a1a1a; /* Adjusted background color */
            color: #ffffff; /* Adjusted text color */
        }
    }
</style>
