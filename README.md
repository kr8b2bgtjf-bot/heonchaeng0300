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
      width: 100vw;
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
      flex-shrink: 0;
      transform-origin: center center;
    }

    /* Dynamic Island 기본 및 FaceTime 수신 확장 애니메이션 */
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
      transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
      overflow: hidden;
      display: flex;
      align-items: center;
    }

    .dynamic-island.facetime-incoming {
      width: 356px;
      height: 62px;
      border-radius: 32px;
      padding: 0 10px;
      justify-content: space-between;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.6);
    }

    .di-facetime-content {
      display: none;
      width: 100%;
      height: 100%;
      align-items: center;
      justify-content: space-between;
      color: #fff;
    }

    .dynamic-island.facetime-incoming .di-facetime-content {
      display: flex;
    }

    .di-ft-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .di-ft-avatar {
      width: 42px;
      height: 42px;
      border-radius: 50%;
      overflow: hidden;
      flex-shrink: 0;
    }

    .di-ft-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .di-ft-text {
      display: flex;
      flex-direction: column;
    }

    .di-ft-sub {
      font-size: 11px;
      color: #8e8e93;
      font-weight: 500;
    }

    .di-ft-name {
      font-size: 14px;
      font-weight: 600;
      color: #fff;
    }

    .di-ft-actions {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .di-ft-btn {
      width: 38px;
      height: 38px;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      overflow: hidden;
      transition: transform 0.1s;
    }

    .di-ft-btn img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .di-ft-btn:active {
      transform: scale(0.92);
    }

    .di-ft-decline {
      background: #ff3b30;
    }

    .di-ft-accept {
      background: #34c759;
    }

    /* Status Bar */
    .status-bar {
      height: 48px;
      padding: 14px 24px 0 34px;
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

    .status-left {
      font-weight: 600;
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

    /* 🔒 잠금화면 */
    #lock-view {
      background-image: linear-gradient(rgba(0,0,0,0.1), rgba(0,0,0,0.2)), url('https://i.postimg.cc/0yGfSkpM/IMG-7286.webp');
      padding-top: 90px;
      align-items: center;
      cursor: pointer;
    }

    .lock-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
    }

    .lock-date {
      font-size: 16px;
      font-weight: 400;
      margin-bottom: 2px;
      color: #fff;
      display: flex;
      align-items: center;
      gap: 6px;
      text-shadow: 0 1px 4px rgba(0,0,0,0.8);
    }

    .lock-time {
      font-size: 76px;
      font-weight: 200;
      color: #fff;
      letter-spacing: -2px;
      text-shadow: 0 2px 8px rgba(0,0,0,0.8);
    }

    /* 🔑 암호 입력 화면 */
    #passcode-view {
      background-image: linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.5)), url('https://i.postimg.cc/0yGfSkpM/IMG-7286.webp');
      padding-top: 80px;
      align-items: center;
      color: #fff;
    }

    .passcode-header {
      font-size: 20px;
      font-weight: 600;
      margin-bottom: 24px;
      text-shadow: 0 1px 4px rgba(0,0,0,0.8);
    }

    .passcode-dots {
      display: flex;
      gap: 16px;
      margin-bottom: 40px;
    }

    .dot {
      width: 14px;
      height: 14px;
      border: 1.5px solid #fff;
      border-radius: 50%;
      transition: background 0.2s;
    }

    .keypad {
      display: grid;
      grid-template-columns: repeat(3, 75px);
      gap: 16px;
      justify-content: center;
    }

    .key {
      width: 75px;
      height: 75px;
      background: rgba(255, 255, 255, 0.15);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      border-radius: 50%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      font-size: 28px;
      font-weight: 400;
      cursor: pointer;
      user-select: none;
      transition: background 0.1s;
    }

    .key:active {
      background: rgba(255, 255, 255, 0.35);
    }

    .key .sub {
      font-size: 10px;
      letter-spacing: 2px;
      margin-top: -2px;
      color: rgba(255,255,255,0.7);
    }

    .key.empty {
      background: transparent;
      cursor: default;
    }

    .passcode-bottom-actions {
      width: 280px;
      display: flex;
      justify-content: space-between;
      margin-top: 30px;
      font-size: 15px;
      color: #fff;
      cursor: pointer;
    }

    /* Home Screen View */
    #home-view {
      background-image: linear-gradient(rgba(0,0,0,0.1), rgba(0,0,0,0.2)), url('https://i.postimg.cc/0yGfSkpM/IMG-7286.webp');
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
      background-size: 100% 100%;
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

    /* 아이콘 이미지 연결용 CSS */
    .icon-phone { background-image: url('https://i.postimg.cc/nhWghYtq/371C3CDC-F33C-4C47-870D-CCAA1DCA277A.jpg'); }
    .icon-messages { background-image: url('https://i.postimg.cc/0jLPmgTG/2BAB8799-B43E-4A04-BD55-5BF01AA874BC.jpg'); }
    .icon-notes { background-image: url('https://i.postimg.cc/VvybfhKJ/05A28501-CEB7-41F5-94F4-D669E0E8313F.jpg'); }
    .icon-safari { background-image: url('https://i.postimg.cc/L5T4cQwK/73ACAA96-BD53-460D-9EC7-BE5369D81BB6.jpg'); }
    .icon-photos { background-image: url('https://i.postimg.cc/yNFMpgw4/2495967D-9216-4D1C-BAAC-9D72A38E7FCF.jpg'); }
    .icon-translate { background-image: url('https://i.postimg.cc/C5VC59z7/IMG-7268.jpg'); }
    .icon-weather { background-image: url('https://i.postimg.cc/xjKPkGMw/A2AAF013-52E8-4C30-B88D-D0604CF05339.jpg'); }
    .icon-appstore { background-image: url('https://i.postimg.cc/fTm1R0Rj/77167EB1-3BE3-4AE8-9B3A-FC3ED462E920.jpg'); }
    .icon-kakaotalk { background-image: url('https://i.postimg.cc/R061PHhh/E8CA9AA1-61D8-4313-A847-BC620480BA44.jpg'); }
    .icon-instagram { background-image: url('https://i.postimg.cc/mkjwLL33/C9933B0F-9D3E-4A6E-96F7-A262F2EAFD04.jpg'); }
    .icon-voice-memo { background-image: url('https://i.postimg.cc/0jXwXbdh/EF848D93-4C6F-4913-97A0-A4C1376E7323.jpg'); }

    /* 🔔 인스타그램 푸시 알림 배너 스타일 */
    #insta-banner {
      position: absolute;
      top: -100px;
      left: 12px;
      right: 12px;
      background: rgba(40, 40, 45, 0.85);
      backdrop-filter: blur(30px);
      -webkit-backdrop-filter: blur(30px);
      border-radius: 24px;
      padding: 12px 16px;
      display: flex;
      align-items: center;
      gap: 12px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
      border: 0.5px solid rgba(255, 255, 255, 0.15);
      color: #fff;
      z-index: 99999;
      transition: top 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      cursor: pointer;
    }

    #insta-banner.show {
      top: 54px;
    }

    .insta-banner-icon {
      width: 38px;
      height: 38px;
      border-radius: 10px;
      background-image: url('https://i.postimg.cc/mkjwLL33/C9933B0F-9D3E-4A6E-96F7-A262F2EAFD04.jpg');
      background-size: cover;
      background-position: center;
      flex-shrink: 0;
    }

    .insta-banner-content {
      flex: 1;
      min-width: 0;
    }

    .insta-banner-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2px;
    }

    .insta-banner-title {
      font-size: 13px;
      font-weight: 600;
      color: #fff;
    }

    .insta-banner-time {
      font-size: 11px;
      color: #8e8e93;
    }

    .insta-banner-text {
      font-size: 13px;
      color: #d1d1d6;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    /* 💬 카카오톡 푸시 알림 배너 스타일 */
    #kakao-banner {
      position: absolute;
      top: -100px;
      left: 12px;
      right: 12px;
      background: rgba(40, 40, 45, 0.85);
      backdrop-filter: blur(30px);
      -webkit-backdrop-filter: blur(30px);
      border-radius: 24px;
      padding: 12px 16px;
      display: flex;
      align-items: center;
      gap: 12px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
      border: 0.5px solid rgba(255, 255, 255, 0.15);
      color: #fff;
      z-index: 99999;
      transition: top 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      cursor: pointer;
    }

    #kakao-banner.show {
      top: 54px;
    }

    .kakao-banner-icon {
      width: 38px;
      height: 38px;
      border-radius: 10px;
      background-image: url('https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg');
      background-size: cover;
      background-position: center;
      flex-shrink: 0;
    }

    .kakao-banner-content {
      flex: 1;
      min-width: 0;
    }

    .kakao-banner-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2px;
    }

    .kakao-banner-title {
      font-size: 13px;
      font-weight: 600;
      color: #fff;
    }

    .kakao-banner-time {
      font-size: 11px;
      color: #8e8e93;
    }

    .kakao-banner-text {
      font-size: 13px;
      color: #d1d1d6;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    /* 📸 인스타그램 피드 뷰 스타일 */
    #insta-feed-view {
      background: #000;
      color: #fff;
    }

    .insta-feed-header {
      padding: 48px 16px 10px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.1);
      background: #000;
    }

    .insta-logo-text {
      font-family: 'Billabong', cursive, sans-serif;
      font-size: 24px;
      font-weight: 700;
      letter-spacing: -0.5px;
    }

    .insta-header-icons {
      display: flex;
      gap: 18px;
      font-size: 20px;
    }

    .insta-feed-scroll {
      flex: 1;
      overflow-y: auto;
      padding-bottom: 60px;
    }

    .insta-stories-bar {
      display: flex;
      gap: 14px;
      padding: 12px 16px;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.1);
      overflow-x: auto;
    }

    .insta-story-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      cursor: pointer;
      flex-shrink: 0;
    }

    .insta-story-ring {
      width: 62px;
      height: 62px;
      border-radius: 50%;
      background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
      padding: 2px;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
    }

    .insta-story-ring.viewed {
      background: #555 !important;
    }

    .insta-story-ring img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      border: 2px solid #000;
      object-fit: cover;
    }

    .insta-story-plus {
      position: absolute;
      bottom: 0;
      right: 0;
      width: 20px;
      height: 20px;
      background: #ffffff;
      border: 2px solid #000;
      border-radius: 50%;
      color: #000000;
      font-size: 13px;
      font-weight: bold;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .insta-story-name {
      font-size: 11px;
      color: #ddd;
    }

    .insta-post {
      margin-bottom: 20px;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.08);
    }

    .insta-post-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 14px;
    }

    .insta-post-user {
      display: flex;
      align-items: center;
      gap: 10px;
      cursor: pointer;
    }

    .insta-post-avatar {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
      padding: 1.5px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .insta-post-avatar.viewed {
      background: #555 !important;
    }

    .insta-post-avatar-inner {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      overflow: hidden;
      border: 1.5px solid #000;
    }

    .insta-post-avatar-inner img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .insta-post-username {
      font-size: 13px;
      font-weight: 600;
    }

    .insta-post-image {
      width: 100%;
      aspect-ratio: 1;
      background: #1c1c1e;
    }

    .insta-post-image img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .insta-post-actions {
      display: flex;
      justify-content: space-between;
      padding: 10px 14px 6px 14px;
      font-size: 20px;
    }

    .insta-actions-left {
      display: flex;
      gap: 14px;
    }

    .insta-post-likes {
      padding: 0 14px;
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 6px;
    }

    .insta-post-caption {
      padding: 0 14px;
      font-size: 13px;
      line-height: 1.4;
      color: #eee;
    }

    .insta-post-caption b {
      color: #fff;
      margin-right: 6px;
    }

    .insta-post-time {
      padding: 6px 14px 12px 14px;
      font-size: 10px;
      color: #8e8e93;
    }

    /* 👤 인스타그램 프로필 뷰 공통 스타일 */
    #insta-profile-view, #insta-my-profile-view {
      background: #000;
      color: #fff;
    }

    .insta-profile-header {
      padding: 48px 16px 12px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.1);
      background: #000;
      position: relative;
    }

    .insta-profile-scroll-body {
      flex: 1;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      padding-bottom: 60px;
    }

    .insta-profile-top-info {
      padding: 14px 16px 8px 16px;
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .insta-profile-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .insta-profile-avatar-ring {
      width: 82px;
      height: 82px;
      border-radius: 50%;
      background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
      padding: 2.5px;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
    }

    .insta-profile-avatar-ring.viewed {
      background: #555 !important;
    }

    .insta-profile-avatar-large {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      overflow: hidden;
      border: 2px solid #000;
    }

    .insta-profile-avatar-large img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .insta-profile-stats {
      display: flex;
      gap: 22px;
      text-align: center;
      margin-right: 6px;
    }

    .insta-stat-item {
      display: flex;
      flex-direction: column;
    }

    .insta-stat-num {
      font-size: 16px;
      font-weight: 700;
      color: #ffffff;
    }

    .insta-stat-label {
      font-size: 12px;
      color: #ffffff;
    }

    .insta-profile-bio {
      display: flex;
      flex-direction: column;
      gap: 2px;
      font-size: 13px;
    }

    .insta-bio-name {
      font-weight: 600;
      color: #fff;
    }

    .insta-bio-text {
      color: #ddd;
      line-height: 1.4;
    }

    .insta-mutual-followers {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 0 16px 10px 16px;
      font-size: 12px;
      color: #ccc;
    }

    .mutual-avatars {
      display: flex;
      margin-right: -4px;
    }

    .mutual-avatar {
      width: 22px;
      height: 22px;
      border-radius: 50%;
      border: 1px solid #000;
      overflow: hidden;
      margin-left: -8px;
    }

    .mutual-avatar:first-child {
      margin-left: 0;
    }

    .mutual-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .insta-profile-buttons {
      display: flex;
      gap: 6px;
      padding: 0 16px 12px 16px;
    }

    .insta-btn {
      flex: 1;
      background: #262626;
      color: #fff;
      font-size: 13px;
      font-weight: 600;
      padding: 7px 0;
      border-radius: 8px;
      text-align: center;
      cursor: pointer;
    }

    .insta-btn-icon {
      width: 34px;
      background: #262626;
      border-radius: 8px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
      cursor: pointer;
    }

    .insta-highlights-bar {
      display: flex;
      gap: 16px;
      padding: 0 16px 12px 16px;
      overflow-x: auto;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.08);
    }

    .insta-highlight-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      cursor: pointer;
    }

    .insta-highlight-ring {
      width: 58px;
      height: 58px;
      border-radius: 50%;
      border: 1px solid rgba(255,255,255,0.25);
      padding: 2px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .insta-highlight-ring img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      object-fit: cover;
    }

    .insta-highlight-name {
      font-size: 11px;
      color: #aaa;
    }

    .insta-grid-tabs {
      display: flex;
      border-bottom: 0.5px solid rgba(255, 255, 255, 0.1);
    }

    .insta-grid-tab {
      flex: 1;
      display: flex;
      justify-content: center;
      padding: 10px 0;
      font-size: 18px;
      color: #8e8e93;
      cursor: pointer;
    }

    .insta-grid-tab.active {
      color: #fff;
      border-bottom: 1px solid #fff;
    }

    .insta-user-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 2px;
    }

    .insta-grid-item {
      aspect-ratio: 1;
      background: #1c1c1e;
      overflow: hidden;
      position: relative;
    }

    .insta-grid-item img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    /* 📱 인스타그램 하단 탭바 스타일 */
    .insta-tabbar {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 50px;
      background: #000;
      border-top: 0.5px solid rgba(255, 255, 255, 0.15);
      display: flex;
      justify-content: space-around;
      align-items: center;
      z-index: 100;
    }

    .insta-tab-icon {
      font-size: 20px;
      color: #fff;
      cursor: pointer;
      opacity: 0.6;
    }

    .insta-tab-icon.active {
      opacity: 1;
    }

    .insta-my-avatar-icon {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      overflow: hidden;
      border: 1px solid #fff;
      cursor: pointer;
    }

    .insta-my-avatar-icon img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    /* 📸 인스타그램 스토리 뷰어 스타일 */
    #insta-story-view {
      background: #121212;
      color: #fff;
      position: relative;
    }

    .story-container {
      width: 100%;
      height: 100%;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #000;
    }

    .story-bg-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
      position: absolute;
      top: 0;
      left: 0;
    }

    .story-header {
      position: absolute;
      top: 54px;
      left: 16px;
      right: 16px;
      z-index: 20;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .story-bars {
      display: flex;
      gap: 4px;
      width: 100%;
    }

    .story-bar {
      flex: 1;
      height: 2px;
      background: rgba(255, 255, 255, 0.4);
      border-radius: 2px;
      overflow: hidden;
    }

    .story-bar-fill {
      width: 100%;
      height: 100%;
      background: #fff;
      transform: scaleX(0);
      transform-origin: left;
      animation: fillBar 5s linear forwards;
    }

    @keyframes fillBar {
      to { transform: scaleX(1); }
    }

    .story-user-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .story-profile-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .story-profile-img {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      overflow: hidden;
      border: 1px solid rgba(255,255,255,0.3);
    }

    .story-profile-img img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .story-username {
      font-size: 13px;
      font-weight: 600;
      text-shadow: 0 1px 2px rgba(0,0,0,0.8);
    }

    .story-time {
      font-size: 12px;
      color: rgba(255,255,255,0.7);
      text-shadow: 0 1px 2px rgba(0,0,0,0.8);
    }

    .story-close-btn {
      font-size: 22px;
      color: #fff;
      cursor: pointer;
      background: none;
      border: none;
      text-shadow: 0 1px 3px rgba(0,0,0,0.8);
    }

    .story-footer {
      position: absolute;
      bottom: 30px;
      left: 16px;
      right: 16px;
      z-index: 20;
      display: flex;
      gap: 12px;
      align-items: center;
    }

    .story-input {
      flex: 1;
      background: rgba(0, 0, 0, 0.4);
      border: 1px solid rgba(255, 255, 255, 0.4);
      border-radius: 20px;
      padding: 10px 16px;
      color: #fff;
      font-size: 14px;
      outline: none;
      backdrop-filter: blur(10px);
    }

    .story-action-icon {
      font-size: 24px;
      cursor: pointer;
      text-shadow: 0 1px 3px rgba(0,0,0,0.8);
    }

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
      justify-content: center;
      gap: 16px;
      align-items: center;
    }

    /* Common App Header */
    .app-header {
      padding: 50px 16px 12px 16px;
      background: rgba(20, 20, 22, 0.95);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 0.5px solid rgba(255,255,255,0.1);
      position: relative;
    }

    .back-btn {
      color: #0a84ff;
      font-size: 16px;
      display: flex;
      align-items: center;
      cursor: pointer;
      z-index: 2;
    }

    .app-title {
      font-size: 16px;
      font-weight: 600;
    }

    .center-title-header {
      position: relative;
    }

    .center-title-header .app-title {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
    }

    /* 💌 메시지 목록 화면 스타일 */
    #messages-view {
      background: #000;
      color: #fff;
    }

    .msg-list-header {
      padding: 52px 16px 10px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #000;
      position: relative;
    }

    .msg-edit-btn {
      background: #1c1c1e;
      color: #fff;
      font-size: 14px;
      font-weight: 500;
      padding: 6px 14px;
      border-radius: 16px;
      cursor: pointer;
      z-index: 2;
    }

    .msg-main-title {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      font-size: 18px;
      font-weight: 700;
    }

    .msg-menu-btn {
      width: 32px;
      height: 32px;
      background: #1c1c1e;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
      color: #fff;
      cursor: pointer;
      z-index: 2;
    }

    .msg-conversation-list {
      flex: 1;
      overflow-y: auto;
      padding-bottom: 70px;
    }

    .msg-conv-item {
      display: flex;
      align-items: center;
      padding: 10px 16px;
      gap: 12px;
      cursor: pointer;
      position: relative;
    }

    .msg-conv-item:active {
      background: #1c1c1e;
    }

    .msg-unread-dot {
      width: 8px;
      height: 8px;
      background: #0a84ff;
      border-radius: 50%;
      flex-shrink: 0;
    }

    .msg-conv-avatar {
      width: 42px;
      height: 42px;
      border-radius: 50%;
      background: #2c2c2e;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      flex-shrink: 0;
    }

    .msg-conv-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .msg-conv-info {
      flex: 1;
      min-width: 0;
    }

    .msg-conv-top-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2px;
    }

    .msg-conv-sender {
      font-size: 15px;
      font-weight: 600;
      color: #fff;
    }

    .msg-conv-time {
      font-size: 13px;
      color: #8e8e93;
    }

    .msg-conv-preview {
      font-size: 13px;
      color: #8e8e93;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .msg-conv-chevron {
      color: #8e8e93;
      font-size: 14px;
      margin-left: 4px;
    }

    .msg-bottom-search-bar {
      position: absolute;
      bottom: 0;
      width: 100%;
      background: rgba(20, 20, 22, 0.95);
      backdrop-filter: blur(20px);
      padding: 10px 16px 28px 16px;
      display: flex;
      align-items: center;
      gap: 12px;
      border-top: 0.5px solid rgba(255,255,255,0.1);
      z-index: 10;
    }

    .msg-search-input-box {
      flex: 1;
      background: #1c1c1e;
      border-radius: 20px;
      padding: 8px 14px;
      display: flex;
      align-items: center;
      gap: 8px;
      color: #8e8e93;
      font-size: 14px;
    }

    .msg-search-input-box input {
      background: transparent;
      border: none;
      color: #fff;
      outline: none;
      width: 100%;
      font-size: 14px;
    }

    .msg-compose-btn {
      font-size: 20px;
      color: #0a84ff;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    /* 🖼️ 사진 앱 스타일 */
    #photos-view {
      background: #000;
      color: #fff;
    }

    .photos-header {
      padding: 52px 16px 10px 16px;
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      background: #000;
    }

    .photos-title-box {
      display: flex;
      flex-direction: column;
    }

    .photos-title {
      font-size: 26px;
      font-weight: 700;
      color: #fff;
      line-height: 1.1;
    }

    .photos-subtitle {
      font-size: 13px;
      font-weight: 500;
      color: #8e8e93;
      margin-top: 3px;
    }

    .photos-actions {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .photos-btn-icon {
      width: 32px;
      height: 32px;
      background: #1c1c1e;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
      color: #fff;
      cursor: pointer;
    }

    .photos-select-btn {
      background: #1c1c1e;
      color: #fff;
      font-size: 13px;
      font-weight: 600;
      padding: 6px 14px;
      border-radius: 18px;
      cursor: pointer;
    }

    .photos-grid-container {
      flex: 1;
      overflow-y: auto;
      padding: 10px 12px 100px 12px;
    }

    /* 📸 사진 그리드 스타일 (2열 배치) */
    .photos-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 8px;
    }

    .photo-thumb {
      width: 100%;
      aspect-ratio: 0.85;
      border-radius: 12px;
      overflow: hidden;
      background: #1c1c1e;
      position: relative;
    }

    .photo-thumb img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .photos-floating-tabbar {
      position: absolute;
      bottom: 22px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      gap: 12px;
      z-index: 1000;
    }

    .photos-tab-group {
      background: rgba(30, 30, 32, 0.85);
      backdrop-filter: blur(25px);
      -webkit-backdrop-filter: blur(25px);
      border-radius: 36px;
      padding: 6px 8px;
      display: flex;
      align-items: center;
      gap: 6px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
      border: 0.5px solid rgba(255, 255, 255, 0.1);
    }

    .photos-tab-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 8px 18px;
      border-radius: 28px;
      cursor: pointer;
      color: #8e8e93;
      transition: all 0.2s ease;
    }

    .photos-tab-item.active {
      background: rgba(255, 255, 255, 0.15);
      color: #34c759;
    }

    .photos-tab-item svg {
      width: 20px;
      height: 20px;
      fill: currentColor;
    }

    .photos-tab-label {
      font-size: 11px;
      font-weight: 600;
      margin-top: 3px;
    }

    .photos-search-btn {
      background: rgba(30, 30, 32, 0.85);
      backdrop-filter: blur(25px);
      -webkit-backdrop-filter: blur(25px);
      width: 52px;
      height: 52px;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      color: #fff;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
      border: 0.5px solid rgba(255, 255, 255, 0.1);
    }

    /* 💬 카카오톡 스타일 */
    #kakao-talk-view {
      background: #000000;
      color: #ffffff;
    }

    .k-header {
      padding: 50px 16px 12px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #000000;
    }

    .k-title {
      font-size: 22px;
      font-weight: 700;
    }

    .k-icons {
      display: flex;
      gap: 16px;
      font-size: 18px;
    }

    .k-sub-filters {
      display: flex;
      gap: 8px;
      padding: 0 16px 12px 16px;
      background: #000000;
      align-items: center;
    }

    .k-pill {
      background: #222224;
      color: #fff;
      padding: 6px 12px;
      border-radius: 16px;
      font-size: 13px;
      font-weight: 600;
    }

    .k-pill.active {
      background: #fff;
      color: #000;
    }

    .k-pill-badge {
      background: #ff3b30;
      color: #fff;
      font-size: 10px;
      padding: 1px 5px;
      border-radius: 10px;
      margin-left: 4px;
    }

    .k-banner {
      margin: 0 16px 12px 16px;
      background: #1c1c1e;
      border-radius: 12px;
      padding: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .k-banner-text {
      font-size: 13px;
      color: #ccc;
    }

    .k-banner-text b {
      color: #fff;
      display: block;
      margin-bottom: 2px;
    }

    .k-chat-list {
      flex: 1;
      overflow-y: auto;
      background: #000000;
      padding-bottom: 60px;
    }

    .k-chat-item {
      display: flex;
      align-items: center;
      padding: 12px 16px;
      gap: 12px;
      cursor: pointer;
    }

    .k-chat-item:active {
      background: #1c1c1e;
    }

    .k-profile-img {
      width: 48px;
      height: 48px;
      border-radius: 18px;
      overflow: hidden;
      background: #333;
      flex-shrink: 0;
    }

    .k-profile-img img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .k-chat-info {
      flex: 1;
      min-width: 0;
    }

    .k-chat-name-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
    }

    .k-chat-name {
      font-size: 15px;
      font-weight: 600;
      color: #fff;
    }

    .k-chat-time {
      font-size: 11px;
      color: #8e8e93;
    }

    .k-chat-preview {
      font-size: 13px;
      color: #8e8e93;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .k-badge-count {
      background: #ff3b30;
      color: #fff;
      font-size: 11px;
      font-weight: 600;
      min-width: 18px;
      height: 18px;
      border-radius: 9px;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0 5px;
      margin-top: 4px;
    }

    .k-tabbar {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 55px;
      background: #121212;
      border-top: 0.5px solid #222;
      display: flex;
      justify-content: space-between;
      padding: 0 24px;
      align-items: center;
      z-index: 999;
    }

    .k-tab-item {
      font-size: 20px;
      color: #8e8e93;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      position: relative;
    }

    .k-tab-item.active {
      color: #fff;
    }

    .k-tab-badge {
      position: absolute;
      top: -2px;
      right: -8px;
      background: #ff3b30;
      color: #fff;
      font-size: 9px;
      padding: 1px 4px;
      border-radius: 8px;
    }

    /* 📱 카톡 상세 대화방 */
    #kakao-chat-room {
      background: #1e1e1e;
      color: #fff;
    }

    .kc-header {
      padding: 50px 16px 10px 16px;
      background: #1e1e1e;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 0.5px solid rgba(255,255,255,0.05);
    }

    .kc-header-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .kc-header-name {
      font-size: 16px;
      font-weight: 600;
    }

    .kc-header-right {
      display: flex;
      gap: 16px;
      font-size: 18px;
    }

    .kc-notice {
      background: #2c2c2e;
      margin: 8px 12px;
      padding: 8px 12px;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 12px;
      color: #ddd;
    }

    .kc-messages {
      flex: 1;
      overflow-y: auto;
      padding: 12px;
      display: flex;
      flex-direction: column;
      gap: 12px;
      background: #1e1e1e;
      padding-bottom: 20px;
    }

    .kc-date-divider {
      text-align: center;
      margin: 12px 0;
    }

    .kc-date-pill {
      background: rgba(0,0,0,0.3);
      color: #aaa;
      font-size: 11px;
      padding: 4px 12px;
      border-radius: 12px;
      display: inline-block;
    }

    .kc-msg-row {
      display: flex;
      align-items: flex-end;
      gap: 8px;
    }

    .kc-msg-row.them {
      align-self: flex-start;
    }

    .kc-msg-row.me {
      align-self: flex-end;
      flex-direction: row-reverse;
    }

    .kc-avatar {
      width: 36px;
      height: 36px;
      border-radius: 14px;
      overflow: hidden;
      background: #333;
      flex-shrink: 0;
      cursor: pointer;
    }

    .kc-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .kc-bubble {
      max-width: 220px;
      padding: 10px 14px;
      border-radius: 16px;
      font-size: 14px;
      line-height: 1.4;
      word-break: break-word;
    }

    .kc-msg-row.them .kc-bubble {
      background: #313133;
      color: #fff;
      border-top-left-radius: 4px;
    }

    .kc-msg-row.me .kc-bubble {
      background: #fee500;
      color: #000;
      border-top-right-radius: 4px;
      font-weight: 500;
    }

    .kc-time {
      font-size: 10px;
      color: #8e8e93;
      align-self: flex-end;
      margin-bottom: 2px;
    }

    .facetalk-card {
      background: #2c2c2e;
      border-radius: 14px;
      padding: 12px 16px;
      width: 180px;
      display: flex;
      flex-direction: column;
      gap: 6px;
      align-self: flex-start;
      margin-left: 44px;
      cursor: pointer;
      transition: background 0.2s;
    }

    .facetalk-card:active {
      background: #3a3a3c;
    }

    .facetalk-top {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 13px;
      font-weight: 600;
      color: #fff;
    }

    .facetalk-icon {
      background: #34c759;
      width: 28px;
      height: 28px;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
    }

    .facetalk-duration {
      font-size: 13px;
      color: #8e8e93;
      padding-left: 36px;
    }

    /* 📹 페이스톡 UI 스타일 (PIP 왼쪽 + 가로 탭 언더바만 남김) */
    #facetalk-view {
      background: #000;
      position: relative;
      overflow: hidden;
    }

    /* 🍏 페이스타임 UI 스타일 (페이스톡과 분리됨) */
    #facetime-view {
      background: #000;
      position: relative;
      overflow: hidden;
    }

    .ft-main-video-wrap {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }

    .ft-main-video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .ft-top-overlay {
      position: absolute;
      top: 50px;
      left: 16px;
      right: 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 10;
    }

    .ft-caller-pill {
      background: rgba(40, 40, 40, 0.6);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      padding: 6px 14px 6px 6px;
      border-radius: 30px;
      display: flex;
      align-items: center;
      gap: 10px;
      color: #fff;
    }

    .ft-caller-avatar {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      overflow: hidden;
    }

    .ft-caller-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .ft-caller-name {
      font-size: 14px;
      font-weight: 600;
    }

    .ft-top-right-btn {
      width: 40px;
      height: 40px;
      background: rgba(40, 40, 40, 0.6);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: default;
    }

    /* 페이스톡 전용 왼쪽 하단 PIP 위치 설정 */
    .facetalk-left-pip {
      position: absolute;
      left: 16px;
      bottom: 100px;
      z-index: 10;
    }

    /* 페이스톡 전용 하단 가로 언더바 패널 */
    .facetalk-bottom-bar {
      position: absolute;
      bottom: 30px;
      left: 0;
      width: 100%;
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      padding: 0 16px;
      z-index: 10;
    }

    /* 페이스타임 전용 왼쪽 하단 PIP 내 화면 영상 컨트롤 영역 */
    .ft-left-controls {
      position: absolute;
      left: 16px;
      bottom: 30px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
      z-index: 10;
    }

    /* 페이스타임 우측 하단 컨트롤 버튼 패널 (세로 정렬) */
    .ft-right-action-controls {
      position: absolute;
      right: 16px;
      bottom: 30px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
      z-index: 10;
    }

    .ft-pip-video-box {
      width: 110px;
      height: 170px;
      border-radius: 20px;
      overflow: hidden;
      background: #222;
      box-shadow: 0 8px 25px rgba(0,0,0,0.5);
      border: 1.5px solid rgba(255,255,255,0.2);
      position: relative;
    }

    .ft-pip-video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .ft-control-btn {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      background: rgba(50, 50, 50, 0.7);
      backdrop-filter: blur(15px);
      -webkit-backdrop-filter: blur(15px);
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      color: #fff;
      font-size: 18px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
      transition: transform 0.1s;
      overflow: hidden;
    }

    .ft-control-btn img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border-radius: 50%;
    }

    /* 끊기 버튼 외에는 클릭해도 반응이 없도록 pointer-events 제거 및 기본 커서 적용 */
    .ft-control-btn.inert-btn {
      cursor: default;
    }

    .ft-control-btn.end-call {
      background: #ff3b30;
      cursor: pointer;
    }

    .ft-control-btn.end-call:active {
      transform: scale(0.92);
    }

    .photo-exchange-box {
      width: 180px;
      height: 200px;
      background: #2c2c2e;
      border-radius: 12px;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
    }
    
    .photo-exchange-box img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .kc-input-area {
      padding: 10px 12px 28px 12px;
      background: #121212;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .kc-input-icon {
      font-size: 18px;
      color: #aaa;
      cursor: pointer;
    }

    .kc-input-box {
      flex: 1;
      background: #1c1c1e;
      border: none;
      border-radius: 20px;
      padding: 8px 14px;
      color: #fff;
      font-size: 14px;
      outline: none;
    }

    .kc-send-btn {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      background: #fee500;
      border: none;
      color: #000;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      font-weight: bold;
      font-size: 13px;
      flex-shrink: 0;
    }

    /* Phone App View (전화 앱 스타일) */
    #phone-view {
      background: #000;
      color: #fff;
    }

    .phone-sub-view {
      display: none;
      flex: 1;
      overflow-y: auto;
      padding-bottom: 75px;
      position: relative;
    }

    .phone-sub-view.active {
      display: flex;
      flex-direction: column;
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
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .fav-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
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
      cursor: pointer;
    }

    .call-item:active {
      background: #1c1c1e;
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
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .call-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
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

    /* 전화 앱 하단 iOS 탭바 스타일 */
    .phone-tabbar {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 55px;
      background: rgba(20, 20, 22, 0.95);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border-top: 0.5px solid rgba(255,255,255,0.1);
      display: flex;
      justify-content: space-around;
      align-items: center;
      z-index: 100;
    }
    .phone-tab-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #8e8e93;
      font-size: 10px;
      font-weight: 500;
      cursor: pointer;
      gap: 2px;
      padding: 4px 12px;
      border-radius: 20px;
    }
    .phone-tab-btn.active {
      color: #0a84ff;
      background: #2c2c2e;
    }

    /* 📞 순정 통화 연결 중 화면 스타일 */
    #audio-call-view {
      background: #000000;
      color: #fff;
      display: none;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      padding: 70px 24px 50px 24px;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 9999;
      overflow: hidden;
    }

    #audio-call-view.active {
      display: flex;
    }

    .audio-call-bg-video {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 1;
      opacity: 0.4;
    }

    .audio-call-profile-area,
    .audio-call-grid,
    .audio-call-end-container {
      position: relative;
      z-index: 2;
    }

    .audio-call-profile-area {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
      margin-top: 20px;
    }

    .audio-call-avatar {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      overflow: hidden;
      background: #333;
    }

    .audio-call-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .audio-call-name {
      font-size: 26px;
      font-weight: 600;
      color: #fff;
    }

    .audio-call-status {
      font-size: 15px;
      color: #8e8e93;
    }

    .audio-call-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 24px 32px;
      width: 100%;
      max-width: 280px;
      margin-bottom: 20px;
    }

    .audio-call-btn-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      cursor: pointer;
    }

    .audio-call-circle-icon {
      width: 64px;
      height: 64px;
      background: rgba(255, 255, 255, 0.12);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      color: #fff;
      backdrop-filter: blur(10px);
      transition: background 0.2s;
    }

    .audio-call-circle-icon:active {
      background: rgba(255, 255, 255, 0.3);
    }

    .audio-call-label {
      font-size: 12px;
      color: #fff;
      font-weight: 400;
    }

    .audio-call-end-container {
      width: 100%;
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
    }

    .audio-call-end-btn {
      width: 72px;
      height: 72px;
      background: #ff3b30;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 32px;
      color: #fff;
      cursor: pointer;
      box-shadow: 0 8px 20px rgba(255, 59, 48, 0.4);
      transition: transform 0.1s;
    }

    .audio-call-end-btn:active {
      transform: scale(0.92);
    }

    /* iMessage Chat Room View */
    #messages-chat-view {
      background: #000;
    }

    #messages-chat-view .app-header {
      padding-top: 75px;
      padding-bottom: 18px;
    }

    .msg-header-profile {
      position: absolute;
      left: 50%;
      bottom: 14px;
      transform: translateX(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      z-index: 1;
    }

    .default-avatar {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: #8e8e93;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
    }

    .default-avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
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

    .msg-date-divider {
      text-align: center;
      margin: 14px 0 6px 0;
      font-size: 11px;
      font-weight: 500;
      color: #8e8e93;
    }

    .msg-wrapper {
      display: flex;
      align-items: flex-end;
      gap: 6px;
      max-width: 85%;
    }

    .msg-wrapper.msg-them {
      align-self: flex-start;
      flex-direction: row;
    }

    .msg-wrapper.msg-me {
      align-self: flex-end;
      flex-direction: row-reverse;
    }

    .message-bubble {
      padding: 10px 14px;
      border-radius: 18px;
      font-size: 15px;
      line-height: 1.4;
      word-break: break-word;
    }

    .msg-wrapper.msg-me .message-bubble {
      background: #0a84ff;
      color: #fff;
      border-bottom-right-radius: 4px;
    }

    .msg-wrapper.msg-them .message-bubble {
      background: #262628;
      color: #fff;
      border-bottom-left-radius: 4px;
    }

    .msg-time {
      font-size: 10px;
      color: #8e8e93;
      margin-bottom: 3px;
      white-space: nowrap;
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

    /* Notes App View */
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

    /* 🎙️ 음성 메모 앱 스타일 */
    #voice-memo-view, #voice-memo-detail-view {
      background: #000;
      color: #fff;
    }

    .vm-header-actions {
      padding: 50px 20px 10px 20px;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      gap: 12px;
    }

    .vm-action-pill {
      background: rgba(255,255,255,0.18);
      backdrop-filter: blur(10px);
      color: #fff;
      padding: 6px 14px;
      border-radius: 20px;
      font-size: 14px;
      font-weight: 500;
      cursor: pointer;
    }

    .vm-main-title {
      font-size: 32px;
      font-weight: 700;
      padding: 0 20px 16px 20px;
      letter-spacing: -0.5px;
    }

    .vm-card-container {
      background: #1c1c1e;
      border-radius: 14px;
      margin: 0 16px;
      overflow: hidden;
    }

    .vm-card-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 14px 16px;
      cursor: pointer;
    }

    .vm-card-item:active {
      background: #2c2c2e;
    }

    .vm-card-item:not(:last-child) {
      border-bottom: 0.5px solid rgba(255,255,255,0.1);
    }

    .vm-card-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .vm-icon-blue {
      color: #0a84ff;
      font-size: 20px;
    }

    .vm-card-title {
      font-size: 16px;
      font-weight: 500;
      color: #fff;
    }

    .vm-card-right {
      display: flex;
      align-items: center;
      gap: 6px;
      color: #8e8e93;
      font-size: 15px;
    }

    .vm-section-header {
      font-size: 15px;
      font-weight: 600;
      color: #8e8e93;
      padding: 24px 20px 8px 20px;
    }

    .vm-detail-header {
      padding: 50px 16px 10px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .vm-header-left-btn {
      width: 32px;
      height: 32px;
      background: rgba(255,255,255,0.15);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      font-size: 20px;
      cursor: pointer;
    }

    .vm-header-right-btns {
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .vm-circle-btn {
      width: 32px;
      height: 32px;
      background: rgba(255,255,255,0.15);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      font-size: 14px;
      cursor: pointer;
    }

    .vm-list-area {
      flex: 1;
      padding: 10px 20px;
      overflow-y: auto;
    }

    .vm-record-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 0;
      border-bottom: 0.5px solid rgba(255,255,255,0.1);
      cursor: pointer;
    }

    .vm-rec-title {
      font-size: 16px;
      font-weight: 600;
      color: #fff;
      margin-bottom: 3px;
    }

    .vm-rec-time {
      font-size: 13px;
      color: #8e8e93;
    }

    .vm-rec-duration {
      font-size: 13px;
      color: #8e8e93;
    }

    .vm-bottom-record-bar {
      height: 100px;
      display: flex;
      justify-content: center;
      align-items: center;
      padding-bottom: 20px;
    }

    .vm-red-record-btn {
      width: 62px;
      height: 62px;
      border-radius: 50%;
      background: #ff3b30;
      border: 3px solid #000;
      outline: 2px solid #ff3b30;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: transform 0.1s;
    }

    .vm-red-record-btn:active {
      transform: scale(0.92);
    }

    .vm-red-record-inner {
      width: 24px;
      height: 24px;
      background: #ff3b30;
      border-radius: 50%;
    }

    /* Home Bar */
    .home-bar {
      position: absolute;
      bottom: 8px;
      left: 50%;
      transform: translateX(-50%);
      width: 134px;
      height: 5px;
      background: #fff;
      border-radius: 3px;
      z-index: 9999;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <!-- 공통 이미지 업로드용 파일 input -->
  <input type="file" id="imageUploadInput" accept="image/*" style="display: none;" onchange="handleImageUpload(event)">

  <div class="phone-mockup" id="phoneMockup">
    <!-- Dynamic Island (FaceTime 수신 알림 지원) -->
    <div class="dynamic-island" id="dynamicIsland">
      <div class="di-facetime-content">
        <div class="di-ft-left">
          <div class="di-ft-avatar">
            <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌이">
          </div>
          <div class="di-ft-text">
            <span class="di-ft-sub">FaceTime 영상 통화</span>
            <span class="di-ft-name">지헌이❤️</span>
          </div>
        </div>
        <div class="di-ft-actions">
          <div class="di-ft-btn di-ft-decline" onclick="declineFaceTimeCall(event)">
            <img src="https://i.postimg.cc/dQWVhWN3/58A45F67-CC3E-4B05-A4FE-63A577D77EE7.jpg" alt="거절">
          </div>
          <div class="di-ft-btn di-ft-accept" onclick="acceptFaceTimeCall(event)">
            <img src="https://i.postimg.cc/CK9msZCP/C113850C-AA21-42F5-B43D-95F1AB7B4757.jpg" alt="수신">
          </div>
        </div>
      </div>
    </div>

    <!-- Status Bar -->
    <div class="status-bar">
      <span class="status-left" id="status-bar-time" style="visibility: hidden;">23:48</span>
      <div class="status-right"></div>
    </div>

    <!-- 🔔 인스타그램 푸시 알림 배너 -->
    <div id="insta-banner" onclick="openInstaStory()">
      <div class="insta-banner-icon"></div>
      <div class="insta-banner-content">
        <div class="insta-banner-top">
          <span class="insta-banner-title">Instagram</span>
          <span class="insta-banner-time">지금</span>
        </div>
        <div class="insta-banner-text">honeyheon_님이 스토리를 추가했습니다.</div>
      </div>
    </div>

    <!-- 💬 카카오톡 푸시 알림 배너 -->
    <div id="kakao-banner" onclick="openApp('kakao-talk-view'); hideKakaoNotification();">
      <div class="kakao-banner-icon"></div>
      <div class="kakao-banner-content">
        <div class="kakao-banner-top">
          <span class="kakao-banner-title">지헌이❤️</span>
          <span class="kakao-banner-time">지금</span>
        </div>
        <div class="kakao-banner-text">스토리 봐써? ㅎㅎ</div>
      </div>
    </div>

    <div class="screen">
      <!-- 0. LOCK SCREEN VIEW -->
      <div id="lock-view" class="view active" onclick="showPasscode()">
        <div class="lock-content">
          <div class="lock-date">
            <span id="lock-date-text">19일 (월)</span>
          </div>
          <div class="lock-time" id="lock-time-text">23:48</div>
        </div>
      </div>

      <!-- 🔑 암호 입력 화면 -->
      <div id="passcode-view" class="view">
        <div class="passcode-header">암호 입력</div>
        <div class="passcode-dots">
          <div class="dot" id="dot-0"></div>
          <div class="dot" id="dot-1"></div>
          <div class="dot" id="dot-2"></div>
          <div class="dot" id="dot-3"></div>
        </div>
        <div class="keypad">
          <div class="key" onclick="pressKey('1')">1<span class="sub">&nbsp;</span></div>
          <div class="key" onclick="pressKey('2')">2<span class="sub">ABC</span></div>
          <div class="key" onclick="pressKey('3')">3<span class="sub">DEF</span></div>
          <div class="key" onclick="pressKey('4')">4<span class="sub">GHI</span></div>
          <div class="key" onclick="pressKey('5')">5<span class="sub">JKL</span></div>
          <div class="key" onclick="pressKey('6')">6<span class="sub">MNO</span></div>
          <div class="key" onclick="pressKey('7')">7<span class="sub">PQRS</span></div>
          <div class="key" onclick="pressKey('8')">8<span class="sub">TUV</span></div>
          <div class="key" onclick="pressKey('9')">9<span class="sub">WXYZ</span></div>
          <div class="key empty"></div>
          <div class="key" onclick="pressKey('0')">0<span class="sub">&nbsp;</span></div>
          <div class="key empty" onclick="deleteKey()" style="font-size:22px; display:flex; justify-content:center; align-items:center; cursor:pointer;">⌫</div>
        </div>
        <div class="passcode-bottom-actions">
          <span onclick="alert('긴급 상황')">긴급 상황</span>
          <span onclick="hidePasscode()">취소</span>
        </div>
      </div>

      <!-- 1. HOME SCREEN VIEW -->
      <div id="home-view" class="view">
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
          <div class="app-icon" onclick="openApp('photos-view')">
            <div class="icon-box icon-photos"></div>
            <span class="app-name">사진</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-translate"></div>
            <span class="app-name">연락처</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-weather"></div>
            <span class="app-name">날씨</span>
          </div>
          <div class="app-icon">
            <div class="icon-box icon-appstore"></div>
            <span class="app-name">App Store</span>
          </div>
          <div class="app-icon" onclick="openApp('kakao-talk-view')">
            <div class="icon-box icon-kakaotalk"></div>
            <span class="app-name">카카오톡</span>
          </div>
          <div class="app-icon" onclick="openInstaFeed()">
            <div class="icon-box icon-instagram"></div>
            <span class="app-name">Instagram</span>
          </div>
          <div class="app-icon" onclick="openApp('voice-memo-view')">
            <div class="icon-box icon-voice-memo"></div>
            <span class="app-name">음성 메모</span>
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

      <!-- 📸 INSTAGRAM FEED VIEW -->
      <div id="insta-feed-view" class="view">
        <div class="insta-feed-header">
          <span class="insta-logo-text">Instagram</span>
          <div class="insta-header-icons">
            <span>♥</span>
            <span>✈︎</span>
          </div>
        </div>

        <div class="insta-feed-scroll">
          <div class="insta-stories-bar">
            <!-- 내 프로필 -->
            <div class="insta-story-item" onclick="openMyInstaProfile()">
              <div class="insta-story-ring viewed" style="background:none; border:1px solid #000;">
                <img src="https://i.postimg.cc/zBMfrzdr/IMG-3549.jpg" alt="내 스토리에 추가">
                <div class="insta-story-plus">+</div>
              </div>
              <span class="insta-story-name">내 스토리</span>
            </div>
            <!-- 지헌이 스토리 -->
            <div class="insta-story-item" onclick="openInstaStory()">
              <div class="insta-story-ring" id="feed-story-ring">
                <img src="https://i.postimg.cc/xdstPhd4/DB003073-C611-4D9F-8657-D63B34BF8535.jpg" alt="honeyheon_">
              </div>
              <span class="insta-story-name">honeyheon_</span>
            </div>
            <!-- 친구 1 -->
            <div class="insta-story-item">
              <div class="insta-story-ring">
                <img src="https://i.postimg.cc/1XCf40RN/IMG-9850.jpg" alt="강민영">
              </div>
              <span class="insta-story-name">ssaerrommm</span>
            </div>
            <!-- 친구 2 -->
            <div class="insta-story-item">
              <div class="insta-story-ring">
                <img src="https://i.postimg.cc/N0cT34Ks/163643CB-B998-43F2-9A62-3C0CC7E37C58.jpg" alt="고다은">
              </div>
              <span class="insta-story-name">nyang_ha</span>
            </div>
            <!-- 친구 3 -->
            <div class="insta-story-item">
              <div class="insta-story-ring">
                <img src="https://i.postimg.cc/25Dvsw15/0683D904-6437-49F8-AC1B-D2F48B26CC01.jpg" alt="유세빈">
              </div>
              <span class="insta-story-name">ys_y</span>
            </div>
          </div>

          <div class="insta-post">
            <div class="insta-post-header">
              <div class="insta-post-user" onclick="openInstaProfile()">
                <div class="insta-post-avatar" id="post-avatar-ring" onclick="event.stopPropagation(); openInstaStory();">
                  <div class="insta-post-avatar-inner">
                    <img src="https://i.postimg.cc/xdstPhd4/DB003073-C611-4D9F-8657-D63B34BF8535.jpg" alt="프로필">
                  </div>
                </div>
                <span class="insta-post-username">honeyheon_</span>
              </div>
              <span style="color:#aaa; cursor:pointer;">•••</span>
            </div>

            <div class="insta-post-image">
              <img src="https://i.postimg.cc/zvgGDsQQ/IMG-9611.jpg" alt="게시물 사진">
            </div>

            <div class="insta-post-actions">
              <div class="insta-actions-left">
                <span>♥</span>
                <span></span>
                <span>✈︎</span>
              </div>
              <span></span>
            </div>

            <div class="insta-post-likes">좋아요 1,284개</div>
            <div class="insta-post-caption">
              <b>honeyheon_</b>오랜만에.. ㅋㅋㅋ
            </div>
            <div class="insta-post-time">1주 전</div>
          </div>
        </div>

        <div class="insta-tabbar">
          <span class="insta-tab-icon active" onclick="openInstaFeed()">☗</span>
          <span class="insta-tab-icon"></span>
          <span class="insta-tab-icon">▶</span>
          <span class="insta-tab-icon"></span>
          <div class="insta-my-avatar-icon" onclick="openMyInstaProfile()">
            <img src="https://i.postimg.cc/zBMfrzdr/IMG-3549.jpg" alt="내 프로필">
          </div>
        </div>
      </div>

      <!-- 👤 INSTAGRAM USER PROFILE VIEW -->
      <div id="insta-profile-view" class="view">
        <div class="insta-profile-header">
          <span class="back-btn" onclick="openInstaFeed()" style="font-size:22px; color:#fff; z-index:2;">‹</span>
          <span style="font-size:15px; font-weight:700; color:#ffffff; position:absolute; left:50%; transform:translateX(-50%);">honeyheon_</span>
          <div style="display:flex; gap:16px; font-size:18px; z-index:2;">
            <span></span>
            <span>•••</span>
          </div>
        </div>

        <div class="insta-profile-scroll-body">
          <div class="insta-profile-top-info">
            <div class="insta-profile-row">
              <div class="insta-profile-avatar-ring" id="profile-avatar-ring" onclick="openInstaStory()">
                <div class="insta-profile-avatar-large">
                  <img src="https://i.postimg.cc/xdstPhd4/DB003073-C611-4D9F-8657-D63B34BF8535.jpg" alt="honeyheon_ 프로필">
                </div>
              </div>
              <div class="insta-profile-stats">
                <div class="insta-stat-item"><span class="insta-stat-num">3</span><span class="insta-stat-label">게시물</span></div>
                <div class="insta-stat-item"><span class="insta-stat-num">1.2만</span><span class="insta-stat-label">팔로워</span></div>
                <div class="insta-stat-item"><span class="insta-stat-num">532</span><span class="insta-stat-label">팔로잉</span></div>
              </div>
            </div>

            <div class="insta-profile-bio">
              <span class="insta-bio-name">백지헌</span>
              <span class="insta-bio-text">✨🩵<br></span>
            </div>
          </div>

          <div class="insta-mutual-followers">
            <div class="mutual-avatars">
              <div class="mutual-avatar"><img src="https://i.postimg.cc/1XCf40RN/IMG-9850.jpg" alt=""></div>
              <div class="mutual-avatar"><img src="https://i.postimg.cc/N0cT34Ks/163643CB-B998-43F2-9A62-3C0CC7E37C58.jpg" alt=""></div>
              <div class="mutual-avatar"><img src="https://i.postimg.cc/25Dvsw15/0683D904-6437-49F8-AC1B-D2F48B26CC01.jpg" alt=""></div>
            </div>
            <span><b>ssaerrommm님, nyang_ha님</b> 외 23명이 팔로우합니다</span>
          </div>

          <div class="insta-profile-buttons">
            <div class="insta-btn" style="background:#262626;">팔로잉</div>
            <div class="insta-btn" style="background:#262626;">메시지</div>
            <div class="insta-btn-icon">+</div>
          </div>

          <div class="insta-highlights-bar">
            <div class="insta-highlight-item"><div class="insta-highlight-ring"><img src="https://i.postimg.cc/ZRgqSbwM/IMG-2604.jpg" alt=""></div><span class="insta-highlight-name">me</span></div>
            <div class="insta-highlight-item"><div class="insta-highlight-ring"><img src="https://i.postimg.cc/RF80BSgw/IMG-2488.jpg" alt=""></div><span class="insta-highlight-name">💋</span></div>
            <div class="insta-highlight-item"><div class="insta-highlight-ring"><img src="https://i.postimg.cc/76cLkHKt/IMG-1103.jpg" alt=""></div><span class="insta-highlight-name">💗</span></div>
            <div class="insta-highlight-item"><div class="insta-highlight-ring"><img src="https://i.postimg.cc/50GtV4Pd/IMG-3493.jpg" alt=""></div><span class="insta-highlight-name">travel</span></div>
          </div>

          <div class="insta-grid-tabs">
            <div class="insta-grid-tab active">▦</div>
            <div class="insta-grid-tab">▶</div>
            <div class="insta-grid-tab"></div>
          </div>

          <div class="insta-user-grid">
            <div class="insta-grid-item"><img src="https://i.postimg.cc/zvgGDsQQ/IMG-9611.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/xC7d2f6s/IMG-7610.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/C1tKghcX/IMG-3419.jpg" alt=""></div>
          </div>
        </div>

        <div class="insta-tabbar">
          <span class="insta-tab-icon" onclick="openInstaFeed()">☗</span>
          <span class="insta-tab-icon"></span>
          <span class="insta-tab-icon">▶</span>
          <span class="insta-tab-icon"></span>
          <div class="insta-my-avatar-icon" onclick="openMyInstaProfile()">
            <img src="https://i.postimg.cc/zBMfrzdr/IMG-3549.jpg" alt="내 프로필">
          </div>
        </div>
      </div>

      <!-- 📸 INSTAGRAM MY PROFILE VIEW -->
      <div id="insta-my-profile-view" class="view">
        <div class="insta-profile-header">
          <span class="back-btn" onclick="openInstaFeed()" style="font-size:22px; color:#fff; z-index:2;">‹</span>
          <span style="font-size:15px; font-weight:700; color:#ffffff; position:absolute; left:50%; transform:translateX(-50%);">l2eechang_0</span>
          <div style="display:flex; gap:16px; font-size:18px; z-index:2;">
            <span>+</span>
            <span>≡</span>
          </div>
        </div>

        <div class="insta-profile-scroll-body">
          <div class="insta-profile-top-info">
            <div class="insta-profile-row">
              <div class="insta-profile-avatar-ring" style="background:none; border:2px solid #333;">
                <div class="insta-profile-avatar-large">
                  <img src="https://i.postimg.cc/zBMfrzdr/IMG-3549.jpg" alt="내 프로필">
                </div>
              </div>
              <div class="insta-profile-stats">
                <div class="insta-stat-item"><span class="insta-stat-num">6</span><span class="insta-stat-label">게시물</span></div>
                <div class="insta-stat-item"><span class="insta-stat-num">1.1만</span><span class="insta-stat-label">팔로워</span></div>
                <div class="insta-stat-item"><span class="insta-stat-num">280</span><span class="insta-stat-label">팔로잉</span></div>
              </div>
            </div>

            <div class="insta-profile-bio">
              <span class="insta-bio-name">이채영</span>
              <span class="insta-bio-text">✨🩵</span>
            </div>
          </div>

          <div class="insta-profile-buttons">
            <div class="insta-btn">프로필 편집</div>
            <div class="insta-btn">프로필 공유</div>
          </div>

          <div class="insta-grid-tabs">
            <div class="insta-grid-tab active">▦</div>
            <div class="insta-grid-tab">▶</div>
            <div class="insta-grid-tab"></div>
          </div>

          <div class="insta-user-grid">
            <div class="insta-grid-item"><img src="https://i.postimg.cc/0Ndq28cz/FB9DA08F-7311-4090-A9CD-ED200C84DD8E.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/wMwQHbtN/IMG-2894.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/BZpgxtmg/C0138260-1DAD-42E0-9DF0-0023406FCF79.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/J0r5fWzP/A8CFD8BE-136B-4DDC-BE41-3FEB7D09E9F1.jpg" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/qq1x0Ztn/IMG-8522.png" alt=""></div>
            <div class="insta-grid-item"><img src="https://i.postimg.cc/4yWQZFKH/17265D12-CC13-41A9-BDEC-FBCAA60922A0.jpg" alt=""></div>
          </div>
        </div>

        <div class="insta-tabbar">
          <span class="insta-tab-icon" onclick="openInstaFeed()">☗</span>
          <span class="insta-tab-icon"></span>
          <span class="insta-tab-icon">▶</span>
          <span class="insta-tab-icon"></span>
          <div class="insta-my-avatar-icon active" onclick="openMyInstaProfile()" style="border: 2px solid #fff;">
            <img src="https://i.postimg.cc/zBMfrzdr/IMG-3549.jpg" alt="내 프로필">
          </div>
        </div>
      </div>

      <!-- 📸 INSTAGRAM STORY VIEW -->
      <div id="insta-story-view" class="view">
        <div class="story-container">
          <img class="story-bg-image" src="https://i.postimg.cc/DwJDm2yz/404F6B5B-EC71-4B22-9C67-683EBC432391.jpg" alt="스토리 사진">
          
          <div class="story-header">
            <div class="story-bars">
              <div class="story-bar"><div class="story-bar-fill"></div></div>
            </div>
            <div class="story-user-info">
              <div class="story-profile-left">
                <div class="story-profile-img">
                  <img src="https://i.postimg.cc/xdstPhd4/DB003073-C611-4D9F-8657-D63B34BF8535.jpg" alt="프로필">
                </div>
                <span class="story-username">honeyheon_</span>
                <span class="story-time">방금 전</span>
              </div>
              <button class="story-close-btn" onclick="closeInstaStory()">×</button>
            </div>
          </div>

          <div class="story-footer">
            <input type="text" class="story-input" placeholder="honeyheon_님에게 메시지 보내기...">
            <span class="story-action-icon">❤️</span>
            <span class="story-action-icon"></span>
          </div>
        </div>
      </div>

      <!-- 🖼️ PHOTOS APP VIEW -->
      <div id="photos-view" class="view">
        <div class="photos-header">
          <div class="photos-title-box">
            <div class="photos-title">보관함</div>
            <div class="photos-subtitle">60530장</div>
          </div>
          <div class="photos-actions">
            <div class="photos-btn-icon">≡</div>
            <div class="photos-select-btn">선택</div>
          </div>
        </div>

        <div class="photos-grid-container">
          <div class="photos-grid" id="photos-grid"></div>
        </div>

        <div class="photos-floating-tabbar">
          <div class="photos-tab-group">
            <div class="photos-tab-item active">
              <svg viewBox="0 0 24 24"><path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H5V5h14v14zm-5.04-6.71l-2.75 3.54-1.96-2.36L6.5 17h11l-3.54-4.71z"/></svg>
              <span class="photos-tab-label">보관함</span>
            </div>
            <div class="photos-tab-item">
              <svg viewBox="0 0 24 24"><path d="M4 6h16v2H4zm0 5h16v2H4zm0 5h16v2H4z"/></svg>
              <span class="photos-tab-label">모음</span>
            </div>
          </div>
          <div class="photos-search-btn">
            <svg viewBox="0 0 24 24" style="width:22px; height:22px; fill:none; stroke:currentColor; stroke-width:2.5;"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
          </div>
        </div>
      </div>

      <!-- 🎙️ VOICE MEMO MAIN VIEW -->
      <div id="voice-memo-view" class="view">
        <div class="vm-header-actions">
          <div class="vm-action-pill">+</div>
          <div class="vm-action-pill">편집</div>
        </div>
        
        <div class="vm-main-title">음성 메모</div>

        <div class="vm-card-container">
          <div class="vm-card-item" onclick="openApp('voice-memo-detail-view')">
            <div class="vm-card-left">
              <span class="vm-icon-blue"></span>
              <span class="vm-card-title">모든 녹음 항목</span>
            </div>
            <div class="vm-card-right">
              <span>1</span>
              <span>›</span>
            </div>
          </div>
        </div>

        <div class="vm-section-header">나의 폴더</div>

        <div class="vm-card-container">
          <div class="vm-card-item">
            <div class="vm-card-left">
              <span class="vm-icon-blue">▶</span>
              <span class="vm-card-title">그냥</span>
            </div>
            <div class="vm-card-right">
              <span>1</span>
              <span>›</span>
            </div>
          </div>
          <div class="vm-card-item">
            <div class="vm-card-left">
              <span class="vm-icon-blue">▶</span>
              <span class="vm-card-title">스피치</span>
            </div>
            <div class="vm-card-right">
              <span>0</span>
              <span>›</span>
            </div>
          </div>
        </div>
      </div>

      <!-- 🎙️ VOICE MEMO DETAIL VIEW -->
      <div id="voice-memo-detail-view" class="view">
        <div class="vm-detail-header">
          <div class="vm-header-left-btn" onclick="openApp('voice-memo-view')">‹</div>
          <div class="vm-header-right-btns">
            <div class="vm-circle-btn"></div>
            <div class="vm-action-pill">선택</div>
          </div>
        </div>

        <div class="vm-main-title" style="padding-top:10px;">모든 녹음 항목</div>

        <div class="vm-list-area">
          <div class="vm-record-row">
            <div>
              <div class="vm-rec-title">새로운 녹음</div>
              <div class="vm-rec-time">14:09</div>
            </div>
            <div class="vm-rec-duration">0:04</div>
          </div>
        </div>

        <div class="vm-bottom-record-bar">
          <div class="vm-red-record-btn" onclick="toggleVoiceRecording()">
            <div class="vm-red-record-inner" id="vmRedBtnInner"></div>
          </div>
        </div>
      </div>

      <!-- 💬 카카오톡 채팅 목록 화면 -->
      <div id="kakao-talk-view" class="view">
        <div class="k-header">
          <div class="k-title">채팅</div>
          <div class="k-icons">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>

        <div class="k-sub-filters">
          <div class="k-pill active">전체</div>
          <div class="k-pill">안읽음 <span class="k-pill-badge" id="total-unread-badge">3</span></div>
          <div class="k-pill">ChatGPT</div>
          <div class="k-pill">통화</div>
        </div>

        <div class="k-banner">
          <div class="k-banner-text">
            <b>오늘의 카카오가 궁금하다면?</b>
            카카오소식 보러가기
          </div>
          <span style="font-size:20px;">📣</span>
        </div>

        <div class="k-chat-list">
          <div class="k-chat-item" onclick="openKakaoChatRoom()">
            <div class="k-profile-img">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌이">
            </div>
            <div class="k-chat-info">
              <div class="k-chat-name-row">
                <span class="k-chat-name">지헌이❤️</span>
                <span class="k-chat-time" id="jiheon-list-time">23:48</span>
              </div>
              <div class="k-chat-preview" id="jiheon-list-preview">스토리 봐써? ㅎㅎ</div>
            </div>
            <div class="k-badge-count" id="jiheon-badge-count">1</div>
          </div>

          <div class="k-chat-item">
            <div class="k-profile-img">
              <img src="https://i.postimg.cc/VvdYxnr5/C0324339-DF8F-4075-AB47-0DFE93BB57D9.jpg" alt="김현석 파트장님">
            </div>
            <div class="k-chat-info">
              <div class="k-chat-name-row">
                <span class="k-chat-name">김현석 파트장님</span>
                <span class="k-chat-time">금요일</span>
              </div>
              <div class="k-chat-preview">다음 주 일정 공유해 드립니다. 확인 부탁드립니다.</div>
            </div>
          </div>

          <div class="k-chat-item">
            <div class="k-profile-img">
              <img src="https://i.postimg.cc/sfmsVVp5/IMG-0354.jpg" alt="유지애 위원님">
            </div>
            <div class="k-chat-info">
              <div class="k-chat-name-row">
                <span class="k-chat-name">유지애 위원님</span>
                <span class="k-chat-time">목요일</span>
              </div>
              <div class="k-chat-preview">네 수고하셨습니다 위원님!</div>
            </div>
          </div>

          <div class="k-chat-item">
            <div class="k-profile-img">
              <img src="https://i.postimg.cc/VvdYxnr5/C0324339-DF8F-4075-AB47-0DFE93BB57D9.jpg" alt="언니1">
            </div>
            <div class="k-chat-info">
              <div class="k-chat-name-row">
                <span class="k-chat-name">언니1</span>
                <span class="k-chat-time">수요일</span>
              </div>
              <div class="k-chat-preview">ㅇ</div>
            </div>
            <div class="k-badge-count">2</div>
          </div>
        </div>

        <div class="k-tabbar">
          <div class="k-tab-item" onclick="goHome()"></div>
          <div class="k-tab-item active"></div>
          <div class="k-tab-item"></div>
          <div class="k-tab-item"></div>
          <div class="k-tab-item"></div>
        </div>
      </div>

      <!-- 📱 카톡 상세 대화방 -->
      <div id="kakao-chat-room" class="view">
        <div class="kc-header">
          <div class="kc-header-left">
            <span class="back-btn" onclick="returnToKakaoList()" style="color: #fff;">‹ 2</span>
            <span class="kc-header-name">지헌이❤️</span>
          </div>
          <div class="kc-header-right">
            <span></span>
            <span></span>
            <span>≡</span>
          </div>
        </div>

        <div class="kc-notice">
          <span>백지헌바보팅이ㅣㅣ</span>
          <span>✕</span>
        </div>

        <div class="kc-messages" id="kakao-chat-container">
          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">톡 왜 안봐?</div>
            <div class="kc-time">23:26</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">걱정되게...</div>
            <div class="kc-time">23:28</div>
          </div>

          <div class="kc-date-divider">
            <span class="kc-date-pill">2026년 10월 19일 월요일</span>
          </div>

          <div class="facetalk-card" onclick="openFaceTalkVideo()">
            <div class="facetalk-top">
              <div class="facetalk-icon">▶</div>
              <span>페이스톡 종료</span>
            </div>
            <div class="facetalk-duration">1:12:11</div>
            <div style="font-size:10px; color:#fff; text-align:right; margin-top:2px;">01:43</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble" style="line-height:1.5;">
              [네이버지도]<br>
              umoae<br>
              서울 용산구 한강대로84길 21-17 1층<br>
              <a href="https://naver.me/FdqG58Et" target="_blank" style="color:#4da6ff; text-decoration:underline;">https://naver.me/FRoUjnpK</a>
            </div>
            <div class="kc-time">11:21</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="photo-exchange-box">
              <img src="https://i.postimg.cc/gJLjSs41/IMG-1579.jpg" alt="주고받은 사진">
            </div>
            <div class="kc-time">22:32</div>
          </div>

          <div class="kc-msg-row me">
            <div class="kc-bubble">이뿌넹💗💗</div>
            <div class="kc-time">22:43</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">흥</div>
            <div class="kc-time">23:06</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">스토리 봐써? ㅎㅎ</div>
            <div class="kc-time">23:48</div>
          </div>
        </div>

        <div class="kc-input-area">
          <span class="kc-input-icon">➕</span>
          <input type="text" id="kakao-chat-input" class="kc-input-box" placeholder="메시지 입력">
          <span class="kc-input-icon">#</span>
          <button class="kc-send-btn" onclick="sendKakaoMessage()">↑</button>
        </div>
      </div>

      <!-- 📹 카카오톡 페이스톡 전용 화면 (상단 탭 제거 및 언더바 탭만 유효) -->
      <div id="facetalk-view" class="view">
        <div class="ft-main-video-wrap">
          <video id="facetalkVideoPlayer" class="ft-main-video" autoplay loop playsinline webkit-playsinline preload="auto">
            <source id="facetalkVideoSource" src="https://ik.imagekit.io/ewslo2qz1/ScreenRecording_09-23-2026%2022-29-45_1.mov" type="video/mp4">
          </video>
        </div>

        <!-- 왼쪽 하단 PIP (내 화면 영상) -->
        <div class="facetalk-left-pip">
          <div class="ft-pip-video-box">
            <video id="myPipVideoPlayer" class="ft-pip-video" autoplay loop muted playsinline webkit-playsinline preload="auto">
              <source src="https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2021.mov/ik-video.mp4?updatedAt=1792585829522" type="video/mp4">
            </video>
          </div>
        </div>

        <!-- 하단 언더바 가로 탭 패널 (통화 종료만 동작) -->
        <div class="facetalk-bottom-bar">
          <!-- 탭 1 -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/pLz40hJq/28E1C5AF-ACA7-4A87-9231-95D836E4915A.jpg" alt="탭1">
          </div>
          
          <!-- 탭 2 -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/cLw2DgBT/CD1CB754-A38C-4570-8C56-B7EB225BCC82.jpg" alt="탭2">
          </div>
          
          <!-- 탭 3: 통화 종료 탭 (클릭 시 정상 종료 동작) -->
          <div class="ft-control-btn end-call" onclick="closeFaceTalkVideo()">
            <img src="https://i.postimg.cc/dQWVhWN3/58A45F67-CC3E-4B05-A4FE-63A577D77EE7.jpg" alt="통화종료">
          </div>
          
          <!-- 탭 4 -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/3xmVL4FC/73250111-3256-4274-988E-B38FC0F75BF3.jpg" alt="탭4">
          </div>
          
          <!-- 탭 5 -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/gkf1qq83/4313BB15-DD81-46A8-919A-3BA1A31A998F.jpg" alt="탭5">
          </div>
        </div>
      </div>

      <!-- 🍏 애플 페이스타임 전용 화면 -->
      <div id="facetime-view" class="view">
        <div class="ft-main-video-wrap">
          <video id="facetimeVideoPlayer" class="ft-main-video" autoplay loop playsinline webkit-playsinline preload="auto">
            <source id="facetimeVideoSource" src="https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2020-57-51_1.mov/ik-video.mp4?updatedAt=1792585437006" type="video/mp4">
          </video>
        </div>

        <div class="ft-top-overlay">
          <div class="ft-caller-pill">
            <div class="ft-caller-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="지헌이">
            </div>
            <span class="ft-caller-name">지헌이❤️</span>
          </div>
          <div class="ft-top-right-btn">
            <svg viewBox="0 0 24 24" style="width:20px; height:20px; fill:#fff;"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-2h2v2zm0-4h-2V7h2v6z"/></svg>
          </div>
        </div>

        <!-- 왼쪽 하단에 위치한 PIP 내 화면 영상 및 전환 버튼 영역 -->
        <div class="ft-left-controls">
          <div class="ft-pip-video-box">
            <video id="myFacetimePipVideoPlayer" class="ft-pip-video" autoplay loop muted playsinline webkit-playsinline preload="auto">
              <source src="https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2020-50-08_1.mov/ik-video.mp4?updatedAt=1792585554041" type="video/mp4">
            </video>
          </div>
          <!-- 첫 번째 버튼: 이미지 링크 삽입 가능 (클릭 반응 없음) -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/LsdJkfQw/74CF011F-1353-43D2-A648-14F5AEC6E599.jpg" alt="버튼1">
          </div>
        </div>

        <!-- 우측 하단 컨트롤 버튼 패널 (세로 정렬) -->
        <div class="ft-right-action-controls">
          <!-- 두 번째 버튼: 이미지 링크 삽입 가능 (클릭 반응 없음) -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/tTCmB1YC/F5A28033-BDDD-4771-8A8A-057A0C104349.jpg" alt="버튼2">
          </div>
          <!-- 세 번째 버튼: 이미지 링크 삽입 가능 (클릭 반응 없음) -->
          <div class="ft-control-btn inert-btn">
            <img src="https://i.postimg.cc/hjP32Jfc/31FD42D5-1868-4709-B62A-1729FD2FAB76.jpg" alt="버튼3">
          </div>
          <!-- 네 번째 버튼: 통화 종료 버튼 (이미지 링크 삽입 가능 및 클릭 시 정상 작동) -->
          <div class="ft-control-btn end-call" onclick="closeFaceTimeVideo()">
            <img src="https://i.postimg.cc/tTCmB1Y4/44B95DE8-872C-41FA-82E4-44EE4F8A3207.jpg" alt="통화 종료">
          </div>
        </div>
      </div>

      <!-- 📞 순정 통화 연결 중 화면 -->
      <div id="audio-call-view">
        <video id="audioCallBgVideo" class="audio-call-bg-video" playsinline webkit-playsinline preload="auto">
          <source id="audioCallBgSource" src="https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2018-18-52_1.mov/ik-video.mp4?updatedAt=1792575791181" type="video/mp4">
        </video>

        <div class="audio-call-profile-area">
          <div class="audio-call-avatar">
            <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="지헌이❤️">
          </div>
          <div class="audio-call-name">지헌이❤️</div>
          <div class="audio-call-status">iPhone 연결 중...</div>
        </div>

        <div class="audio-call-grid">
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/pXHyb9St/E1A0F6BB-7BC1-4974-8383-25AF18077533.jpg" alt="소리 끔" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">소리 끔</div>
          </div>
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/gkWrFw7b/F33D57E7-AD5A-4446-90E9-16CBE822E4E4.jpg" alt="키패드" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">키패드</div>
          </div>
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/j2GVzJJh/5C86D0EF-F024-496A-8CA5-8AD819882564.jpg" alt="스피커" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">스피커</div>
          </div>
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/nr6zCrFv/462B7FC1-3472-42F2-B3E8-1BAC13D05298.jpg" alt="통화 추가" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">통화 추가</div>
          </div>
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/sfS1NPwn/EA5FDE4A-1085-466E-8800-B1E7B4005C9C.jpg" alt="FaceTime" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">FaceTime</div>
          </div>
          <div class="audio-call-btn-item">
            <div class="audio-call-circle-icon">
              <img src="https://i.postimg.cc/GtMFNkj9/D50AE200-89B9-4BD2-9192-301728D71E82.jpg" alt="연락처" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
            </div>
            <div class="audio-call-label">연락처</div>
          </div>
        </div>

        <div class="audio-call-end-container">
          <div class="audio-call-end-btn" onclick="closeAudioCall()">
            <img src="https://i.postimg.cc/dQWVhWN3/58A45F67-CC3E-4B05-A4FE-63A577D77EE7.jpg" alt="통화 종료" style="width:100%; height:100%; object-fit:cover; border-radius:50%;">
          </div>
        </div>
      </div>

      <!-- 📞 PHONE APP VIEW -->
      <div id="phone-view" class="view">
        <div id="phone-sub-recents" class="phone-sub-view active">
          <div class="app-header">
            <span class="back-btn" onclick="goHome()">‹ 홈</span>
            <span class="app-title">통화</span>
            <span style="color:#0a84ff; font-size:14px;">편집</span>
          </div>
          <div class="favorites-section">
            <div class="fav-card">
              <div class="fav-avatar">
                <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="지헌이❤️">
              </div>
              <span style="font-size:12px; font-weight:600;">지헌이❤️</span>
            </div>
          </div>
          <div class="call-list">
            <div class="call-item" onclick="openAudioCall()">
              <div class="call-info">
                <div class="call-avatar">
                  <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필">
                </div>
                <div>
                  <div class="call-name">지헌이❤️</div>
                  <div class="call-type">↙ 휴대폰</div>
                </div>
              </div>
              <div class="call-meta"><div class="call-date">12:38</div></div>
            </div>
            <div class="call-item">
              <div class="call-info">
                <div class="call-avatar"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필"></div>
                <div>
                  <div class="call-name" style="color:#fff;">지헌이❤️</div>
                  <div class="call-type">↗ 휴대폰</div>
                </div>
              </div>
              <div class="call-meta"><div class="call-date">어제</div><div class="call-duration">3시간 12분</div></div>
            </div>
            <div class="call-item">
              <div class="call-info">
                <div class="call-avatar"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필"></div>
                <div>
                  <div class="call-name" style="color:#fff;">지헌이❤️</div>
                  <div class="call-type">↙ 카카오톡 영상통화</div>
                </div>
              </div>
              <div class="call-meta"><div class="call-date">토요일</div><div class="call-duration">1시간 12분</div></div>
            </div>
            <div class="call-item">
              <div class="call-info">
                <div class="call-avatar"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필"></div>
                <div>
                  <div class="call-name" style="color:#fff;">지헌이❤️</div>
                  <div class="call-type">↙ 휴대폰</div>
                </div>
              </div>
              <div class="call-meta"><div class="call-date">목요일</div><div class="call-duration">5시간 14분</div></div>
            </div>
          </div>
        </div>

        <div id="phone-sub-contacts" class="phone-sub-view">
          <div style="padding: 50px 16px 12px 16px; display: flex; justify-content: space-between; align-items: center; background: rgba(20, 20, 22, 0.95); border-bottom: 0.5px solid rgba(255,255,255,0.1); position: relative;">
            <span class="back-btn" onclick="switchPhoneTab('recents')">‹ 통화</span>
            <span style="font-size: 16px; font-weight: 600; position: absolute; left: 50%; transform: translateX(-50%);">연락처</span>
            <span style="font-size: 20px; color: #0a84ff; cursor: pointer; z-index: 2;">+</span>
          </div>
          <div style="flex: 1; overflow-y: auto; padding-bottom: 70px; position: relative;">
            <div style="display: flex; align-items: center; padding: 12px 16px; gap: 12px; border-bottom: 0.5px solid #222;">
              <div style="width: 44px; height: 44px; border-radius: 50%; overflow: hidden; background: #555;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="이채영" style="width:100%; height:100%; object-fit:cover;"></div>
              <div><div style="font-size: 16px; font-weight: 600; color: #fff;">이채영</div><div style="font-size: 12px; color: #8e8e93; margin-top: 2px;">내 카드</div></div>
            </div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㄱ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">강민영</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">고다은</span></div><span style="color: #ff3b30; font-size: 20px; font-weight: bold;">*</span></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">고모</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">고모부</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">그녀의 여식1(언니1)</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">그녀의 여식2(언니2)</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">김현석 파트장님</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㄴ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">나연정</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">노윤경</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㄷ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">도강민</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㄹ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">류영빈</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅁ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">마더</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">문태빈</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅂ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">박수아</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅅ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">손은지</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">신현아</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅇ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">유세빈</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">유지애 위원님</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅈ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">장진영</span></div></div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">지헌이❤️</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅊ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">채원진</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅋ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">카스테라 가게</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅌ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">태은석</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅍ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">파더</span></div></div>
            <div style="padding: 6px 16px; font-size: 14px; font-weight: 600; color: #8e8e93; background: #000;">ㅎ</div>
            <div style="display: flex; align-items: center; justify-content: space-between; padding: 10px 16px; border-bottom: 0.5px solid #1c1c1e;"><div style="display: flex; align-items: center; gap: 12px;"><div style="width: 40px; height: 40px; border-radius: 50%; overflow: hidden; background: #2c2c2e;"><img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" style="width:100%; height:100%; object-fit:cover;"></div><span style="font-size: 15px; font-weight: 500; color: #fff;">한현영</span></div></div>
          </div>
          <div style="position: absolute; right: 4px; top: 80px; bottom: 70px; display: flex; flex-direction: column; align-items: center; justify-content: space-between; font-size: 9px; color: #0a84ff; padding: 4px 0; z-index: 10; pointer-events: none;">
            <span>ㄱ</span><span>ㄴ</span><span>ㄷ</span><span>ㄹ</span><span>ㅁ</span><span>ㅂ</span><span>ㅅ</span><span>ㅇ</span><span>ㅈ</span><span>ㅊ</span><span>ㅋ</span><span>ㅌ</span><span>ㅍ</span><span>ㅎ</span><span>A</span><span>B</span><span>C</span><span>D</span><span>E</span><span>F</span><span>G</span><span>H</span><span>I</span><span>J</span><span>K</span><span>L</span><span>M</span><span>N</span><span>O</span><span>P</span><span>Q</span><span>R</span><span>S</span><span>T</span><span>U</span><span>V</span><span>W</span><span>X</span><span>Y</span><span>Z</span><span>#</span>
          </div>
        </div>

        <div id="phone-sub-keypad" class="phone-sub-view" style="justify-content: center; align-items: center; padding-bottom: 50px;">
          <div class="keypad" style="padding-bottom: 20px;">
            <div class="key" style="background:#1c1c1e; color:#fff;">1<span class="sub">&nbsp;</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">2<span class="sub" style="color:#8e8e93;">ABC</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">3<span class="sub" style="color:#8e8e93;">DEF</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">4<span class="sub" style="color:#8e8e93;">GHI</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">5<span class="sub" style="color:#8e8e93;">JKL</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">6<span class="sub" style="color:#8e8e93;">MNO</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">7<span class="sub" style="color:#8e8e93;">PQRS</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">8<span class="sub" style="color:#8e8e93;">TUV</span></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">9<span class="sub" style="color:#8e8e93;">WXYZ</span></div>
            <div class="key empty"></div>
            <div class="key" style="background:#1c1c1e; color:#fff;">0<span class="sub" style="color:#8e8e93;">+</span></div>
            <div class="key empty" style="font-size:22px; display:flex; justify-content:center; align-items:center; color:#fff;">⌫</div>
          </div>
        </div>

        <div class="phone-tabbar">
          <div class="phone-tab-btn active" id="tab-btn-recents" onclick="switchPhoneTab('recents')"><span>통화</span></div>
          <div class="phone-tab-btn" id="tab-btn-contacts" onclick="switchPhoneTab('contacts')"><span>연락처</span></div>
          <div class="phone-tab-btn" id="tab-btn-keypad" onclick="switchPhoneTab('keypad')"><span>키패드</span></div>
        </div>
      </div>

      <!-- 💌 MESSAGES LIST VIEW -->
      <div id="messages-view" class="view">
        <div class="msg-list-header">
          <div class="msg-edit-btn">편집</div>
          <div class="msg-main-title">메시지</div>
          <div class="msg-menu-btn">≡</div>
        </div>

        <div class="msg-conversation-list">
          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="지헌이❤️">
            </div>
            <div class="msg-conv-info" onclick="openMessagesChat()">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender" style="color:#fff;">지헌이❤️</span>
                <span class="msg-conv-time">12:45</span>
              </div>
              <div class="msg-conv-preview">퇴근하고 바로가께🩵🩵</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="김현석 파트장님">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">김현석 파트장님</span>
                <span class="msg-conv-time">11:20</span>
              </div>
              <div class="msg-conv-preview">확인.</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="유지애 위원님">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">유지애 위원님</span>
                <span class="msg-conv-time">11:18</span>
              </div>
              <div class="msg-conv-preview">네 알겠습니다*^^*</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="강민영">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">강민영</span>
                <span class="msg-conv-time">어제</span>
              </div>
              <div class="msg-conv-preview">뭐 먹을겨? 애들한테 물어보니까...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="고다은">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">고다은</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">니네 언니가 톡좀 읽으라더라; 좀 읽...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="언니1">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">그녀의 여식1(언니1)</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">전화 장식이냐</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="인증번호">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1644-4174</span>
                <span class="msg-conv-time">목요일</span>
              </div>
              <div class="msg-conv-preview">[Web발신] [인증번호: 000514] 카카오계정 로그인...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="쿠팡">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1577-7011</span>
                <span class="msg-conv-time">수요일</span>
              </div>
              <div class="msg-conv-preview">[Web발신] [쿠팡] 주문하신 상품이 배송 완료되었습니다.</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>
        </div>

        <div class="msg-bottom-search-bar">
          <div class="msg-search-input-box">
            <span></span>
            <input type="text" placeholder="검색">
          </div>
          <span class="msg-compose-btn"></span>
        </div>
      </div>

      <!-- 💬 MESSAGES CHAT ROOM VIEW -->
      <div id="messages-chat-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="returnToMessagesList()">‹ 메시지</span>
          <div class="msg-header-profile">
            <div class="default-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="지헌이❤️">
            </div>
            <span style="font-size:12px;">지헌이❤️</span>
          </div>
          <span style="width:35px;"></span>
        </div>

        <div class="chat-container" id="chat-container">
          <div class="msg-date-divider">10월 18일 일요일</div>
          <div class="msg-wrapper msg-them">
            <div class="message-bubble">언니 오늘 모해ㅐ</div>
            <div class="msg-time">11:12</div>
          </div>
          <div class="msg-wrapper msg-me">
            <div class="message-bubble">오늘 약속이써ㅠ... 흑..</div>
            <div class="msg-time">11:14</div>
          </div>
          <div class="msg-wrapper msg-them">
            <div class="message-bubble">아 그래? 웅 조심히 다녀와요💗💗</div>
            <div class="msg-time">11:15</div>
          </div>
          <div class="msg-wrapper msg-me">
            <div class="message-bubble">알게써요 ㅎㅎㅎ</div>
            <div class="msg-time">11:18</div>
          </div>
          <div class="msg-wrapper msg-them">
            <div class="message-bubble">아직 밖이야? 카톡 안 보길래..</div>
            <div class="msg-time">23:40</div>
          </div>
          <div class="msg-wrapper msg-me">
            <div class="message-bubble">헐 ㅠ 진동해놨었나봐... 먄ㅜㅜㅜ</div>
            <div class="msg-time">23:49</div>
          </div>
          <div class="msg-wrapper msg-them">
            <div class="message-bubble">... 페이스톡이나 받아....</div>
            <div class="msg-time">23:52</div>
          </div>

          <div class="msg-date-divider">10월 19일 월요일</div>
          <div class="msg-wrapper msg-them">
            <div class="message-bubble">톡에 보내놓은 카페에서 기다리께용💗</div>
            <div class="msg-time">12:43</div>
          </div>
          <div class="msg-wrapper msg-me">
            <div class="message-bubble">퇴근하고 바로가께🩵🩵</div>
            <div class="msg-time">12:45</div>
          </div>
        </div>

        <div class="chat-input-area">
          <input type="text" id="chat-input" class="chat-input" placeholder="iMessage">
          <button class="send-btn" onclick="sendMessage()">↑</button>
        </div>
      </div>

      <!-- 4. NOTES VIEW -->
      <div id="notes-view" class="view">
        <div class="app-header center-title-header">
          <span class="back-btn" onclick="goHome()">‹ 폴더</span>
          <span class="app-title">메모</span>
          <span style="width: 35px;"></span>
        </div>
        <div class="notes-list">
          <div class="note-card" onclick="openApp('note-detail-view')">
            <div class="note-title">지헌이 줄 꽃 사기</div>
            <div class="note-preview">07:50 (인스타 팔로 해뒀던 꽃집에서 사...</div>
          </div>
        </div>
      </div>

      <!-- 5. NOTE DETAIL VIEW -->
      <div id="note-detail-view" class="view">
        <div class="app-header">
          <span class="back-btn" onclick="openApp('notes-view')">‹ 메모</span>
          <span class="app-title"></span>
          <span></span>
        </div>
        <div class="note-content">
          <h2 style="font-size: 20px; font-weight: 700; margin-bottom: 12px;">지헌이 줄 꽃 사기</h2>
          <p style="color: #ccc; font-size: 15px;">(인스타 팔로 해뒀던 꽃집에서 사기!!!!)</p>
        </div>
      </div>

    </div>

    <!-- Home Indicator Bar -->
    <div class="home-bar" onclick="goHome()"></div>
  </div>

  <script>
    // 📞 통화 화면 배경 동영상 URL
    const audioCallVideoUrl = 'https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2018-18-52_1.mov/ik-video.mp4?updatedAt=1792575791181';

    // 💬 1. 카카오톡 페이스톡 동영상 URL
    const myFaceTalkVideoUrl = 'https://ik.imagekit.io/ewslo2qz1/ScreenRecording_09-23-2026%2022-29-45_1.mov';

    // 🍏 2. 애플 페이스타임 동영상 URL
    const myFaceTimeVideoUrl = 'https://ik.imagekit.io/ewslo2qz1/ScreenRecording_10-21-2026%2020-57-51_1.mov';

    let activeProfileElement = null;
    let hasViewedStory = false;
    let previousInstaView = 'insta-feed-view';

    function triggerImageUpload(event, element) {
      event.stopPropagation();
      activeProfileElement = element;
      document.getElementById('imageUploadInput').click();
    }

    function handleImageUpload(event) {
      const file = event.target.files[0];
      if (file && activeProfileElement) {
        const reader = new FileReader();
        reader.onload = function(e) {
          const imageUrl = e.target.result;
          let imgTag = activeProfileElement.querySelector('img');
          if (imgTag) {
            imgTag.src = imageUrl;
          } else {
            activeProfileElement.innerHTML = `<img src="${imageUrl}" alt="프로필">`;
          }
        };
        reader.readAsDataURL(file);
      }
      event.target.value = '';
    }

    // 💬 카카오톡 페이스톡 열기
    function openFaceTalkVideo() {
      const videoPlayer = document.getElementById('facetalkVideoPlayer');
      const pipVideoPlayer = document.getElementById('myPipVideoPlayer');
      
      openApp('facetalk-view');

      videoPlayer.muted = false;
      if (pipVideoPlayer) {
        pipVideoPlayer.muted = true;
        pipVideoPlayer.play().catch(e => console.log("PIP video play error:", e));
      }

      const playPromise = videoPlayer.play();
      if (playPromise !== undefined) {
        playPromise.then(() => {
          videoPlayer.muted = false;
        }).catch(error => {
          const playOnTouch = () => {
            videoPlayer.muted = false;
            videoPlayer.play();
            if (pipVideoPlayer) pipVideoPlayer.play();
            document.removeEventListener('click', playOnTouch);
            document.removeEventListener('touchstart', playOnTouch);
          };
          document.addEventListener('click', playOnTouch, { once: true });
          document.addEventListener('touchstart', playOnTouch, { once: true });
        });
      }
    }

    function closeFaceTalkVideo() {
      const videoPlayer = document.getElementById('facetalkVideoPlayer');
      const pipVideoPlayer = document.getElementById('myPipVideoPlayer');
      if (videoPlayer) {
        videoPlayer.pause();
        videoPlayer.currentTime = 0;
      }
      if (pipVideoPlayer) {
        pipVideoPlayer.pause();
        pipVideoPlayer.currentTime = 0;
      }
      openApp('kakao-chat-room');
    }

    /* FaceTime 다이내믹 아일랜드 알림 수신/거절 제어 함수 */
    function triggerFaceTimeBanner() {
      const island = document.getElementById('dynamicIsland');
      if (island) {
        island.classList.add('facetime-incoming');
      }
    }

    function declineFaceTimeCall(e) {
      if (e) e.stopPropagation();
      const island = document.getElementById('dynamicIsland');
      if (island) {
        island.classList.remove('facetime-incoming');
      }
    }

    // 🍏 다이내믹 아일랜드 페이스타임 수락
    function acceptFaceTimeCall(e) {
      if (e) e.stopPropagation();
      declineFaceTimeCall();
      
      const videoPlayer = document.getElementById('facetimeVideoPlayer');
      const pipVideoPlayer = document.getElementById('myFacetimePipVideoPlayer');
      
      openApp('facetime-view');

      videoPlayer.muted = false;
      if (pipVideoPlayer) {
        pipVideoPlayer.muted = true;
        pipVideoPlayer.play().catch(e => console.log("FaceTime PIP video play error:", e));
      }

      videoPlayer.play().catch(e => console.log("FaceTime video play error:", e));
    }

    function closeFaceTimeVideo() {
      const videoPlayer = document.getElementById('facetimeVideoPlayer');
      const pipVideoPlayer = document.getElementById('myFacetimePipVideoPlayer');
      if (videoPlayer) {
        videoPlayer.pause();
        videoPlayer.currentTime = 0;
      }
      if (pipVideoPlayer) {
        pipVideoPlayer.pause();
        pipVideoPlayer.currentTime = 0;
      }
      openApp('home-view');
    }

    /* 📞 음성 통화 연결 화면 및 타이머 제어 */
    let callTimer = null;
    let callSeconds = 0;

    function openAudioCall() {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      const audioCallView = document.getElementById('audio-call-view');
      audioCallView.classList.add('active');
      document.getElementById('status-bar-time').style.visibility = 'hidden';

      const bgVideo = document.getElementById('audioCallBgVideo');
      const bgSource = document.getElementById('audioCallBgSource');
      if (bgVideo) {
        if (bgSource && bgSource.src !== audioCallVideoUrl) {
          bgSource.src = audioCallVideoUrl;
          bgVideo.load();
        }
        bgVideo.currentTime = 0;
        bgVideo.muted = false;
        bgVideo.play().catch(e => {
          console.log("Audio call bg video play error:", e);
        });

        bgVideo.onended = function() {
          closeAudioCall();
        };
      }

      const statusText = audioCallView.querySelector('.audio-call-status');
      if (statusText) {
        statusText.innerText = "iPhone 연결 중...";
      }

      if (callTimer) {
        clearInterval(callTimer);
        callTimer = null;
      }
      callSeconds = 0;

      setTimeout(() => {
        if (audioCallView.classList.contains('active')) {
          if (statusText) {
            statusText.innerText = "00:01";
          }
          callSeconds = 1;

          callTimer = setInterval(() => {
            callSeconds++;
            const mins = String(Math.floor(callSeconds / 60)).padStart(2, '0');
            const secs = String(callSeconds % 60).padStart(2, '0');
            if (statusText) {
              statusText.innerText = `${mins}:${secs}`;
            }
          }, 1000);
        }
      }, 1000);
    }

    function closeAudioCall() {
      if (callTimer) {
        clearInterval(callTimer);
        callTimer = null;
      }
      callSeconds = 0;

      const bgVideo = document.getElementById('audioCallBgVideo');
      if (bgVideo) {
        bgVideo.pause();
        bgVideo.currentTime = 0;
        bgVideo.onended = null;
      }

      document.getElementById('audio-call-view').classList.remove('active');
      openApp('phone-view');
    }

    /* 📸 인스타그램 제어 함수들 */
    function openInstaFeed() {
      hideInstaNotification();
      updateStoryRings();
      openApp('insta-feed-view');
    }

    function openInstaProfile() {
      hideInstaNotification();
      updateStoryRings();
      openApp('insta-profile-view');
    }

    function openMyInstaProfile() {
      hideInstaNotification();
      openApp('insta-my-profile-view');
    }

    function openInstaStory() {
      hideInstaNotification();
      hasViewedStory = true;
      updateStoryRings();

      const currentActiveView = document.querySelector('.view.active');
      if (currentActiveView && (currentActiveView.id === 'insta-profile-view' || currentActiveView.id === 'insta-feed-view' || currentActiveView.id === 'insta-my-profile-view')) {
        previousInstaView = currentActiveView.id;
      } else {
        previousInstaView = 'insta-feed-view';
      }

      openApp('insta-story-view');
    }

    let hasTriggeredKakaoNotification = false;

    function closeInstaStory() {
      openApp(previousInstaView);
      
      if (!hasTriggeredKakaoNotification) {
        hasTriggeredKakaoNotification = true;
        setTimeout(() => {
          triggerKakaoNotification();
        }, 800);
      }
    }

    function triggerKakaoNotification() {
      const banner = document.getElementById('kakao-banner');
      if (banner) {
        banner.classList.add('show');
        setTimeout(() => {
          banner.classList.remove('show');
        }, 4000);
      }
    }

    function hideKakaoNotification() {
      const banner = document.getElementById('kakao-banner');
      if (banner) banner.classList.remove('show');
    }

    function updateStoryRings() {
      const feedRing = document.getElementById('feed-story-ring');
      const profileRing = document.getElementById('profile-avatar-ring');
      const postAvatarRing = document.getElementById('post-avatar-ring');

      if (hasViewedStory) {
        if (feedRing) feedRing.classList.add('viewed');
        if (profileRing) profileRing.classList.add('viewed');
        if (postAvatarRing) postAvatarRing.classList.add('viewed');
      } else {
        if (feedRing) feedRing.classList.remove('viewed');
        if (profileRing) profileRing.classList.remove('viewed');
        if (postAvatarRing) postAvatarRing.classList.remove('viewed');
      }
    }

    function adjustScale() {
      const mockup = document.getElementById('phoneMockup');
      const targetWidth = 380;
      const targetHeight = 780;
      const padding = 20;

      const scaleX = (window.innerWidth - padding) / targetWidth;
      const scaleY = (window.innerHeight - padding) / targetHeight;
      
      let scale = Math.min(scaleX, scaleY);
      if (scale > 1) scale = 1;

      mockup.style.transform = `scale(${scale})`;
    }

    window.addEventListener('resize', adjustScale);
    window.addEventListener('load', adjustScale);

    const myPhotoList = [
      'https://i.postimg.cc/Bnc0JTS5/IMG-1448.jpg',
      'https://i.postimg.cc/2SFDCQk5/89747A02-0A10-4577-AC9F-BA4CDCA71181.jpg',
      'https://i.postimg.cc/MpbSz76B/IMG-5070.jpg',
      'https://i.postimg.cc/vmJk9DZZ/IMG-2896.jpg',
      'https://i.postimg.cc/C1bt4YRg/IMG-8097.jpg',
      'https://i.postimg.cc/pL9F7dPP/IMG-7430.png',
      'https://i.postimg.cc/3xPvHHwY/IMG-7428.png',
      'https://i.postimg.cc/MphQ88Gw/IMG-7431.png',
      'https://i.postimg.cc/QdRTrrM7/IMG-7427.png',
      'https://i.postimg.cc/8CMrpnFv/IMG-7426.png',
      'https://i.postimg.cc/q7yCJZt8/IMG-7425.png',
      'https://i.postimg.cc/RVptXt56/IMG-7432.png',
    ];

    function renderPhotos() {
      const grid = document.getElementById('photos-grid');
      if (!grid) return;
      grid.innerHTML = '';
      myPhotoList.forEach(url => {
        const item = document.createElement('div');
        item.className = 'photo-thumb';
        item.innerHTML = `<img src="${url}" alt="보관함 사진">`;
        grid.appendChild(item);
      });
    }

    renderPhotos();

    let enteredPasscode = "";
    const correctPasscode = "0417";
    let hasShownInstaNotification = false;

    function openApp(viewId) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById('audio-call-view').classList.remove('active');
      
      const bgVideo = document.getElementById('audioCallBgVideo');
      if (bgVideo) {
        bgVideo.pause();
        bgVideo.currentTime = 0;
      }

      document.getElementById(viewId).classList.add('active');

      const statusBarTime = document.getElementById('status-bar-time');
      if (viewId === 'lock-view' || viewId === 'passcode-view' || viewId === 'facetalk-view' || viewId === 'facetime-view' || viewId === 'insta-story-view') {
        statusBarTime.style.visibility = 'hidden';
      } else {
        statusBarTime.style.visibility = 'visible';
      }

      if (viewId === 'home-view') {
        triggerFirstInstaNotification();
      } else {
        hideInstaNotification();
      }

      if (viewId === 'phone-view') {
        switchPhoneTab('recents');
      }

      if (viewId !== 'facetalk-view') {
        const videoPlayer = document.getElementById('facetalkVideoPlayer');
        const pipVideoPlayer = document.getElementById('myPipVideoPlayer');
        if (videoPlayer) videoPlayer.pause();
        if (pipVideoPlayer) pipVideoPlayer.pause();
      }

      if (viewId !== 'facetime-view') {
        const ftVideoPlayer = document.getElementById('facetimeVideoPlayer');
        const ftPipPlayer = document.getElementById('myFacetimePipVideoPlayer');
        if (ftVideoPlayer) ftVideoPlayer.pause();
        if (ftPipPlayer) ftPipPlayer.pause();
      }
    }

    function triggerFirstInstaNotification() {
      if (hasShownInstaNotification) return;

      const banner = document.getElementById('insta-banner');
      banner.classList.remove('show');

      setTimeout(() => {
        const homeView = document.getElementById('home-view');
        if (homeView && homeView.classList.contains('active') && !hasShownInstaNotification) {
          hasShownInstaNotification = true;
          banner.classList.add('show');

          setTimeout(() => {
            banner.classList.remove('show');
          }, 4000);
        }
      }, 300);
    }

    function hideInstaNotification() {
      const banner = document.getElementById('insta-banner');
      if (banner) banner.classList.remove('show');
    }

    function switchPhoneTab(tabName) {
      document.querySelectorAll('.phone-sub-view').forEach(v => v.classList.remove('active'));
      document.querySelectorAll('.phone-tab-btn').forEach(b => b.classList.remove('active'));

      if (tabName === 'recents') {
        document.getElementById('phone-sub-recents').classList.add('active');
        document.getElementById('tab-btn-recents').classList.add('active');
      } else if (tabName === 'contacts') {
        document.getElementById('phone-sub-contacts').classList.add('active');
        document.getElementById('tab-btn-contacts').classList.add('active');
      } else if (tabName === 'keypad') {
        document.getElementById('phone-sub-keypad').classList.add('active');
        document.getElementById('tab-btn-keypad').classList.add('active');
      }
    }

    function goHome() {
      openApp('home-view');
    }

    function showPasscode() {
      openApp('passcode-view');
    }

    function hidePasscode() {
      enteredPasscode = "";
      updateDots();
      openApp('lock-view');
    }

    function pressKey(num) {
      if (enteredPasscode.length < 4) {
        enteredPasscode += num;
        updateDots();
        if (enteredPasscode.length === 4) {
          setTimeout(() => {
            if (enteredPasscode === correctPasscode) {
              enteredPasscode = "";
              updateDots();
              goHome();
            } else {
              alert('틀렸수다~ㅡㅡ; (힌트: 바둑이 생일)');
              enteredPasscode = "";
              updateDots();
            }
          }, 150);
        }
      }
    }

    function deleteKey() {
      if (enteredPasscode.length > 0) {
        enteredPasscode = enteredPasscode.slice(0, -1);
        updateDots();
      }
    }

    function updateDots() {
      for (let i = 0; i < 4; i++) {
        const dot = document.getElementById(`dot-${i}`);
        if (i < enteredPasscode.length) {
          dot.style.background = "#fff";
        } else {
          dot.style.background = "transparent";
        }
      }
    }

    let isJiheonRead = false;
    let hasSentFlowerMsg = false;

    function openKakaoChatRoom() {
      isJiheonRead = true;
      const jiheonBadge = document.getElementById('jiheon-badge-count');
      const totalBadge = document.getElementById('total-unread-badge');
      if (jiheonBadge) {
        jiheonBadge.style.display = 'none';
      }
      if (totalBadge) {
        totalBadge.innerText = '2';
      }
      openApp('kakao-chat-room');

      if (!hasSentFlowerMsg) {
        hasSentFlowerMsg = true;
        setTimeout(() => {
          const chatContainer = document.getElementById('kakao-chat-container');
          
          const replyWrapper = document.createElement('div');
          replyWrapper.className = 'kc-msg-row them';

          const replyAvatar = document.createElement('div');
          replyAvatar.className = 'kc-avatar';
          replyAvatar.innerHTML = `<img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">`;

          const replyBubble = document.createElement('div');
          replyBubble.className = 'kc-bubble';
          replyBubble.innerText = "꽃도 너무 마음에 들어ㅓ ㅎㅎㅎ";

          const replyTime = document.createElement('div');
          replyTime.className = 'kc-time';
          replyTime.innerText = "23:48";

          replyWrapper.appendChild(replyAvatar);
          replyWrapper.appendChild(replyBubble);
          replyWrapper.appendChild(replyTime);
          chatContainer.appendChild(replyWrapper);

          chatContainer.scrollTop = chatContainer.scrollHeight;

          const listPreview = document.getElementById('jiheon-list-preview');
          const listTime = document.getElementById('jiheon-list-time');
          if (listPreview) listPreview.innerText = "꽃도 너무 마음에 들어ㅓ ㅎㅎㅎ";
          if (listTime) listTime.innerText = "23:48";
        }, 1200);
      }
    }

    function returnToKakaoList() {
      openApp('kakao-talk-view');
    }

    function openMessagesChat() {
      openApp('messages-chat-view');
    }

    function returnToMessagesList() {
      openApp('messages-view');
    }

    let isRecordingVoice = false;
    function toggleVoiceRecording() {
      const btnInner = document.getElementById('vmRedBtnInner');
      isRecordingVoice = !isRecordingVoice;
      if (isRecordingVoice) {
        btnInner.style.borderRadius = "4px";
        btnInner.style.width = "20px";
        btnInner.style.height = "20px";
        btnInner.style.background = "#fff";
      } else {
        btnInner.style.borderRadius = "50%";
        btnInner.style.width = "24px";
        btnInner.style.height = "24px";
        btnInner.style.background = "#ff3b30";
      }
    }

    function sendKakaoMessage() {
      const input = document.getElementById('kakao-chat-input');
      const text = input.value.trim();
      if (!text) return;

      const chatContainer = document.getElementById('kakao-chat-container');
      const currentTime = "23:48";

      const myWrapper = document.createElement('div');
      myWrapper.className = 'kc-msg-row me';

      const myBubble = document.createElement('div');
      myBubble.className = 'kc-bubble';
      myBubble.innerText = text;

      const myTime = document.createElement('div');
      myTime.className = 'kc-time';
      myTime.innerText = currentTime;

      myWrapper.appendChild(myBubble);
      myWrapper.appendChild(myTime);
      chatContainer.appendChild(myWrapper);

      input.value = '';
      chatContainer.scrollTop = chatContainer.scrollHeight;

      if (hasSentFlowerMsg) {
        setTimeout(() => {
          triggerFaceTimeBanner();
        }, 500);
      } else {
        setTimeout(() => {
          let replyText = "ㅎㅎㅎ";

          if (text.includes('사랑해') || text.includes('사랑햐') || text.includes('사룽해') || text.includes('사랑')) {
            replyText = "나도!! 사랑해ㅐㅐ ❤️❤️❤️";
          } else if (text.includes('보고싶') || text.includes('보구싶')) {
            replyText = "나도 너무 보고싶어ㅓ..";
          } else if (text.includes('뭐해') || text.includes('모해')) {
            replyText = "나 언니 생각하고 있어찌이ㅣㅣ";
          } else if (text.includes('안녕') || text.includes('하이')) {
            replyText = "갑자기?ㅋㅋ";
          }

          const replyWrapper = document.createElement('div');
          replyWrapper.className = 'kc-msg-row them';

          const replyAvatar = document.createElement('div');
          replyAvatar.className = 'kc-avatar';
          replyAvatar.innerHTML = `<img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">`;

          const replyBubble = document.createElement('div');
          replyBubble.className = 'kc-bubble';
          replyBubble.innerText = replyText;

          const replyTime = document.createElement('div');
          replyTime.className = 'kc-time';
          replyTime.innerText = "23:48";

          replyWrapper.appendChild(replyAvatar);
          replyWrapper.appendChild(replyBubble);
          replyWrapper.appendChild(replyTime);
          chatContainer.appendChild(replyWrapper);

          chatContainer.scrollTop = chatContainer.scrollHeight;
        }, 1000);
      }
    }

    function sendMessage() {
      const input = document.getElementById('chat-input');
      const text = input.value.trim();
      if (!text) return;

      const chatContainer = document.getElementById('chat-container');
      const currentTime = "23:48";

      const myWrapper = document.createElement('div');
      myWrapper.className = 'msg-wrapper msg-me';

      const myBubble = document.createElement('div');
      myBubble.className = 'message-bubble';
      myBubble.innerText = text;

      const myTime = document.createElement('div');
      myTime.className = 'msg-time';
      myTime.innerText = currentTime;

      myWrapper.appendChild(myBubble);
      myWrapper.appendChild(myTime);
      chatContainer.appendChild(myWrapper);

      input.value = '';
      chatContainer.scrollTop = chatContainer.scrollHeight;

      setTimeout(() => {
        let replyText = "힛";

        if (text.includes('사랑해') || text.includes('사랑') || text.includes('사룽')) {
          replyText = "나두 💗💗💗 히히";
        } 
        else if (text.includes('보고싶') || text.includes('보구싶') || text.includes('보고시퍼')) {
          replyText = "나도...";
        } 
        else if (text.includes('뭐해') || text.includes('모해') || text.includes('머해')) {
          replyText = "언니 생각하지요오 ㅎㅎ";
        } 
        else if (text.includes('들어갔') || text.includes('조심') || text.includes('들어갓')) {
          replyText = "언니랑 더 있고 싶었는데..";
        } 
        else if (text.includes('미안') || text.includes('먄') || text.includes('미아')) {
          replyText = "ㅡㅡ";
        } 
        else if (text.includes('헌') || text.includes('지헌')) {
          replyText = "웅";
        } 
        else if (text.includes('ㅋㅋ') || text.includes('ㅎㅎ') || text.includes('ㅋㅋㅋ')) {
          replyText = "ㅋㅋㅋㅋㅋ";
        } 
        else if (text.includes('잘자') || text.includes('꿈꿔') || text.includes('굿나잇')) {
          replyText = "쪽❤️❤️";
        }

        const replyWrapper = document.createElement('div');
        replyWrapper.className = 'msg-wrapper msg-them';

        const replyBubble = document.createElement('div');
        replyBubble.className = 'message-bubble';
        replyBubble.innerText = replyText;

        const replyTime = document.createElement('div');
        replyTime.className = 'msg-time';
        replyTime.innerText = "23:48";

        replyWrapper.appendChild(replyBubble);
        replyWrapper.appendChild(replyTime);
        chatContainer.appendChild(replyWrapper);

        chatContainer.scrollTop = chatContainer.scrollHeight;
      }, 1000);
    }
  </script>
</body>
</html>

이게 최종본이야
