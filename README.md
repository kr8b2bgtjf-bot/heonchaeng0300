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

    .photos-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 6px;
    }

    .photo-thumb {
      width: 100%;
      aspect-ratio: 0.8;
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
      cursor: pointer;
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

    #facetalk-view {
      background: #000;
      position: relative;
    }

    .facetalk-video-container {
      width: 100%;
      height: 100%;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #000;
    }

    .facetalk-video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .facetalk-overlay-header {
      position: absolute;
      top: 50px;
      left: 16px;
      right: 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 10;
      color: #fff;
    }

    .facetalk-back-btn {
      font-size: 24px;
      cursor: pointer;
      color: #fff;
      background: rgba(0,0,0,0.4);
      width: 36px;
      height: 36px;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
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

    /* Phone App View */
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

    /* iMessage 날짜 구분선 스타일 */
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
    <!-- Dynamic Island -->
    <div class="dynamic-island"></div>

    <!-- Status Bar -->
    <div class="status-bar">
      <span class="status-left" id="status-bar-time" style="visibility: hidden;">23:48</span>
      <div class="status-right"></div>
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

      <!-- 🔑 PASSCODE VIEW -->
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

      <!-- 💬 카카오톡 채팅 목록 화면 -->
      <div id="kakao-talk-view" class="view">
        <div class="k-header">
          <div class="k-title">채팅</div>
          <div class="k-icons">
            <span>🔍</span>
            <span>💬</span>
            <span>⚙️</span>
          </div>
        </div>

        <div class="k-sub-filters">
          <div class="k-pill active">전체</div>
          <div class="k-pill">안읽음 <span class="k-pill-badge" id="total-unread-badge">2</span></div>
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
          <div class="k-chat-item">
            <div class="k-profile-img" onclick="triggerImageUpload(event, this)">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌이">
            </div>
            <div class="k-chat-info" onclick="openKakaoChatRoom()">
              <div class="k-chat-name-row">
                <span class="k-chat-name">지헌이❤️</span>
                <span class="k-chat-time">22:06</span>
              </div>
              <div class="k-chat-preview">흥</div>
            </div>
          </div>

          <div class="k-chat-item">
            <div class="k-profile-img" onclick="triggerImageUpload(event, this)">
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
            <div class="k-profile-img" onclick="triggerImageUpload(event, this)">
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
            <div class="k-profile-img" onclick="triggerImageUpload(event, this)">
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
          <div class="k-tab-item" onclick="goHome()">👤</div>
          <div class="k-tab-item active">💬<span class="k-tab-badge" id="tabbar-badge">2</span></div>
          <div class="k-tab-item">💬</div>
          <div class="k-tab-item">🛍️</div>
          <div class="k-tab-item">⋯</div>
        </div>
      </div>

      <!-- 💌 ‘지헌이❤️’ 카카오톡 대화 상세 방 -->
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
            <div class="kc-avatar" onclick="triggerImageUpload(event, this)">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">톡 왜 안봐?</div>
            <div class="kc-time">23:26</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar" onclick="triggerImageUpload(event, this)">
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
              <div class="facetalk-icon">📹</div>
              <span>페이스톡 종료</span>
            </div>
            <div class="facetalk-duration">1:12:11</div>
            <div style="font-size:10px; color:#fff; text-align:right; margin-top:2px;">01:43</div>
          </div>

          <div class="kc-msg-row them">
            <div class="kc-avatar" onclick="triggerImageUpload(event, this)">
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
            <div class="kc-avatar" onclick="triggerImageUpload(event, this)">
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
            <div class="kc-avatar" onclick="triggerImageUpload(event, this)">
              <img src="https://i.postimg.cc/nzGhJnhQ/IMG-1498.jpg" alt="지헌">
            </div>
            <div class="kc-bubble">흥</div>
            <div class="kc-time">23:06</div>
          </div>
        </div>

        <div class="kc-input-area">
          <span class="kc-input-icon">➕</span>
          <input type="text" id="kakao-chat-input" class="kc-input-box" placeholder="메시지 입력">
          <span class="kc-input-icon">#</span>
          <button class="kc-send-btn" onclick="sendKakaoMessage()">↑</button>
        </div>
      </div>

      <!-- 📹 목업 내부 페이스톡 동영상 재생 화면 -->
      <div id="facetalk-view" class="view">
        <div class="facetalk-overlay-header">
          <span class="facetalk-back-btn" onclick="closeFaceTalkVideo()">‹</span>
          <span style="font-weight:600; font-size:15px; text-shadow:0 1px 3px rgba(0,0,0,0.8);"></span>
          <span style="width:36px;"></span>
        </div>
        <div class="facetalk-video-container">
          <video id="facetalkVideoPlayer" class="facetalk-video" autoplay loop playsinline webkit-playsinline muted controls preload="auto">
            <source id="facetalkVideoSource" src="https://ik.imagekit.io/ewslo2qz1/Untitled.mp4/ik-video.mp4?updatedAt=1792428006372" type="video/mp4">
          </video>
        </div>
      </div>

      <!-- 2. PHONE APP VIEW (통화 앱) -->
      <div id="phone-view" class="view">
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
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">
                <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필">
              </div>
              <div>
                <div class="call-name">지헌이❤️</div>
                <div class="call-type">↙ 휴대폰</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">12:38</div>
            </div>
          </div>
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">
                <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필">
              </div>
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
              <div class="call-avatar">
                <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필">
              </div>
              <div>
                <div class="call-name" style="color:#fff;">지헌이❤️</div>
                <div class="call-type">↙ 카카오톡 영상통화</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">토요일</div>
              <div class="call-duration">1시간 12분</div>
            </div>
          </div>
          <div class="call-item">
            <div class="call-info">
              <div class="call-avatar">
                <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="프로필">
              </div>
              <div>
                <div class="call-name" style="color:#fff;">지헌이❤️</div>
                <div class="call-type">↙ 휴대폰</div>
              </div>
            </div>
            <div class="call-meta">
              <div class="call-date">목요일</div>
              <div class="call-duration">5시간 14분</div>
            </div>
          </div>
        </div>
      </div>

      <!-- 💌 MESSAGES LIST VIEW (메시지 앱 목록 화면) -->
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
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="+82 1599-6405">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1644-4174</span>
                <span class="msg-conv-time">어제</span>
              </div>
              <div class="msg-conv-preview">[Web발신] [인증번호: 000514] 카카오계정 로그...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="+82 1588-1580">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1644-1107</span>
                <span class="msg-conv-time">어제</span>
              </div>
              <div class="msg-conv-preview">[Web발신] (광고)컬리 12,000원 쿠폰 당첨!...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="+82 2-525-2110">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1544-1900</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">[Web발신] (광고)[교보문고]리커버에디션 예약판매!...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="+82 1577-7011">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1577-7011</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">[Web발신] (광고)쿠팡_쿠폰 당첨 축하합니다!...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-unread-dot"></div>
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="114">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 10-2000-...</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">[한진택배] 네이버배송 상품 도착!</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>

          <div class="msg-conv-item">
            <div class="msg-conv-avatar">
              <img src="https://i.postimg.cc/k5CkQqfD/IMG-7307.jpg" alt="+82 1600-0987">
            </div>
            <div class="msg-conv-info">
              <div class="msg-conv-top-row">
                <span class="msg-conv-sender">+82 1600-0987</span>
                <span class="msg-conv-time">토요일</span>
              </div>
              <div class="msg-conv-preview">[Web발신] (광고)[배달의민족] 인기 브랜드...</div>
            </div>
            <span class="msg-conv-chevron">></span>
          </div>
        </div>

        <div class="msg-bottom-search-bar">
          <div class="msg-search-input-box">
            <span></span>
            <input type="text" placeholder="검색">
            <span></span>
          </div>
          <span class="msg-compose-btn"></span>
        </div>
      </div>

      <!-- 💬 MESSAGES CHAT ROOM VIEW (메시지 상세 대화방) -->
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
          <!-- 10월 18일 일요일 대화 (과거 내역) -->
          <div class="msg-date-divider">10월 18일 일요일</div>

          <div class="msg-wrapper msg-them">
            <div class="message-bubble">언니 오늘 모해ㅐ</div>
            <div class="msg-time">11:12</div>
          </div>

          <div class="msg-wrapper msg-me">
            <div class="message-bubble">오늘 약속이써ㅠ... 흑.. </div>
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

          <!-- 10월 19일 월요일 대화 (가장 최근 기점) -->
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
          <p style="color: #ccc; font-size: 15px;">
            (인스타 팔로 해뒀던 꽃집에서 사기!!!!)
          </p>
        </div>
      </div>

    </div>

    <!-- Home Indicator Bar -->
    <div class="home-bar" onclick="goHome()"></div>
  </div>

  <script>
    let activeProfileElement = null;

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

    const myFaceTalkVideoUrl = 'https://ik.imagekit.io/ewslo2qz1/Untitled.mp4/ik-video.mp4?updatedAt=1792428006372';

    function openFaceTalkVideo() {
      const videoPlayer = document.getElementById('facetalkVideoPlayer');
      const videoSource = document.getElementById('facetalkVideoSource');
      
      openApp('facetalk-view');

      if (videoSource.src !== myFaceTalkVideoUrl) {
        videoSource.src = myFaceTalkVideoUrl;
        videoPlayer.load();
      }
      
      videoPlayer.muted = true;
      const playPromise = videoPlayer.play();
      
      if (playPromise !== undefined) {
        playPromise.then(() => {
          setTimeout(() => { videoPlayer.muted = false; }, 300);
        }).catch(error => {
          console.log('자동 재생 제한 발생, 터치 대기:', error);
          const playOnTouch = () => {
            videoPlayer.muted = false;
            videoPlayer.play();
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
      if (videoPlayer) {
        videoPlayer.pause();
        videoPlayer.currentTime = 0;
      }
      openApp('kakao-chat-room');
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
      'https://i.postimg.cc/bN8W3JwD/IMG-2338.jpg',
      'https://i.postimg.cc/gc31VRmR/IMG-2489.jpg',
      'https://i.postimg.cc/3N6hDQ2b/IMG-3432.jpg',
      'https://i.postimg.cc/2y3m6SLK/IMG-3430.jpg',
      'https://i.postimg.cc/fbjMqFhx/IMG-4083.jpg',
      'https://i.postimg.cc/Vv516k0X/IMG-3452.jpg',
      'https://i.postimg.cc/nrMZzh93/IMG-3443.jpg',
      'https://i.postimg.cc/dVCsr3HS/IMG-4647.jpg',
      'https://i.postimg.cc/PxXTg4XG/IMG-3507.jpg',
      'https://i.postimg.cc/7LMqtcyn/IMG-3826.jpg',
      'https://i.postimg.cc/Qt8h2b8s/IMG-3508.jpg',
      'https://i.postimg.cc/6q84CwGt/IMG-3495.jpg',
      'https://i.postimg.cc/MHnf1xj6/IMG-3496.jpg',
      'https://i.postimg.cc/SRnYcqzK/IMG-3497.jpg',
      'https://i.postimg.cc/VvJ0X1b6/IMG-3498.jpg'
    ];

    function renderPhotos() {
      const grid = document.getElementById('photos-grid');
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

    function openApp(viewId) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById(viewId).classList.add('active');

      const statusBarTime = document.getElementById('status-bar-time');
      if (viewId === 'lock-view' || viewId === 'passcode-view' || viewId === 'facetalk-view') {
        statusBarTime.style.visibility = 'hidden';
      } else {
        statusBarTime.style.visibility = 'visible';
      }

      if (viewId !== 'facetalk-view') {
        const videoPlayer = document.getElementById('facetalkVideoPlayer');
        if (videoPlayer) videoPlayer.pause();
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

    function lockPhone() {
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

    function openKakaoChatRoom() {
      openApp('kakao-chat-room');
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
