# heonchaeng0300
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>iPhone Simulator - 이채영</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-user-select: none;
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", Arial, sans-serif;
    }

    body {
      background-color: #0f0f11;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }

    /* iPhone Frame */
    .phone-mockup {
      width: 380px;
      height: 780px;
      background: #000;
      border-radius: 54px;
      box-shadow: 0 25px 60px rgba(0,0,0,0.8), 0 0 0 12px #222224, 0 0 0 13px #444446;
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    /* Dynamic Island */
    .dynamic-island {
      position: absolute;
      top: 11px;
      left: 50%;
      transform: translateX(-50%);
      width: 120px;
      height: 32px;
      background-color: #000;
      border-radius: 20px;
      z-index: 9999;
    }

    /* Status Bar */
    .status-bar {
      height: 48px;
      padding: 14px 24px 0 24px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: #fff;
      font-size: 14px;
      font-weight: 600;
      z-index: 9000;
      position: absolute;
      top: 0;
      width: 100%;
    }

    .status-right {
      display: flex;
      gap: 6px;
      align-items: center;
    }

    /* Screen Container */
    .screen {
      flex: 1;
      width: 100%;
      height: 100%;
      position: relative;
      background-color: #000;
    }

    /* Views */
    .view {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: none;
      flex-direction: column;
      background-size: cover;
      background-position: center;
    }

    .view.active {
      display: flex;
    }

    /* Home Screen View */
    #home-view {
      /* 야경 셀카 배경화면 반영[span_0](start_span)[span_0](end_span) */
      background-image: linear-gradient(rgba(0,0,0,0.1), rgba(0,0,0,0.2)), url('https://images.unsplash.com/photo-1514565131-fce0801e5785?auto=format&fit=crop&w=800&q=80');
      padding-top: 60px;
      padding-bottom: 20px;
      justify-content: space-between;
    }

    .app-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px 14px;
      padding: 20px;
    }

    .app-icon {
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
    }

    .icon-box {
      width: 62px;
      height: 62px;
      border-radius: 16px;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: 0 8px 20px rgba(0,0,0,0.3);
      position: relative;
      overflow: hidden;
      backdrop-filter: blur(10px);
      transition: transform 0.1s;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
    }

    .icon-box:active {
      transform: scale(0.92);
    }

    .app-name {
      color: #fff;
      font-size: 11px;
      margin-top: 6px;
      text-shadow: 0 1px 4px rgba(0,0,0,0.8);
    }

    /* 아이콘 이미지 연결용 CSS (postimages 등에 업로드한 이미지 링크를 url('') 안에 넣으시면 됩니다) */
    .icon-phone { background-color: #34c759; }
    .icon-messages { background-color: #30b0c7; }
    .icon-notes { background-color: #fbc531; }
    .icon-safari { background-color: #007aff; }
    .icon-photos { background-color: #ff9500; }
    .icon-translate { background-color: #5ac8fa; }
    .icon-weather { background-color: #5856d6; }
    .icon-appstore { background-color: #007aff; }

    /* Dock */
    .dock-container {
      padding: 0 16px 16px 16px;
    }

    .dock {
      background: rgba(255, 255, 255, 0.25);
      backdrop-filter: blur(25px);
      -webkit-backdrop-filter: blur(25px);
      border-radius: 32px;
      padding: 12px 18px;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }

    /* Apps Interface (Dark Mode iOS) */
    .app-header {
      padding: 50px 16px 12px 16px;
      background: rgba(20, 20, 22, 0.95);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 0.5px solid rgba(255,255,255,0.1);
    }

    .back-btn {
      color: #0a84ff;
      font-size: 16px;
      display: flex;
      align-items: center;
      cursor: pointer;
    }

    .app-title {
      font-size: 16px;
      font-weight: 600;
    }

    /* Phone App View (지헌이❤️ 통화 목록)[span_1](start_span)[span_1](end_span) */
    #phone-view {
      background: #000;
      color: #fff;
    }

    .favorites-section {
      padding: 16px;
      border-bottom: 0.5px solid #222;
    }

    .fav-card {
      background: #1c1c1e;
      border-radius: 16px;
      padding: 14px;
      width: 90px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }

    .fav-avatar {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background: #333;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 20px;
    }

    .call-list {
      flex: 1;
      overflow-y: auto;
      padding: 10px 16px;
    }

    .call-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 0;
      border-bottom: 0.5px solid #1c1c1e;
    }

    .call-info {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .call-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: #2c2c2e;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 18px;
    }

    .call-name {
      font-weight: 600;
      font-size: 15px;
      color: #ff3b30;
    }

    .call-type {
      font-size: 12px;
      color: #8e8e93;
    }

    .call-meta {
      text-align: right;
    }

    .call-date {
      font-size: 13px;
      color: #8e8e93;
    }

    .call-duration {
      font-size: 11px;
      color: #34c759;
      margin-top: 2px;
    }

    /* Messages View (채영언니 & 지헌이 대화 및 트리거) */
    #messages-view {
      background: #000;
    }

    .msg-header-profile {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
    }

    .default-avatar {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: #8e8e93;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
    }

    .chat-container {
      flex: 1;
      padding: 16px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 10px;
      background: #000;
    }

    .message-bubble {
      max-width: 70%;
      padding: 10px 14px;
      border-radius: 18px;
      font-size: 15px;
      line-height: 1.4;
      word-break: break-word;
    }

    .msg-me {
      align-self: flex-end;
      background: #0a84ff;
      color: #fff;
      border-bottom-right-radius: 4px;
    }

    .msg-them {
      align-self: flex-start;
      background: #262628;
      color: #fff;
      border-bottom-left-radius: 4px;
    }

    .typing-indicator {
      align-self: flex-start;
      background: #262628;
      color: #8e8e93;
      padding: 8px 14px;
      border-radius: 18px;
      font-size: 13px;
      display: none;
    }

    .chat-input-area {
      padding: 10px 16px 28px 16px;
      background: #121212;
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .chat-input {
      flex: 1;
      background: #1c1c1e;
      border: 1px solid #333;
      border-radius: 20px;
      padding: 8px 16px;
      color: #fff;
      font-size: 14px;
      outline: none;
    }

    .send-btn {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background: #0a84ff;
      border: none;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      font-weight: bold;
    }

    /* Notes App View (메모 목록 및 상세 내용)[span_2](start_span)[span_2](end_span)[span_3](start_span)[span_3](end_span) */
    #notes-view {
      background: #000;
      color: #fff;
    }

    .notes-list {
      padding: 16px;
    }

    .note-card {
      background: #1c1c1e;
      border-radius: 12px;
      padding: 14px 16px;
      cursor: pointer;
    }

    .note-title {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .note-preview {
      font-size: 13px;
      color: #8e8e93;
    }

    #note-detail-view {
      background: #000;
      color: #fff;
    }

    .note-content {
      padding: 20px 16px;
      line-height: 1.6;
    }

    /* Home Bar */
    .home-bar {
      position: absolute;
      bottom: 8px;
      left: 50%;
      transform: translateX(-50%);
      width: 134px;
      height: 5px;ㅑ
      background: #fff;
      border-radius: 3px;
      z-index: 9999;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <div class="phone-mockup">
    <!-- Dynamic Island -->
    <div class="dynamic-island"></div>

    <!-- Status Bar -->
    <div class="status-bar">
      <span>11:50</span>
      <div class="status-right">
        <span>5G</span>
        <svg width="18" height="12" viewBox="0 0 24 12" fill="currentColor"><path d="M2,2H20V10H2V2M0,0V12H22V0H0M24,4V8H23V4H24Z"/></svg>
      </div>
    </div>

    <div class="screen">
      <!-- 1. HOME SCREEN VIEW -->
      <div id="home-view" class="view active">
        <div class="app-grid">
          <div class="app-icon" onclick="openApp('phone-view')">
            <div class="icon-box icon-phone"></div>
            <span class="app-name">전화</span>
          </div>
          <div class="app-icon" onclick="openApp('messages-view')">
            <div class="icon-box icon-messages"></div>
            <span class="app-name">메시지</span>
          </div>
          <div class="app-icon" onclick="openApp('notes-view')">
            <div class="icon-box icon-notes"></div>
            <span class="app-name">메모</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-safari"></div>
            <span class="app-name">Safari</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-photos"></div>
            <span class="app-name">사진</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-translate"></div>
            <span class="app-name">번역</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-weather"></div>
            <span class="app-name">날씨</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-appstore"></div>
            <span class="app-name">App Store</span>
          </div>
        </div>

        <div class="dock-container">
          <div class="dock">
            <div class="app-icon" onclick="openApp('phone-view')">
              <div class="icon-box icon-phone"></div>
            </div>
            <div class="app-icon" onclick="openApp('messages-view')">
              <div class="icon-box icon-messages"></div>
            </div>
          </div>
        </div>
      </div>

      <!-- 2. PHONE APP VIEW (지헌이❤️ 통화 목록)[span_4](start_span)[span_4](end_span) -->
      <div id="phone-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="goHome()">‹ 홈</span>
          <span class="app-title">통화</span>
          <span style="color:#0a84ff; font-size:14px;">편집</span>
        </div>
        <div class="favorites-section">
          <div class="fav-card">
            <div class="fav-avatar">❤️</div>
            <span style="font-size:12px; font-weight:600;">지헌이❤️</span>
          </div>
        </div>
        <div class="call-list">
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">👤</div>
              <div>
                <div class="call-name" >지헌이❤️</div>
                <div class="call-type">↙ 휴대폰</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">방금 전</div>
            </div>
          </div>
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">👤</div>
              <div>
                <div class="call-name" style="color:#fff;">지헌이❤️</div>
                <div class="call-type">↗ 휴대폰</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">어제</div>
              <div class="call-duration">3시간 12분</div>
            </div>
          </div>
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">👤</div>
              <div>
                <div class="call-name" style="color:#fff;">지헌이❤️</div>
                <div class="call-type">↙ KakaoTalk 영상통화</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">토요일</div>
              <div class="call-duration">6시간 35분</div>
            </div>
          </div>
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">👤</div>
              <div>
                <div class="call-name" style="color:#fff;">지헌이❤️</div>
                <div class="call-type">↙ 휴대폰</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">목요일</div>
              <div class="call-duration">1시간 02분</div>
            </div>
          </div>
        </div>
      </div>

      <!-- 3. MESSAGES VIEW (채영언니 & 지헌이 트리거) -->
      <div id="messages-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="goHome()">‹ 뒤로</span>
          <div class="msg-header-profile">
            <div class="default-avatar">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
            </div>
            <span style="font-size:12px;">지헌이</span>
          </div>
          <span style="color:#0a84ff; font-size:18px;">📹</span>
        </div>

        <div class="chat-container" id="chat-container">
          <div class="message-bubble msg-them">채영언니! 오늘 몇 시에 만나?</div>
          <div class="message-bubble msg-me">나 퇴근하고 바로 갈게! 히히</div>
        </div>
        <div class="typing-indicator" id="typing">지헌이가 입력 중...</div>

        <div class="chat-input-area">
          <input type="text" id="chat-input" class="chat-input" placeholder="iMessage" onkeypress="handleKeyPress(event)">
          <button class="send-btn" onclick="sendMessage()">↑</button>
        </div>
      </div>

      <!-- 4. NOTES VIEW (목록)[span_5](start_span)[span_5](end_span) -->
      <div id="notes-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="goHome()">‹ 폴더</span>
          <span class="app-title">메모</span>
          <span></span>
        </div>
        <div class="notes-list">
          <div class="note-card" onclick="openApp('note-detail-view')">
            <div class="note-title">지헌이 줄 꽃다발 사기</div>
            <div class="note-preview">11:50 (인스타 팔로우 해뒀던 꽃집에서 사...</div>
          </div>
        </div>
      </div>

      <!-- 5. NOTE DETAIL VIEW (상세 내용)[span_6](start_span)[span_6](end_span) -->
      <div id="note-detail-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="openApp('notes-view')">‹ 메모</span>
          <span class="app-title"></span>
          <span></span>
        </div>
        <div class="note-content">
          <h2 style="font-size: 20px; font-weight: 700; margin-bottom: 12px;">지헌이 줄 꽃다발 사기</h2>
          <p style="color: #ccc; font-size: 15px;">
            (인스타 팔로우 해뒀던 꽃집에서 사기 꼭!!!! ㅠㅠ 그래야 지헌이가 좋아할 거 같으니깐 ㅎㅎㅎ)
          </p>
        </div>
      </div>

    </div>

    <!-- Home Indicator Bar -->
    <div class="home-bar" onclick="goHome()"></div>
  </div>

  <script>
    function openApp(viewId) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById(viewId).classList.add('active');
    }

    function goHome() {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById('home-view').classList.add('active');
    }

    function handleKeyPress(e) {
      if (e.key === 'Enter') sendMessage();
    }

    function sendMessage() {
      const input = document.getElementById('chat-input');
      const text = input.value.trim();
      if (!text) return;

      const chatContainer = document.getElementById('chat-container');

      // Send My Message (이채영)
      const myMsg = document.createElement('div');
      myMsg.className = 'message-bubble msg-me';
      myMsg.innerText = text;
      chatContainer.appendChild(myMsg);

      input.value = '';
      chatContainer.scrollTop = chatContainer.scrollHeight;

      // Show typing indicator
      const typing = document.getElementById('typing');
      setTimeout(() => { typing.style.display = 'block'; }, 600);

      // Auto Reply Logic ('채영언니' 호칭 및 애정 트리거)
      setTimeout(() => {
        typing.style.display = 'none';

        let replyText = "응응! 맞아요 언니! ❤️";

        if (text.includes('사랑해')) {
          replyText = "채영언니 사랑해!! ❤️❤️❤️";
        } else if (text.includes('보고싶')) {
          replyText = "나도 채영언니 너무 보고싶어! 금방 갈게!";
        } else if (text.includes('뭐해') || text.includes('뭐함')) {
          replyText = "나 언니 생각하고 있었지 ㅎㅎ 뭐해?";
        } else if (text.includes('안녕')) {
          replyText = "채영언니 안녕! 오늘 하루도 파이팅이야!";
        }

        const replyMsg = document.createElement('div');
        replyMsg.className = 'message-bubble msg-them';
        replyMsg.innerText = replyText;
        chatContainer.appendChild(replyMsg);

        chatContainer.scrollTop = chatContainer.scrollHeight;
      }, 1800);
    }
  </script>
</body>
</html>
<style>
  /* 중간 생략... */

  /* Home Screen View */
  #home-view {
    background-image: linear-gradient(rgba(0,0,0,0.1), rgba(0,0,0,0.2)), url('https://images.unsplash.com/photo-1514565131-fce0801e5785?auto=format&fit=crop&w=800&q=80');
    background-size: cover;
    background-position: center;
    padding-top: 60px;
    padding-bottom: 20px;
    justify-content: space-between;
  }

  /* 이하 생략... */
</style>