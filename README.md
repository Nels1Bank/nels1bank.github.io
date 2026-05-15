
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nels1Rocks | Heavy Radio</title>
    <link href="https://fonts.googleapis.com/css2?family=VT323&display=swap" rel="stylesheet">
    <style>
        :root {
            --yellow: #ffcc00;
            --black: #0a0a0a;
            --dark-grey: #1a1a1a;
            --display-bg: #222;
        }

        body {
            background-color: var(--black);
            color: var(--yellow);
            font-family: 'VT323', monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        #radio-container {
            width: 400px;
            background: linear-gradient(145deg, #222, #000);
            border: 4px solid #333;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.9), inset 0 0 10px rgba(255,204,0,0.1);
            text-align: center;
        }

        h1 {
            color: var(--yellow);
            background: var(--black);
            margin: 0 0 15px 0;
            padding: 5px;
            font-size: 24px;
            letter-spacing: 4px;
            border: 1px solid var(--yellow);
            text-transform: uppercase;
        }

        .display-screen {
            background-color: var(--display-bg);
            border: 3px inset #444;
            padding: 10px;
            margin-bottom: 20px;
            height: 60px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            box-shadow: inset 0 0 15px #000;
        }

        .track-info {
            font-size: 9pt; /* Conforme instrução: Máximo 9 */
            color: #00ff41; /* Verde clássico de rádio antigo */
            text-transform: uppercase;
            white-space: nowrap;
            overflow: hidden;
            text-shadow: 0 0 5px #00ff41;
        }

        .scrolling-text {
            display: inline-block;
            animation: scroll 10s linear infinite;
        }

        @keyframes scroll {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }

        .controls {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 15px;
        }

        button {
            background: #333;
            color: var(--yellow);
            border: 2px outset #555;
            padding: 8px;
            font-family: 'VT323', monospace;
            cursor: pointer;
            font-size: 14px;
        }

        button:active {
            border-style: inset;
            background: #222;
        }

        #player-hide {
            position: absolute;
            top: -9999px;
            left: -9999px;
        }

        .mixer-status {
            font-size: 8pt;
            color: #555;
            margin-top: 10px;
            text-align: left;
        }
    </style>
</head>
<body>

<div id="radio-container">
    <h1>Nels1Rocks</h1>
    
    <div class="display-screen">
        <div class="track-info">
            <span id="track-display" class="scrolling-text">CARREGANDO PLAYLIST PESADA...</span>
        </div>
    </div>

    <div class="controls">
        <button onclick="prevTrack()">PREV</button>
        <button onclick="togglePlay()" id="playBtn">PLAY</button>
        <button onclick="nextTrack()">NEXT</button>
    </div>

    <div class="mixer-status">
        [MIXER: ON] [SHUFFLE: ACTIVE] [24/7 MODE]<br>
        GENRES: THRASH/HEAVY/PUNK/MELODIC
    </div>
</div>

<!-- Iframe Oculto do YouTube API -->
<div id="player-hide">
    <div id="yt-player"></div>
</div>

<script>
    // Playlist Curada para o Nels1Rocks (Shuffle Nativo)
    const playlist = [
        {title: "Master of Puppets", band: "Metallica", id: "xnKhs2z8KwI"},
        {title: "Painkiller", band: "Judas Priest", id: "nM__lPTWThU"},
        {title: "Raining Blood", band: "Slayer", id: "z8ZqFlw6hYg"},
        {title: "Holy Wars", band: "Megadeth", id: "9d4ui9q7eDM"},
        {title: "Eagle Fly Free", band: "Helloween", id: "FuO3hHwQ54Q"},
        {title: "Holiday in Cambodia", band: "Dead Kennedys", id: "-KTsXHvlAJA"},
        {title: "The Number of the Beast", band: "Iron Maiden", id: "WxnN05vOuSM"},
        {title: "Ace of Spades", band: "Motorhead", id: "3mbvWn1W6pE"}
    ];

    let player;
    let currentTrackIndex = 0;

    // Embaralha a lista no início (Shuffle)
    playlist.sort(() => Math.random() - 0.5);

    function onYouTubeIframeAPIReady() {
        player = new YT.Player('yt-player', {
            height: '0',
            width: '0',
            videoId: playlist[currentTrackIndex].id,
            events: {
                'onReady': onPlayerReady,
                'onStateChange': onPlayerStateChange
            }
        });
    }

    function onPlayerReady(event) {
        updateDisplay();
    }

    function updateDisplay() {
        const track = playlist[currentTrackIndex];
        document.getElementById('track-display').innerText = `${track.band} - ${track.title}`;
    }

    function onPlayerStateChange(event) {
        if (event.data == YT.PlayerState.ENDED) {
            nextTrack();
        }
    }

    function togglePlay() {
        const btn = document.getElementById('playBtn');
        if (player.getPlayerState() === 1) {
            player.pauseVideo();
            btn.innerText = "PLAY";
        } else {
            player.playVideo();
            btn.innerText = "STOP";
        }
    }

    function nextTrack() {
        currentTrackIndex = (currentTrackIndex + 1) % playlist.length;
        player.loadVideoById(playlist[currentTrackIndex].id);
        updateDisplay();
    }

    function prevTrack() {
        currentTrackIndex = (currentTrackIndex - 1 + playlist.length) % playlist.length;
        player.loadVideoById(playlist[currentTrackIndex].id);
        updateDisplay();
    }

    // Carrega a API do Google YouTube
    var tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    var firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
</script>

</body>
</html>
