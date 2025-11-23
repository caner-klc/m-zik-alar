<!doctype html>
<html lang="tr">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Müzik Çalar Pro</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      height: 100%;
      overflow: hidden;
    }
    
    html {
      height: 100%;
    }

    * {
      box-sizing: border-box;
    }

    #app-container {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      position: relative;
    }

    /* Login Screen */
    .auth-screen {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 2000;
      animation: fadeIn 0.5s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .auth-box {
      border-radius: 16px;
      padding: 48px;
      width: 90%;
      max-width: 450px;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
      text-align: center;
      animation: slideUp 0.5s ease;
    }

    @keyframes slideUp {
      from { transform: translateY(30px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }

    .auth-logo {
      font-size: 64px;
      margin-bottom: 20px;
    }

    .auth-title {
      font-size: 32px;
      font-weight: bold;
      margin-bottom: 12px;
    }

    .auth-subtitle {
      font-size: 16px;
      opacity: 0.7;
      margin-bottom: 32px;
    }

    .auth-form {
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .auth-input {
      padding: 16px;
      border: 2px solid rgba(255, 255, 255, 0.2);
      border-radius: 8px;
      font-size: 16px;
      transition: all 0.3s ease;
    }

    .auth-input:focus {
      outline: none;
      border-color: currentColor;
    }

    .auth-button {
      padding: 16px;
      border: none;
      border-radius: 24px;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s ease;
      margin-top: 8px;
    }

    .auth-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
    }

    .auth-button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
    }

    .premium-badge {
      display: inline-block;
      padding: 4px 12px;
      border-radius: 12px;
      font-size: 12px;
      font-weight: bold;
      margin-left: 8px;
      animation: shimmer 2s infinite;
    }

    @keyframes shimmer {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }

    /* Sidebar */
    #sidebar {
      width: 250px;
      height: 100%;
      padding: 20px;
      display: flex;
      flex-direction: column;
      gap: 20px;
      position: fixed;
      left: 0;
      top: 0;
      transition: all 0.3s ease;
    }

    #app-logo {
      font-size: 28px;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }

    .user-profile {
      padding: 16px;
      border-radius: 12px;
      display: flex;
      align-items: center;
      gap: 12px;
      margin-top: auto;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .user-profile:hover {
      transform: translateX(5px);
    }

    .user-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
      flex-shrink: 0;
    }

    .user-info {
      flex: 1;
      min-width: 0;
    }

    .user-email {
      font-size: 14px;
      font-weight: 600;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .logout-text {
      font-size: 12px;
      opacity: 0.7;
    }

    .nav-item {
      padding: 12px 16px;
      border-radius: 8px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 16px;
      font-weight: 500;
      transition: all 0.3s ease;
    }

    .nav-item:hover {
      transform: translateX(5px);
    }

    .nav-item.active {
      font-weight: 700;
    }

    /* Main Content */
    #main-content {
      margin-left: 250px;
      height: 100%;
      display: flex;
      flex-direction: column;
      padding-bottom: 100px;
      transition: all 0.3s ease;
    }

    #content-header {
      padding: 30px 40px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    #content-title {
      font-size: 32px;
      font-weight: bold;
      margin: 0 0 20px 0;
    }

    #add-song-button {
      padding: 12px 24px;
      border: none;
      border-radius: 24px;
      font-size: 14px;
      font-weight: 600;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      transition: all 0.3s ease;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    #add-song-button:hover {
      transform: scale(1.05);
      box-shadow: 0 6px 16px rgba(0, 0, 0, 0.3);
    }

    #song-list-container {
      flex: 1;
      overflow-y: auto;
      padding: 20px 40px;
    }

    .song-item {
      display: flex;
      align-items: center;
      padding: 12px;
      border-radius: 8px;
      margin-bottom: 8px;
      cursor: pointer;
      transition: all 0.3s ease;
      gap: 16px;
      animation: slideIn 0.3s ease;
    }

    @keyframes slideIn {
      from { transform: translateX(-20px); opacity: 0; }
      to { transform: translateX(0); opacity: 1; }
    }

    .song-item:hover {
      transform: translateX(5px);
    }

    .song-item.playing {
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: translateX(0); }
      50% { transform: translateX(5px); }
    }

    .song-number {
      width: 30px;
      text-align: center;
      font-weight: 600;
      opacity: 0.7;
    }

    .song-album-cover {
      width: 50px;
      height: 50px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      flex-shrink: 0;
      background-size: cover;
      background-position: center;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
      transition: all 0.3s ease;
    }

    .song-item:hover .song-album-cover {
      transform: scale(1.1);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
    }

    .song-info {
      flex: 1;
      min-width: 0;
    }

    .song-name {
      font-weight: 600;
      font-size: 16px;
      margin-bottom: 4px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .song-artist {
      font-size: 14px;
      opacity: 0.7;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .song-duration {
      opacity: 0.7;
      font-size: 14px;
      margin-right: 12px;
    }

    .song-actions {
      display: flex;
      gap: 8px;
      opacity: 0;
      transition: opacity 0.3s ease;
    }

    .song-item:hover .song-actions {
      opacity: 1;
    }

    .song-action-button {
      padding: 8px 16px;
      border: none;
      border-radius: 20px;
      font-size: 12px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .song-action-button:hover {
      transform: scale(1.05);
    }

    .empty-state {
      text-align: center;
      padding: 60px 20px;
      opacity: 0.6;
      animation: fadeIn 0.5s ease;
    }

    .empty-state-icon {
      font-size: 64px;
      margin-bottom: 20px;
    }

    .empty-state-text {
      font-size: 18px;
      margin-bottom: 10px;
    }

    /* Player Bar */
    #player-bar {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 90px;
      padding: 16px 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(20px);
      transition: all 0.3s ease;
    }

    #now-playing {
      width: 250px;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    #now-playing-cover {
      width: 56px;
      height: 56px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 28px;
      flex-shrink: 0;
      background-size: cover;
      background-position: center;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
      animation: rotate 20s linear infinite paused;
    }

    .playing #now-playing-cover {
      animation-play-state: running;
    }

    @keyframes rotate {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }

    #now-playing-info {
      flex: 1;
      min-width: 0;
    }

    #now-playing-name {
      font-weight: 600;
      font-size: 14px;
      margin-bottom: 4px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    #now-playing-artist {
      font-size: 12px;
      opacity: 0.7;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    #player-controls {
      flex: 1;
      display: flex;
      flex-direction: column;
      gap: 8px;
      align-items: center;
    }

    #control-buttons {
      display: flex;
      gap: 16px;
      align-items: center;
    }

    .control-button {
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: all 0.3s ease;
      opacity: 0.8;
    }

    .control-button:hover {
      opacity: 1;
      transform: scale(1.1);
    }

    .control-button.play-button {
      font-size: 36px;
    }

    #progress-bar-container {
      width: 100%;
      max-width: 600px;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .time-display {
      font-size: 12px;
      opacity: 0.7;
      min-width: 40px;
    }

    #progress-bar {
      flex: 1;
      height: 6px;
      border-radius: 3px;
      cursor: pointer;
      position: relative;
      overflow: hidden;
    }

    #progress-fill {
      height: 100%;
      width: 0%;
      border-radius: 3px;
      transition: width 0.1s linear;
    }

    /* Modal */
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.8);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      backdrop-filter: blur(10px);
      animation: fadeIn 0.3s ease;
    }

    .modal-content {
      border-radius: 16px;
      padding: 32px;
      width: 90%;
      max-width: 450px;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
      animation: slideUp 0.3s ease;
    }

    .modal-header {
      font-size: 24px;
      font-weight: bold;
      margin-bottom: 24px;
    }

    .form-group {
      margin-bottom: 20px;
    }

    .form-group label {
      display: block;
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 8px;
    }

    .form-group input {
      width: 100%;
      padding: 12px 16px;
      border: 2px solid rgba(255, 255, 255, 0.2);
      border-radius: 8px;
      font-size: 16px;
      transition: all 0.3s ease;
    }

    .form-group input:focus {
      outline: none;
      border-color: currentColor;
    }

    .form-group-hint {
      font-size: 12px;
      opacity: 0.6;
      margin-top: 4px;
    }

    .modal-buttons {
      display: flex;
      gap: 12px;
      margin-top: 24px;
    }

    .modal-button {
      flex: 1;
      padding: 12px 24px;
      border: none;
      border-radius: 24px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .modal-button:hover {
      transform: translateY(-2px);
    }

    .modal-button.primary {
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .modal-button.secondary {
      opacity: 0.7;
    }

    .modal-button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
    }

    .hidden {
      display: none !important;
    }

    /* Scrollbar */
    ::-webkit-scrollbar {
      width: 12px;
    }

    ::-webkit-scrollbar-track {
      background: transparent;
    }

    ::-webkit-scrollbar-thumb {
      background: rgba(255, 255, 255, 0.2);
      border-radius: 6px;
    }

    ::-webkit-scrollbar-thumb:hover {
      background: rgba(255, 255, 255, 0.3);
    }

    .loading-spinner {
      display: inline-block;
      width: 16px;
      height: 16px;
      border: 3px solid rgba(255, 255, 255, 0.3);
      border-top-color: #fff;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    /* Toast Notification */
    .toast {
      position: fixed;
      bottom: 110px;
      left: 50%;
      transform: translateX(-50%);
      padding: 16px 24px;
      border-radius: 24px;
      font-size: 14px;
      font-weight: 600;
      z-index: 2000;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
      animation: toastSlide 3s ease forwards;
    }

    @keyframes toastSlide {
      0% { transform: translateX(-50%) translateY(20px); opacity: 0; }
      10%, 90% { transform: translateX(-50%) translateY(0); opacity: 1; }
      100% { transform: translateX(-50%) translateY(20px); opacity: 0; }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <main id="app-container"><!-- Login/Register Screen -->
   <div id="auth-screen" class="auth-screen">
    <div class="auth-box">
     <div class="auth-logo">
      🎵
     </div>
     <h1 class="auth-title" id="auth-title">Müzik Çalar Pro</h1>
     <p class="auth-subtitle">Müziğinizi ekleyin, dinleyin ve yönetin</p>
     <form id="auth-form" class="auth-form"><input type="email" id="email-input" class="auth-input" placeholder="E-posta adresiniz" required> <button type="submit" class="auth-button" id="auth-submit-button">Giriş Yap</button>
     </form>
    </div>
   </div><!-- Main App (Initially Hidden) -->
   <div id="main-app" class="hidden"><!-- Sidebar -->
    <aside id="sidebar">
     <div id="app-logo"><span>🎵</span> <span id="app-title">Müzik Çalar</span> <span class="premium-badge" id="premium-badge">Premium</span>
     </div>
     <nav>
      <div class="nav-item active"><span>🏠</span> <span>Ana Sayfa</span>
      </div>
      <div class="nav-item"><span>🔍</span> <span>Ara</span>
      </div>
      <div class="nav-item"><span>📚</span> <span>Kütüphanem</span>
      </div>
     </nav>
     <div class="user-profile" id="user-profile">
      <div class="user-avatar">
       👤
      </div>
      <div class="user-info">
       <div class="user-email" id="user-email">
        kullanici@mail.com
       </div>
       <div class="logout-text">
        Çıkış yap
       </div>
      </div>
     </div>
    </aside><!-- Main Content -->
    <div id="main-content">
     <header id="content-header">
      <h1 id="content-title">Müziğim</h1><button id="add-song-button"> <span>➕</span> <span id="add-button-text">Şarkı Ekle</span> </button>
     </header>
     <div id="song-list-container">
      <div id="song-list"></div>
     </div>
    </div><!-- Player Bar -->
    <div id="player-bar">
     <div id="now-playing">
      <div id="now-playing-cover">
       🎵
      </div>
      <div id="now-playing-info">
       <div id="now-playing-name">
        Şarkı seç ve dinle
       </div>
       <div id="now-playing-artist">
        -
       </div>
      </div>
     </div>
     <div id="player-controls">
      <div id="control-buttons"><button class="control-button" id="shuffle-button">🔀</button> <button class="control-button" id="prev-button">⏮️</button> <button class="control-button play-button" id="play-button">▶️</button> <button class="control-button" id="next-button">⏭️</button> <button class="control-button" id="repeat-button">🔁</button>
      </div>
      <div id="progress-bar-container"><span class="time-display" id="current-time">0:00</span>
       <div id="progress-bar">
        <div id="progress-fill"></div>
       </div><span class="time-display" id="total-time">0:00</span>
      </div>
     </div>
     <div style="width: 250px;"></div>
    </div><!-- Add Song Modal -->
    <div id="add-modal" class="modal hidden">
     <div class="modal-content">
      <div class="modal-header">
       Yeni Şarkı Ekle
      </div>
      <form id="add-song-form">
       <div class="form-group"><label for="song-name-input">Şarkı Adı</label> <input type="text" id="song-name-input" placeholder="Şarkı adını girin" required>
       </div>
       <div class="form-group"><label for="artist-name-input">Sanatçı</label> <input type="text" id="artist-name-input" placeholder="Sanatçı adını girin" required>
       </div>
       <div class="form-group"><label for="duration-input">Süre (örn: 3:45)</label> <input type="text" id="duration-input" placeholder="3:45" pattern="[0-9]:[0-5][0-9]" required>
       </div>
       <div class="form-group"><label for="album-cover-input">Albüm Kapak URL (opsiyonel)</label> <input type="url" id="album-cover-input" placeholder="https://example.com/cover.jpg">
        <div class="form-group-hint">
         URL girmezseniz emoji kullanılacak
        </div>
       </div>
       <div class="modal-buttons"><button type="button" class="modal-button secondary" id="cancel-button">İptal</button> <button type="submit" class="modal-button primary" id="save-button">Kaydet</button>
       </div>
      </form>
     </div>
    </div>
   </div>
  </main>
  <script>
    const defaultConfig = {
      background_color: "#121212",
      surface_color: "#1e1e1e",
      text_color: "#ffffff",
      primary_action_color: "#1db954",
      secondary_action_color: "#282828",
      app_title: "Müzik Çalar",
      library_title: "Müziğim",
      add_button_text: "Şarkı Ekle",
      premium_badge_text: "Premium",
      font_family: "Segoe UI",
      font_size: 16
    };

    // State
    let currentUser = null;
    let allSongs = [];
    let userSongs = [];
    let currentSongIndex = -1;
    let isPlaying = false;
    let currentTime = 0;
    let playInterval = null;
    let shuffleMode = false;
    let repeatMode = false;

    // DOM elements
    const authScreen = document.getElementById('auth-screen');
    const mainApp = document.getElementById('main-app');
    const authForm = document.getElementById('auth-form');
    const emailInput = document.getElementById('email-input');
    const authSubmitButton = document.getElementById('auth-submit-button');
    const userProfile = document.getElementById('user-profile');
    const userEmailDisplay = document.getElementById('user-email');
    const sidebar = document.getElementById('sidebar');
    const mainContent = document.getElementById('main-content');
    const songList = document.getElementById('song-list');
    const addSongButton = document.getElementById('add-song-button');
    const addModal = document.getElementById('add-modal');
    const addSongForm = document.getElementById('add-song-form');
    const cancelButton = document.getElementById('cancel-button');
    const saveButton = document.getElementById('save-button');
    const songNameInput = document.getElementById('song-name-input');
    const artistNameInput = document.getElementById('artist-name-input');
    const durationInput = document.getElementById('duration-input');
    const albumCoverInput = document.getElementById('album-cover-input');
    const playButton = document.getElementById('play-button');
    const prevButton = document.getElementById('prev-button');
    const nextButton = document.getElementById('next-button');
    const shuffleButton = document.getElementById('shuffle-button');
    const repeatButton = document.getElementById('repeat-button');
    const nowPlayingCover = document.getElementById('now-playing-cover');
    const nowPlayingName = document.getElementById('now-playing-name');
    const nowPlayingArtist = document.getElementById('now-playing-artist');
    const currentTimeDisplay = document.getElementById('current-time');
    const totalTimeDisplay = document.getElementById('total-time');
    const progressBar = document.getElementById('progress-bar');
    const progressFill = document.getElementById('progress-fill');
    const playerBar = document.getElementById('player-bar');

    // Helper functions
    function showToast(message, bgColor) {
      const toast = document.createElement('div');
      toast.className = 'toast';
      toast.textContent = message;
      toast.style.backgroundColor = bgColor;
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      toast.style.color = config.text_color || defaultConfig.text_color;
      document.body.appendChild(toast);
      setTimeout(() => toast.remove(), 3000);
    }

    function durationToSeconds(duration) {
      const parts = duration.split(':');
      return parseInt(parts[0]) * 60 + parseInt(parts[1]);
    }

    function formatTime(seconds) {
      const mins = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${mins}:${secs.toString().padStart(2, '0')}`;
    }

    function getAlbumEmoji(index) {
      const emojis = ['🎸', '🎹', '🎤', '🎧', '🎼', '🎺', '🎻', '🥁', '🎷', '🎵'];
      return emojis[index % emojis.length];
    }

    // Auth functions
    authForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const email = emailInput.value.trim();
      if (email) {
        currentUser = email;
        userEmailDisplay.textContent = email;
        authScreen.classList.add('hidden');
        mainApp.classList.remove('hidden');
        renderSongs();
        const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
        showToast('✅ Başarıyla giriş yaptınız!', config.primary_action_color || defaultConfig.primary_action_color);
      }
    });

    userProfile.addEventListener('click', () => {
      currentUser = null;
      mainApp.classList.add('hidden');
      authScreen.classList.remove('hidden');
      emailInput.value = '';
      pause();
      currentSongIndex = -1;
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      showToast('👋 Çıkış yaptınız', config.secondary_action_color || defaultConfig.secondary_action_color);
    });

    function renderSongs() {
      if (!currentUser) return;

      userSongs = allSongs.filter(song => song.user_email === currentUser);

      if (userSongs.length === 0) {
        songList.innerHTML = `
          <div class="empty-state">
            <div class="empty-state-icon">🎵</div>
            <div class="empty-state-text">Henüz şarkı eklemedin</div>
            <div style="opacity: 0.6;">Şarkı ekle butonuna tıklayarak başla</div>
          </div>
        `;
        return;
      }

      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      songList.innerHTML = userSongs.map((song, index) => {
        const coverStyle = song.album_cover_url ? 
          `background-image: url('${song.album_cover_url}');` : 
          `background-color: ${config.primary_action_color || defaultConfig.primary_action_color};`;
        
        const coverContent = song.album_cover_url ? '' : getAlbumEmoji(index);
        const isCurrentSong = currentSongIndex === index;

        return `
          <div class="song-item ${isCurrentSong && isPlaying ? 'playing' : ''}" data-index="${index}" style="background-color: ${config.surface_color || defaultConfig.surface_color}; color: ${config.text_color || defaultConfig.text_color};">
            <div class="song-number">${index + 1}</div>
            <div class="song-album-cover" style="${coverStyle}">${coverContent}</div>
            <div class="song-info">
              <div class="song-name">${song.song_name}</div>
              <div class="song-artist">${song.artist_name}</div>
            </div>
            <div class="song-duration">${song.duration}</div>
            <div class="song-actions">
              <button class="song-action-button song-download" data-index="${index}" style="background-color: ${config.primary_action_color || defaultConfig.primary_action_color}; color: ${config.text_color || defaultConfig.text_color};">📥 İndir</button>
              <button class="song-action-button song-delete" data-index="${index}" style="background-color: ${config.secondary_action_color || defaultConfig.secondary_action_color}; color: ${config.text_color || defaultConfig.text_color};">🗑️ Sil</button>
            </div>
          </div>
        `;
      }).join('');

      // Add event listeners
      document.querySelectorAll('.song-item').forEach(item => {
        item.addEventListener('click', (e) => {
          if (!e.target.classList.contains('song-action-button')) {
            const index = parseInt(item.dataset.index);
            playSong(index);
          }
        });
      });

      document.querySelectorAll('.song-download').forEach(btn => {
        btn.addEventListener('click', (e) => {
          e.stopPropagation();
          const index = parseInt(btn.dataset.index);
          downloadSong(index);
        });
      });

      document.querySelectorAll('.song-delete').forEach(btn => {
        btn.addEventListener('click', (e) => {
          e.stopPropagation();
          const index = parseInt(btn.dataset.index);
          deleteSong(index);
        });
      });
    }

    function downloadSong(index) {
      const song = userSongs[index];
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      // Create a downloadable text file with song info
      const content = `Şarkı: ${song.song_name}\nSanatçı: ${song.artist_name}\nSüre: ${song.duration}\n${song.album_cover_url ? 'Kapak: ' + song.album_cover_url : ''}`;
      const blob = new Blob([content], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `${song.song_name} - ${song.artist_name}.txt`;
      a.click();
      URL.revokeObjectURL(url);
      
      showToast('📥 Şarkı bilgisi indirildi!', config.primary_action_color || defaultConfig.primary_action_color);
    }

    function playSong(index) {
      if (index < 0 || index >= userSongs.length) return;
      
      currentSongIndex = index;
      const song = userSongs[index];
      
      nowPlayingName.textContent = song.song_name;
      nowPlayingArtist.textContent = song.artist_name;
      
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      if (song.album_cover_url) {
        nowPlayingCover.style.backgroundImage = `url('${song.album_cover_url}')`;
        nowPlayingCover.textContent = '';
      } else {
        nowPlayingCover.style.backgroundImage = '';
        nowPlayingCover.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
        nowPlayingCover.textContent = getAlbumEmoji(index);
      }
      
      currentTime = 0;
      totalTimeDisplay.textContent = song.duration;
      
      renderSongs();
      play();
    }

    function play() {
      if (currentSongIndex === -1 || userSongs.length === 0) return;
      
      isPlaying = true;
      playButton.textContent = '⏸️';
      playerBar.classList.add('playing');
      
      clearInterval(playInterval);
      playInterval = setInterval(() => {
        const totalSeconds = durationToSeconds(userSongs[currentSongIndex].duration);
        currentTime += 0.1;
        
        if (currentTime >= totalSeconds) {
          if (repeatMode) {
            currentTime = 0;
          } else {
            nextSong();
          }
        } else {
          currentTimeDisplay.textContent = formatTime(currentTime);
          const progress = (currentTime / totalSeconds) * 100;
          progressFill.style.width = `${progress}%`;
        }
      }, 100);
    }

    function pause() {
      isPlaying = false;
      playButton.textContent = '▶️';
      playerBar.classList.remove('playing');
      clearInterval(playInterval);
    }

    function togglePlay() {
      if (currentSongIndex === -1 && userSongs.length > 0) {
        playSong(0);
      } else if (isPlaying) {
        pause();
      } else {
        play();
      }
    }

    function prevSong() {
      if (userSongs.length === 0) return;
      const newIndex = currentSongIndex - 1 < 0 ? userSongs.length - 1 : currentSongIndex - 1;
      playSong(newIndex);
    }

    function nextSong() {
      if (userSongs.length === 0) return;
      
      let newIndex;
      if (shuffleMode) {
        newIndex = Math.floor(Math.random() * userSongs.length);
      } else {
        newIndex = (currentSongIndex + 1) % userSongs.length;
      }
      
      playSong(newIndex);
    }

    function toggleShuffle() {
      shuffleMode = !shuffleMode;
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      shuffleButton.style.opacity = shuffleMode ? '1' : '0.8';
      shuffleButton.style.color = shuffleMode ? (config.primary_action_color || defaultConfig.primary_action_color) : (config.text_color || defaultConfig.text_color);
      showToast(shuffleMode ? '🔀 Karıştır açık' : '🔀 Karıştır kapalı', config.primary_action_color || defaultConfig.primary_action_color);
    }

    function toggleRepeat() {
      repeatMode = !repeatMode;
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      repeatButton.style.opacity = repeatMode ? '1' : '0.8';
      repeatButton.style.color = repeatMode ? (config.primary_action_color || defaultConfig.primary_action_color) : (config.text_color || defaultConfig.text_color);
      showToast(repeatMode ? '🔁 Tekrar açık' : '🔁 Tekrar kapalı', config.primary_action_color || defaultConfig.primary_action_color);
    }

    function openAddModal() {
      addModal.classList.remove('hidden');
      songNameInput.value = '';
      artistNameInput.value = '';
      durationInput.value = '';
      albumCoverInput.value = '';
      songNameInput.focus();
    }

    function closeAddModal() {
      addModal.classList.add('hidden');
    }

    async function deleteSong(index) {
      if (!window.dataSdk) return;
      
      const song = userSongs[index];
      const originalSong = allSongs.find(s => s.__backendId === song.__backendId);
      
      saveButton.disabled = true;
      saveButton.innerHTML = '<div class="loading-spinner"></div>';
      
      const result = await window.dataSdk.delete(originalSong);
      
      saveButton.disabled = false;
      saveButton.textContent = 'Kaydet';
      
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      if (!result.isOk) {
        showToast('⚠️ Şarkı silinemedi', config.secondary_action_color || defaultConfig.secondary_action_color);
        return;
      }
      
      showToast('🗑️ Şarkı silindi', config.secondary_action_color || defaultConfig.secondary_action_color);
      
      // If currently playing song is deleted
      if (currentSongIndex === index) {
        pause();
        currentSongIndex = -1;
        nowPlayingName.textContent = 'Şarkı seç ve dinle';
        nowPlayingArtist.textContent = '-';
        nowPlayingCover.style.backgroundImage = '';
        nowPlayingCover.textContent = '🎵';
        currentTime = 0;
        currentTimeDisplay.textContent = '0:00';
        totalTimeDisplay.textContent = '0:00';
        progressFill.style.width = '0%';
      } else if (currentSongIndex > index) {
        currentSongIndex--;
      }
    }

    // Event listeners
    addSongButton.addEventListener('click', openAddModal);
    cancelButton.addEventListener('click', closeAddModal);
    
    addSongForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      if (!window.dataSdk || !currentUser) return;
      
      if (allSongs.length >= 999) {
        const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
        showToast('⚠️ Maksimum 999 şarkı limitine ulaşıldı', config.secondary_action_color || defaultConfig.secondary_action_color);
        return;
      }
      
      saveButton.disabled = true;
      saveButton.innerHTML = '<div class="loading-spinner"></div>';
      
      const result = await window.dataSdk.create({
        song_name: songNameInput.value.trim(),
        artist_name: artistNameInput.value.trim(),
        duration: durationInput.value.trim(),
        album_cover_url: albumCoverInput.value.trim(),
        date_added: new Date().toISOString(),
        user_email: currentUser
      });
      
      saveButton.disabled = false;
      saveButton.textContent = 'Kaydet';
      
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      if (result.isOk) {
        closeAddModal();
        showToast('✅ Şarkı eklendi!', config.primary_action_color || defaultConfig.primary_action_color);
      } else {
        showToast('⚠️ Şarkı eklenemedi', config.secondary_action_color || defaultConfig.secondary_action_color);
      }
    });

    playButton.addEventListener('click', togglePlay);
    prevButton.addEventListener('click', prevSong);
    nextButton.addEventListener('click', nextSong);
    shuffleButton.addEventListener('click', toggleShuffle);
    repeatButton.addEventListener('click', toggleRepeat);

    progressBar.addEventListener('click', (e) => {
      if (currentSongIndex === -1) return;
      
      const rect = progressBar.getBoundingClientRect();
      const percent = (e.clientX - rect.left) / rect.width;
      const totalSeconds = durationToSeconds(userSongs[currentSongIndex].duration);
      currentTime = totalSeconds * percent;
      currentTimeDisplay.textContent = formatTime(currentTime);
      progressFill.style.width = `${percent * 100}%`;
    });

    addModal.addEventListener('click', (e) => {
      if (e.target === addModal) {
        closeAddModal();
      }
    });

    // Data SDK
    const dataHandler = {
      onDataChanged(data) {
        allSongs = data;
        if (currentUser) {
          renderSongs();
        }
      }
    };

    if (window.dataSdk) {
      window.dataSdk.init(dataHandler).then(result => {
        if (!result.isOk) {
          console.error('Data SDK initialization failed');
        }
      });
    }

    // Element SDK
    async function onConfigChange(config) {
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontSize = config.font_size || defaultConfig.font_size;
      const baseFontStack = 'Segoe UI, Tahoma, Geneva, Verdana, sans-serif';
      
      // Background colors
      document.body.style.backgroundColor = config.background_color || defaultConfig.background_color;
      sidebar.style.backgroundColor = config.background_color || defaultConfig.background_color;
      mainContent.style.backgroundColor = config.background_color || defaultConfig.background_color;
      playerBar.style.backgroundColor = config.surface_color || defaultConfig.surface_color;
      document.querySelector('.modal-content').style.backgroundColor = config.surface_color || defaultConfig.surface_color;
      document.querySelector('.auth-box').style.backgroundColor = config.surface_color || defaultConfig.surface_color;
      authScreen.style.backgroundColor = config.background_color || defaultConfig.background_color;
      progressBar.style.backgroundColor = config.secondary_action_color || defaultConfig.secondary_action_color;
      
      // Text colors
      document.body.style.color = config.text_color || defaultConfig.text_color;
      document.querySelectorAll('.form-group input, .auth-input').forEach(input => {
        input.style.color = config.text_color || defaultConfig.text_color;
        input.style.backgroundColor = config.background_color || defaultConfig.background_color;
      });
      
      // Primary action colors
      addSongButton.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      addSongButton.style.color = config.text_color || defaultConfig.text_color;
      authSubmitButton.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      authSubmitButton.style.color = config.text_color || defaultConfig.text_color;
      document.querySelector('.modal-button.primary').style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      document.querySelector('.modal-button.primary').style.color = config.text_color || defaultConfig.text_color;
      document.querySelector('.modal-button.secondary').style.backgroundColor = config.secondary_action_color || defaultConfig.secondary_action_color;
      document.querySelector('.modal-button.secondary').style.color = config.text_color || defaultConfig.text_color;
      progressFill.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      document.getElementById('premium-badge').style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      document.getElementById('premium-badge').style.color = config.text_color || defaultConfig.text_color;
      
      if (!nowPlayingCover.style.backgroundImage) {
        nowPlayingCover.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      }
      
      // Nav items
      document.querySelectorAll('.nav-item').forEach(item => {
        item.style.color = config.text_color || defaultConfig.text_color;
      });
      document.querySelector('.nav-item.active').style.backgroundColor = config.secondary_action_color || defaultConfig.secondary_action_color;
      userProfile.style.backgroundColor = config.secondary_action_color || defaultConfig.secondary_action_color;
      document.querySelector('.user-avatar').style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
      
      // Fonts
      document.body.style.fontFamily = `${customFont}, ${baseFontStack}`;
      
      // Font sizes
      document.getElementById('app-logo').style.fontSize = `${baseFontSize * 1.75}px`;
      document.getElementById('auth-title').style.fontSize = `${baseFontSize * 2}px`;
      document.querySelector('.auth-subtitle').style.fontSize = `${baseFontSize}px`;
      document.querySelectorAll('.auth-input').forEach(el => el.style.fontSize = `${baseFontSize}px`);
      authSubmitButton.style.fontSize = `${baseFontSize * 1.125}px`;
      document.querySelectorAll('.nav-item').forEach(el => el.style.fontSize = `${baseFontSize}px`);
      document.getElementById('content-title').style.fontSize = `${baseFontSize * 2}px`;
      addSongButton.style.fontSize = `${baseFontSize * 0.875}px`;
      document.querySelectorAll('.song-name').forEach(el => el.style.fontSize = `${baseFontSize}px`);
      document.querySelectorAll('.song-artist').forEach(el => el.style.fontSize = `${baseFontSize * 0.875}px`);
      document.querySelector('.modal-header').style.fontSize = `${baseFontSize * 1.5}px`;
      document.querySelectorAll('.form-group label').forEach(el => el.style.fontSize = `${baseFontSize * 0.875}px`);
      document.querySelectorAll('.form-group input').forEach(el => el.style.fontSize = `${baseFontSize}px`);
      document.getElementById('premium-badge').style.fontSize = `${baseFontSize * 0.75}px`;
      
      // Editable text
      document.getElementById('app-title').textContent = config.app_title || defaultConfig.app_title;
      document.getElementById('auth-title').textContent = config.app_title || defaultConfig.app_title;
      document.getElementById('content-title').textContent = config.library_title || defaultConfig.library_title;
      document.getElementById('add-button-text').textContent = config.add_button_text || defaultConfig.add_button_text;
      document.getElementById('premium-badge').textContent = config.premium_badge_text || defaultConfig.premium_badge_text;
      
      if (currentUser) {
        renderSongs();
      }
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig: defaultConfig,
        onConfigChange: onConfigChange,
        mapToCapabilities: (config) => ({
          recolorables: [
            {
              get: () => config.background_color || defaultConfig.background_color,
              set: (value) => {
                config.background_color = value;
                window.elementSdk.setConfig({ background_color: value });
              }
            },
            {
              get: () => config.surface_color || defaultConfig.surface_color,
              set: (value) => {
                config.surface_color = value;
                window.elementSdk.setConfig({ surface_color: value });
              }
            },
            {
              get: () => config.text_color || defaultConfig.text_color,
              set: (value) => {
                config.text_color = value;
                window.elementSdk.setConfig({ text_color: value });
              }
            },
            {
              get: () => config.primary_action_color || defaultConfig.primary_action_color,
              set: (value) => {
                config.primary_action_color = value;
                window.elementSdk.setConfig({ primary_action_color: value });
              }
            },
            {
              get: () => config.secondary_action_color || defaultConfig.secondary_action_color,
              set: (value) => {
                config.secondary_action_color = value;
                window.elementSdk.setConfig({ secondary_action_color: value });
              }
            }
          ],
          borderables: [],
          fontEditable: {
            get: () => config.font_family || defaultConfig.font_family,
            set: (value) => {
              config.font_family = value;
              window.elementSdk.setConfig({ font_family: value });
            }
          },
          fontSizeable: {
            get: () => config.font_size || defaultConfig.font_size,
            set: (value) => {
              config.font_size = value;
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }),
        mapToEditPanelValues: (config) => new Map([
          ["app_title", config.app_title || defaultConfig.app_title],
          ["library_title", config.library_title || defaultConfig.library_title],
          ["add_button_text", config.add_button_text || defaultConfig.add_button_text],
          ["premium_badge_text", config.premium_badge_text || defaultConfig.premium_badge_text]
        ])
      });
    }
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a33671770f9cd84',t:'MTc2MzkyOTU5OS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
