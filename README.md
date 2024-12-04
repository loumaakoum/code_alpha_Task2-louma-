//index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Music Player</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="music-player">
    <div class="music-info">
      <h2 id="title">Song Title</h2>
      <h3 id="artist">Artist Name</h3>
    </div>
    <audio id="audio" src="song1.mp3"></audio>
    <div class="controls">
      <button id="prev" class="btn">⏮️</button>
      <button id="play" class="btn">▶️</button>
      <button id="next" class="btn">⏭️</button>
    </div>
    <div class="progress-container">
      <div class="progress" id="progress"></div>
    </div>
  </div>
  <script src="script.js"></script>
</body>
</html>

//style.css
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f3f3f3;
    margin: 0;
  }
  
  .music-player {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    text-align: center;
    width: 300px;
  }
  
  .music-info {
    margin-bottom: 10px;
  }
  
  h2, h3 {
    margin: 0;
    padding: 5px 0;
  }
  
  .controls {
    display: flex;
    justify-content: center;
    margin: 10px 0;
  }
  
  .btn {
    font-size: 1.5rem;
    margin: 0 10px;
    padding: 10px;
    background: none;
    border: none;
    cursor: pointer;
  }
  
  .progress-container {
    background-color: #ddd;
    border-radius: 5px;
    height: 5px;
    position: relative;
    width: 100%;
  }
  
  .progress {
    background-color: #3498db;
    border-radius: 5px;
    height: 100%;
    width: 0;
  }

  //script.js
  const audio = document.getElementById('audio');
const playBtn = document.getElementById('play');
const prevBtn = document.getElementById('prev');
const nextBtn = document.getElementById('next');
const title = document.getElementById('title');
const artist = document.getElementById('artist');
const progressContainer = document.getElementById('progress');
const progress = document.getElementById('progress');

const songs = [
  {
    title: 'Song 1',
    artist: 'Artist 1',
    file: 'song1.mp3',
  },
  {
    title: 'Song 2',
    artist: 'Artist 2',
    file: 'song2.mp3',
  },
  {
    title: 'Song 3',
    artist: 'Artist 3',
    file: 'song3.mp3',
  }
];

let songIndex = 0;

function loadSong(song) {
  title.textContent = song.title;
  artist.textContent = song.artist;
  audio.src = song.file;
}

function playSong() {
  audio.play();
  playBtn.textContent = '⏸️';
}

function pauseSong() {
  audio.pause();
  playBtn.textContent = '▶️';
}

function prevSong() {
  songIndex = (songIndex - 1 + songs.length) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
}

function nextSong() {
  songIndex = (songIndex + 1) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
}

function updateProgress(e) {
  const { duration, currentTime } = e.srcElement;
  const progressPercent = (currentTime / duration) * 100;
  progress.style.width = `${progressPercent}%`;
}

function setProgress(e) {
  const width = this.clientWidth;
  const clickX = e.offsetX;
  const duration = audio.duration;
  audio.currentTime = (clickX / width) * duration;
}

playBtn.addEventListener('click', () => {
  const isPlaying = !audio.paused;
  if (isPlaying) {
    pauseSong();
  } else {
    playSong();
  }
});

prevBtn.addEventListener('click', prevSong);
nextBtn.addEventListener('click', nextSong);

audio.addEventListener('timeupdate', updateProgress);
progressContainer.addEventListener('click', setProgress);

audio.addEventListener('ended', nextSong);

loadSong(songs[songIndex]);

