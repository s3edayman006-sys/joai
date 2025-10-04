<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title data-lang-key="pageTitle">JO AI</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet"/>

  <!-- Performance preconnect/dns-prefetch -->
  <link rel="preconnect" href="https://generativelanguage.googleapis.com" crossorigin/>
  <link rel="dns-prefetch" href="https://generativelanguage.googleapis.com"/>
  <link rel="preconnect" href="https://api.openai.com" crossorigin/>
  <link rel="dns-prefetch" href="https://api.openai.com"/>

  <!-- Markdown + Sanitization -->
  <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dompurify@3.0.3/dist/purify.min.js"></script>

  <style>
    body { font-family: 'Cairo', sans-serif; background-color: #0c1521; }
    .container { background-color: #1f2937; border: 1px solid #374151; box-shadow: 0 4px 12px rgba(0,0,0,0.2); }
    #main-app-container.container { background-color: initial; }
    .glowing-shadow { box-shadow: 0 0 15px rgba(255,255,255,0.1), 0 0 30px rgba(74,222,128,0.2), 0 0 45px rgba(74,222,128,0.1); }
    .control-panel { background: linear-gradient(135deg, #1A2033 0%, #171E2D 100%); border: 1px solid #334155; }
    .chat-panel { background: linear-gradient(135deg, #121a29 0%, #0f1522 100%); border: 1px solid #334155; }
    .button-style {
      background: linear-gradient(145deg, #0f172a 0%, #0c1521 100%); color: white;
      transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); position: relative; overflow: hidden;
      border: 1px solid #4ade80; box-shadow: 0 0 10px #4ade8050;
    }
    .button-style:hover:not(:disabled) { background: linear-gradient(145deg, #1c2742 0%, #151d2e 100%); transform: translateY(-3px); box-shadow: 0 8px 15px rgba(0,0,0,0.2); }
    .button-style:active:not(:disabled) { transform: translateY(1px); }
    .button-style:disabled { cursor: not-allowed; background: #374151; border-color: #4b5563; opacity: 0.6; }
    .button-style::before {
      content: ''; position: absolute; top: 50%; left: 50%; width: 300%; height: 300%;
      background-color: rgba(255,255,255,0.15); transition: all 0.6s ease-in-out; border-radius: 50%;
      transform: translate(-50%, -50%) scale(0);
    }
    .button-style:hover:not(:disabled)::before { transform: translate(-50%, -50%) scale(1); }
    .glowing-shadow-green { box-shadow: 0 0 15px rgba(74, 222, 128, 0.2), 0 0 30px rgba(74, 222, 128, 0.1), 0 0 60px rgba(74, 222, 128, 0.05); }
    .glowing-shadow-green:hover { box-shadow: 0 0 20px rgba(74, 222, 128, 0.4), 0 0 40px rgba(74, 222, 128, 0.2), 0 0 60px rgba(74, 222, 128, 0.1); }
    .chat-messages { max-height: 420px; overflow-y: auto; padding: 0.75rem; scroll-behavior: smooth; }
    .chat-bubble { max-width: 85%; padding: 0.6rem 0.8rem; border-radius: 0.75rem; line-height: 1.4; font-size: 0.95rem; border: 1px solid #303b50; word-wrap: break-word; }
    .chat-bubble-user { background: #0b1220; color: #e5e7eb; margin-left: auto;}
    .chat-bubble-assistant { background: #122033; color: #d1d5db; margin-right: auto;}
    .chat-row { display: flex; margin: 0.4rem 0; flex-direction: column; }
    .chat-row.user { align-items: flex-end; }
    .chat-row.assistant { align-items: flex-start; }
    .chat-image-thumb { width: 72px; height: 72px; object-fit: cover; border-radius: 0.5rem; border: 1px solid #374151; cursor: pointer; }
    .chat-images-container { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 8px; }
    .status-bar { background: linear-gradient(135deg, #0f1626 0%, #0d1524 100%); border: 1px solid #334155; }
    .status-pill { border: 1px solid #334155; padding: 0.25rem 0.5rem; border-radius: 9999px; font-size: 0.75rem; }
    .loading-animation { border-top-color: #4ade80; animation: spin 1s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .ai-badge { border: 1px solid rgba(168,85,247,0.35); background: linear-gradient(135deg,#1b1430 0%,#141126 100%); box-shadow: 0 0 12px rgba(168,85,247,0.15); }
    .ai-badge-strong { border-color: rgba(168,85,247,0.6); box-shadow: 0 0 18px rgba(168,85,247,0.25); }
    .app-grid { display: grid; grid-template-columns: 1fr; gap: 1rem; }
    @media (min-width: 1024px) { .app-grid { grid-template-columns: 1fr 1.4fr; gap: 1.25rem; } }
    .image-preview-thumbnail { width: 72px; height: 72px; object-fit: cover; border-radius: 0.5rem; border: 1px solid #374151; }
    .remove-image-btn {
      position: absolute; top: -8px; right: -8px; width: 22px; height: 22px;
      border-radius: 9999px; background: #ef4444; color: #fff; border: 1px solid #7f1d1d;
      display: grid; place-items: center; font-weight: 700; line-height: 1; cursor: pointer; z-index: 10;
    }
    .remove-image-btn:hover { background: #dc2626; }
    #output-section { transition: background 0.5s ease-in-out, box-shadow 0.5s ease-in-out; }
    .bg-default { background: transparent; }
    .bg-green-trade { background: linear-gradient(135deg, #102016 0%, #0c1521 100%); box-shadow: inset 0 0 24px rgba(16,185,129,0.25), 0 0 22px rgba(16,185,129,0.25); }
    .bg-red-trade { background: linear-gradient(135deg, #1f0f12 0%, #0c1521 100%); box-shadow: inset 0 0 24px rgba(239,68,68,0.25), 0 0 22px rgba(239,68,68,0.25); }
    .prose { line-height: 1.65; color: #d1d5db; }
    .prose h1, .prose h2, .prose h3 { color: #fff; border-bottom: 1px solid #374151; padding-bottom: 0.3em; margin-bottom: 0.5em; }
    .prose strong { color: #e5e7eb; }
    .prose ul { list-style-type: disc; padding-right: 1.5em; }
    .prose li { margin-bottom: 0.25em; }
    .prose-invert { color: #d1d5db; }
    .brand-ig, .brand-wa { background: rgba(17,24,39,0.6); border: 1px solid #334155; transition: transform 0.2s ease, box-shadow 0.2s ease; }
    .brand-ig:hover, .brand-wa:hover { transform: translateY(-2px); box-shadow: 0 8px 18px rgba(0,0,0,0.25); }
    ::-webkit-scrollbar { width: 8px; }
    ::-webkit-scrollbar-track { background: #1f2937; }
    ::-webkit-scrollbar-thumb { background: #4b5563; border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #6b7280; }
  </style>
</head>
<body class="p-4 min-h-screen flex flex-col items-center justify-center text-white">

  <!-- Language Switcher -->
  <div class="fixed top-4 right-4 z-50 flex gap-2">
    <button id="lang-ar" class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-md font-semibold text-sm transition-colors duration-200">العربية</button>
    <button id="lang-en" class="px-4 py-2 bg-gray-600 hover:bg-gray-700 text-white rounded-md font-semibold text-sm transition-colors duration-200">English</button>
  </div>

  <!-- Welcome Screen -->
  <div id="welcome-screen" class="min-h-screen w-full flex-col items-center justify-center p-4 text-white bg-gradient-to-br from-[#0c1521] to-[#1a2033] via-[#10192a] flex">
    <div class="container max-w-4xl p-8 rounded-3xl glowing-shadow text-center md:text-right relative overflow-hidden">
      <div class="relative z-10">
        <h1 class="text-4xl lg:text-5xl font-extrabold mb-8 text-green-400">JO AI <span class="text-gray-200" data-lang-key="featuresTitle">Features</span></h1>
        
        <div class="ai-badge ai-badge-strong mb-6 rounded-2xl p-4 text-right">
          <div class="flex items-center gap-3">
            <span class="text-purple-400 text-2xl">⚡</span>
            <div>
              <p class="text-lg font-bold text-purple-300" data-lang-key="aiPowerTitle">AI Power: Ultra AI (Feature)</p>
              <p class="text-sm text-gray-300 mt-1" data-lang-key="aiPowerDesc">Ultra mode automatically combines Gemini and OpenAI and applies self-refinement before output, to improve accuracy and clarity. Always on—not a button.</p>
            </div>
          </div>
          <p class="text-xs text-gray-400 mt-3" data-lang-key="aiPowerHint">Note: Performance is high but results are not guaranteed, and this is not financial advice.</p>
        </div>

        <div class="ai-badge mb-6 rounded-2xl p-4 text-right">
          <div class="flex items-center gap-3">
            <span class="text-purple-400 text-2xl">🚀</span>
            <div>
              <p class="text-lg font-bold text-purple-300" data-lang-key="aiProTitle">AI Power: Ultra AI Pro (Feature)</p>
              <p class="text-sm text-gray-300 mt-1" data-lang-key="aiProDesc">Pro version enhances Ultra AI with a double refinement pass and consistency check before output. Activates automatically with Turbo enabled and both Gemini & OpenAI keys provided.</p>
            </div>
          </div>
        </div>

        <div class="ai-badge mb-6 rounded-2xl p-4 text-right">
          <div class="flex items-center gap-3">
            <span class="text-purple-400 text-2xl">🛡️</span>
            <div>
              <p class="text-lg font-bold text-purple-300" data-lang-key="aiSuperTitle">AI Power: Super Ultra AI (Feature)</p>
              <p class="text-sm text-gray-300 mt-1" data-lang-key="aiSuperDesc">The ultimate mode: multi-draft fusion + rigorous final audit to minimize errors and polish structure. Activates automatically with Turbo and High-Load modes enabled.</p>
            </div>
          </div>
        </div>

        <ul class="space-y-4 text-lg md:text-xl text-right">
          <li class="flex items-start md:items-center"><span class="text-yellow-400 text-2xl ml-3">🔍</span><span class="font-medium text-gray-300" data-lang-key="feature1">Live Market Insights – Tracks prices, market cap, FDV, and token holders.</span></li>
          <li class="flex items-start md:items-center"><span class="text-blue-400 text-2xl ml-3">📈</span><span class="font-medium text-gray-300" data-lang-key="feature2">Token Analytics – Analyzes supply, tokenomics, and unlock schedules.</span></li>
          <li class="flex items-start md:items-center"><span class="text-purple-400 text-2xl ml-3">💡</span><span class="font-medium text-gray-300" data-lang-key="feature3">AI-Powered Insights – Detects trends, sentiment.</span></li>
          <li class="flex items-start md:items-center"><span class="text-green-400 text-2xl ml-3">📊</span><span class="font-medium text-gray-300" data-lang-key="feature4">Technical & Fundamental Analysis – Evaluates price stability, risks, and entry points.</span></li>
          <li class="flex items-start md:items-center"><span class="text-orange-400 text-2xl ml-3">💼</span><span class="font-medium text-gray-300" data-lang-key="feature5">Exchange & Liquidity – Lists exchanges, liquidity scores, and supported wallets.</span></li>
          <li class="flex items-start md:items-center"><span class="text-pink-400 text-2xl ml-3">🌐</span><span class="font-medium text-gray-300" data-lang-key="feature6">Community Tracking – Monitors Twitter, Telegram, and Discord engagement.</span></li>
          <li class="flex items-start md:items-center"><span class="text-red-400 text-2xl ml-3">🔒</span><span class="font-medium text-gray-300" data-lang-key="feature7">Security & Audit Reports – Highlights security scores and past audits.</span></li>
        </ul>

        <div class="mt-10 border-t border-gray-700 pt-6">
          <h2 class="text-xl font-bold text-green-400 mb-2" data-lang-key="contactAdminTitle">Contact Admin</h2>
          <p class="text-gray-300 text-sm mb-4" data-lang-key="contactAdminDesc">Need help or faster approval? Reach out via:</p>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
            <a href="https://instagram.com/_sn8d" target="_blank" rel="noopener noreferrer" class="p-3 rounded-lg font-semibold flex items-center justify-center gap-2 brand-ig">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="#E1306C" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><path d="M7 2C4.24 2 2 4.24 2 7v10c0 2.76 2.24 5 5 5h10c2.76 0 5-2.24 5-5V7c0-2.76-2.24-5-5-5H7zm10 2c1.66 0 3 1.34 3 3v10c0 1.66-1.34 3-3 3H7c-1.66 0-3-1.34-3-3V7c0-1.66 1.34-3 3-3h10zm-5 3.5A5.5 5.5 0 1 0 17.5 13 5.507 5.507 0 0 0 12 7.5zm0 2A3.5 3.5 0 1 1 8.5 13 3.5 3.5 0 0 1 12 9.5zM18 6.3a1 1 0 1 0 1 1 1.001 1.001 0 0 0-1-1z"/></svg>
              <span data-lang-key="instagramLabel">Instagram</span>
              <span class="text-gray-300" dir="ltr">_sn8d</span>
            </a>
            <a href="https://wa.me/962782643348" target="_blank" rel="noopener noreferrer" class="p-3 rounded-lg font-semibold flex items-center justify-center gap-2 brand-wa">
              <svg width="20" height="20" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><path fill="#25D366" d="M20.52 3.48A11.77 11.77 0 0 0 12.01 0C5.4 0 .03 5.37.03 11.98c0 2.11.55 4.17 1.6 5.99L0 24l6.2-1.6a11.94 11.94 0 0 0 5.8 1.48h.01c6.61 0 11.98-5.37 11.98-11.98a11.94 11.94 0 0 0-3.47-8.42zM12 21.5h-.01a9.53 9.53 0 0 1-4.86-1.31l-.35-.2-3.68.95.98-3.58-.23-.37A9.5 9.5 0 1 1 21.5 12a9.48 9.48 0 0 1-2.78 6.76A9.45 9.45 0 0 1 12 21.5zm4.01-10.1c-.27-.14-1.62-.8-1.87-.89-.25-.09-.44-.14-.62.13-.18.27-.71.89-.87 1.07-.16.18-.32.21-.6.07-.27-.13-1.15-.42-2.19-1.35-.81-.72-1.36-1.61-1.52-1.88-.16-.27-.02-.42.12-.56.13-.12.27-.32.41-.48.14-.16.18-.27.27-.45.09-.18.05-.34-.02-.48-.07-.14-.62-1.49-.85-2.04-.22-.54-.45-.47-.62-.48l-.53-.01c-.18 0-.48.07-.73.34-.25.27-.96.94-.96 2.29 0 1.34.99 2.66 1.13 2.84.14.19 1.94 2.97 4.71 4.16.66.28 1.17.45 1.57.58.66.21 1.26.18 1.73.11.53-.08 1.62-.66 1.85-1.3.23-.64.23-1.18.16-1.3-.07-.12-.25-.19-.52-.33z"/></svg>
              <span data-lang-key="whatsappLabel">WhatsApp</span>
              <span class="text-gray-300" dir="ltr">+962782643348</span>
            </a>
          </div>
        </div>

        <button id="proceed-to-login-btn" class="button-style mt-12 px-8 py-4 text-xl rounded-full font-bold glowing-shadow-green" data-lang-key="loginButton">Login</button>
      </div>
    </div>
  </div>
    <!-- Login Screen -->
  <div id="login-form-container" class="hidden w-full max-w-sm">
    <div class="container p-8 rounded-3xl glowing-shadow">
      <div class="text-center mb-8">
        <h1 class="text-3xl font-bold text-white mb-2" data-lang-key="loginToJOUAI">Login to JO AI</h1>
      </div>
      <form id="login-form" class="space-y-6">
        <div>
          <label for="email" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="emailLabel">Email:</label>
          <input type="email" id="email" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300" placeholder="Enter email" required />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="passwordLabel">Password:</label>
          <input type="password" id="password" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300" placeholder="Enter password" required />
        </div>
        <button id="login-submit-btn" type="submit" class="w-full p-3 button-style rounded-lg font-semibold shadow-md flex items-center justify-center gap-2">
          <span id="login-spinner" class="hidden loading-animation w-5 h-5 border-2 rounded-full"></span>
          <span data-lang-key="loginSubmit">Login</span>
        </button>
      </form>
    </div>
  </div>
    <!-- Main App Container -->
  <div id="main-app-container" class="container w-full max-w-6xl p-6 md:p-8 rounded-3xl glowing-shadow hidden bg-default">
    <!-- Header -->
    <div class="flex items-center justify-between mb-6 text-center">
      <div></div> <!-- Spacer -->
      <div class="flex-grow">
        <h1 class="text-4xl font-bold text-white mb-2">JO AI</h1>
        <p class="text-gray-400" data-lang-key="mainAppSubtitle">Your AI-Powered Market Analysis Tool</p>
      </div>
      <div class="flex flex-col items-end">
        <p id="user-id-display" class="text-xs text-gray-500" data-lang-key="idPrefix">ID: </p>
        <button id="logout-btn" class="mt-1 text-xs text-red-400 hover:text-red-300 transition-colors" data-lang-key="logoutButton">Logout</button>
      </div>
    </div>

    <!-- Status Bar -->
    <div class="status-bar p-4 rounded-2xl mb-6 flex flex-col md:flex-row gap-4 items-start md:items-center">
      <div class="flex items-center gap-2">
        <span class="status-pill text-gray-300" id="status-badge">Idle</span>
        <span class="text-xs text-gray-400" id="status-warmup"></span>
        <span id="ai-power-pill" class="status-pill text-purple-300 border-purple-500/40 bg-purple-900/20 ml-2">⚡ <span id="ai-power-label">Ultra AI</span> • <span data-lang-key="aiAlwaysOn">Always On</span></span>
      </div>
      <div class="flex-1 grid grid-cols-2 md:grid-cols-5 gap-3 text-sm">
        <div class="bg-gray-900/40 border border-gray-700 rounded-lg p-2">
          <div class="text-gray-400" data-lang-key="latencyLabel">Latency</div>
          <div class="text-green-300 font-semibold" id="status-latency">—</div>
        </div>
        <div class="bg-gray-900/40 border border-gray-700 rounded-lg p-2">
          <div class="text-gray-400" data-lang-key="avgLatencyLabel">Average</div>
          <div class="text-green-300 font-semibold" id="status-avg-latency">—</div>
        </div>
        <div class="bg-gray-900/40 border border-gray-700 rounded-lg p-2">
          <div class="text-gray-400" data-lang-key="analysisQueueLabel">Analysis Queue</div>
          <div class="text-blue-300 font-semibold" id="status-queue-analysis">0</div>
        </div>
        <div class="bg-gray-900/40 border border-gray-700 rounded-lg p-2">
          <div class="text-gray-400" data-lang-key="chatQueueLabel">Chat Queue</div>
          <div class="text-blue-300 font-semibold" id="status-queue-chat">0</div>
        </div>
        <div class="flex gap-2 items-center">
          <button id="cancel-analysis-btn" class="px-3 py-2 bg-red-600 hover:bg-red-700 text-white text-xs rounded-md hidden" data-lang-key="cancelAnalysis">Cancel Analysis</button>
          <button id="cancel-chat-btn" class="px-3 py-2 bg-red-600 hover:bg-red-700 text-white text-xs rounded-md hidden" data-lang-key="cancelChat">Cancel Chat</button>
        </div>
      </div>
    </div>

    <!-- App Grid -->
    <div class="app-grid">
            <!-- Control Panel -->
      <div class="control-panel p-6 rounded-3xl space-y-6">
        <section class="space-y-4">
          <h3 class="text-lg font-bold text-green-400">1) <span data-lang-key="providerSectionTitle">Provider & API Key</span></h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="provider-select" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="providerLabel">Model Provider:</label>
              <select id="provider-select" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300">
                <option value="gemini" selected data-lang-key="providerGemini">Google Gemini</option>
                <option value="openai" data-lang-key="providerOpenAI">OpenAI (ChatGPT)</option>
              </select>
            </div>
            <div>
              <label for="api-key-input" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="apiKeyLabel">Gemini API Key:</label>
              <input type="password" id="api-key-input" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300 placeholder-gray-500" placeholder="Enter your Google AI Studio key" data-lang-key="apiKeyPlaceholder" />
            </div>
            <div id="openai-key-container" class="md:col-span-2">
              <label for="openai-api-key-input" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="openaiApiKeyLabel">OpenAI Key:</label>
              <input type="password" id="openai-api-key-input" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300 placeholder-gray-500" placeholder="Enter OpenAI key (optional)" />
              <p class="text-xs text-gray-500 mt-1" data-lang-key="openaiNote">This enables Ultra AI Pro/Super features.</p>
            </div>
          </div>
          <div class="grid grid-cols-1">
            <div>
              <label class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="performanceLabel">الأداء:</label>
              <div class="flex flex-col gap-2">
                <label class="inline-flex items-center gap-2 text-sm text-gray-300">
                  <input id="fast-mode-toggle" type="checkbox" class="accent-green-500" checked>
                  <span data-lang-key="fastModeLabel">Fast Response Mode</span>
                </label>
                <label class="inline-flex items-center gap-2 text-sm text-gray-300">
                  <input id="highload-mode-toggle" type="checkbox" class="accent-green-500" checked>
                  <span data-lang-key="highLoadLabel">High Load Tolerance</span>
                </label>
                <label class="inline-flex items-center gap-2 text-sm text-gray-300">
                  <input id="turbo-mode-toggle" type="checkbox" class="accent-green-500">
                  <span data-lang-key="turboModeLabel">Turbo Mode (Max Speed)</span>
                </label>
                <p class="text-xs text-gray-500 mt-1" data-lang-key="performanceHint">Improves latency, queue management, and auto-fallback under pressure.</p>
              </div>
            </div>
          </div>
        </section>

        <section class="space-y-4">
          <h3 class="text-lg font-bold text-green-400">2) <span data-lang-key="strategyTitle">Strategy</span></h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="trading-style-select" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="tradingStyleLabel">Trading Style:</label>
              <select id="trading-style-select" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300">
                <option value="Scalping" data-lang-key="scalpingOption">Scalping</option>
                <option value="Swing" selected data-lang-key="swingOption">Swing</option>
                <option value="Investment" data-lang-key="investmentOption">Investment</option>
              </select>
            </div>
            <div>
              <label for="school-select" class="block text-sm font-medium text-gray-300 mb-2" data-lang-key="schoolLabel">Analytical School:</label>
              <select id="school-select" class="w-full p-3 text-sm text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300">
                <option value="ICT" selected data-lang-key="schoolICT">ICT School</option>
                <option value="SMC" data-lang-key="schoolSMC">SMC School</option>
                <option value="Time-based" data-lang-key="schoolTime">Time-based School</option>
                <option value="Elliott Wave" data-lang-key="schoolElliott">Elliott Wave School</option>
              </select>
            </div>
          </div>
        </section>

        <section class="space-y-4">
          <h3 class="text-lg font-bold text-green-400">3) <span data-lang-key="inputSectionTitle">Input & Images</span></h3>
          <div>
            <textarea id="input-query" rows="4" class="w-full p-4 text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300 placeholder-gray-400" placeholder="Ask me about the market... anything. I know the answer." data-lang-key="inputQueryPlaceholder"></textarea>
          </div>
          <div>
            <input type="file" id="image-upload" accept="image/*" class="hidden" multiple>
            <div class="flex justify-start">
              <button id="upload-btn" class="px-4 py-2 bg-gray-700 hover:bg-gray-600 text-white font-semibold rounded-lg transition-all duration-300" data-lang-key="uploadImageBtn">Upload Images</button>
            </div>
            <div id="image-preview-container" class="hidden mt-4 p-2 rounded-lg border border-gray-700 bg-gray-900/50">
              <div id="image-previews-wrapper" class="flex flex-wrap gap-4"></div>
            </div>
          </div>
        </section>

        <section class="space-y-3">
          <h3 class="text-lg font-bold text-green-400">4) <span data-lang-key="actionsTitle">Quick Actions</span></h3>
          <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
            <button id="analyze-btn" class="p-3 button-style rounded-lg font-semibold shadow-md" data-lang-key="technicalAnalysisBtn">Technical Analysis</button>
            <button id="fundamental-btn" class="p-3 button-style rounded-lg font-semibold shadow-md" data-lang-key="fundamentalAnalysisBtn">Fundamental Analysis</button>
            <button id="direct-trade-btn" class="p-3 button-style rounded-lg font-semibold shadow-md" data-lang-key="directTradeBtn">Trade Recommendation</button>
            <button id="high-conviction-trade-btn" class="p-3 button-style rounded-lg font-semibold shadow-md" data-lang-key="highConvictionBtn">High-Conviction Trade</button>
          </div>
        </section>
      </div>
            <!-- Right Side: Output + Chat -->
      <div class="space-y-6">
        <section id="output-section" class="relative bg-gray-800 rounded-lg p-4 min-h-[200px] border border-gray-700 shadow-inner">
          <div id="loading-spinner" class="hidden absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2">
            <div class="loading-animation w-12 h-12 border-4 rounded-full"></div>
          </div>
          <div id="output-text" class="prose max-w-none text-gray-300 text-sm leading-relaxed prose-invert"></div>
        </section>

        <section id="chat-panel" class="chat-panel p-6 rounded-3xl">
          <div class="flex items-center justify-between mb-3">
            <h2 class="text-2xl font-bold text-white" data-lang-key="chatTitle">Advanced GPT Chat</h2>
            <div class="flex items-center gap-2 text-xs text-gray-400">
              <span class="hidden md:inline" data-lang-key="chatHint">Supports context & images • Select provider above</span>
            </div>
          </div>

          <div id="chat-messages" class="chat-messages bg-gray-900/40 rounded-lg border border-gray-700"></div>

          <div class="mt-4">
            <textarea id="chat-input" rows="3" class="w-full p-3 text-gray-100 bg-gray-900 rounded-lg outline-none border border-gray-700 focus:ring-2 focus:ring-blue-500 transition-all duration-300 placeholder-gray-400" placeholder="Type your message..." data-lang-key="chatPlaceholder"></textarea>
            <input type="file" id="chat-image-upload" accept="image/*" class="hidden" multiple>
            <div id="chat-image-preview-container" class="hidden mt-3 p-2 rounded-lg border border-gray-700 bg-gray-900/50">
              <div id="chat-image-previews-wrapper" class="flex flex-wrap gap-3"></div>
            </div>

            <div class="flex items-center gap-3 mt-3">
              <button id="chat-upload-btn" class="px-4 py-2 bg-gray-700 hover:bg-gray-600 text-white font-semibold rounded-lg transition-all duration-300" data-lang-key="uploadImageChatBtn">Attach Images</button>
              <button id="chat-clear-btn" class="px-4 py-2 bg-gray-700 hover:bg-gray-600 text-white font-semibold rounded-lg transition-all duration-300" data-lang-key="clearChat">Clear Chat</button>
              <div class="ml-auto flex items-center gap-2">
                <div id="chat-loading" class="hidden loading-animation w-6 h-6 border-2 rounded-full"></div>
                <button id="chat-send-btn" class="px-6 py-2 button-style rounded-lg font-semibold" data-lang-key="sendButton">Send</button>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div> <!-- end .app-grid -->
        <!-- Admin Panel -->
    <div id="admin-panel" class="hidden mt-6 p-6 bg-gray-900 rounded-2xl shadow-inner border border-gray-700">
      <h2 class="text-2xl font-bold text-white mb-4" data-lang-key="manageUsersTitle">Manage Users</h2>
      <div id="users-list" class="space-y-4">
        <div class="flex justify-center items-center p-4">
          <div class="loading-animation w-8 h-8 border-4 rounded-full"></div>
        </div>
      </div>
    </div>
  </div> <!-- end #main-app-container -->
    <!-- Modals -->
  <div id="message-modal" class="hidden fixed inset-0 z-[100] flex items-center justify-center p-4" style="background-color: rgba(12,21,33,0.8); backdrop-filter: blur(4px);">
    <div class="p-6 rounded-xl shadow-2xl w-full max-w-sm" style="background-color:#1f2937; border:1px solid #334155; box-shadow:0 8px 25px rgba(0,0,0,0.4)">
      <div class="text-center">
        <p id="modal-text" class="text-lg text-white mb-4"></p>
        <button id="close-modal-btn" class="px-6 py-2 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-full transition-transform duration-200 transform hover:scale-105" data-lang-key="okButton">OK</button>
      </div>
    </div>
  </div>

  <div id="invincible-modal" class="hidden fixed inset-0 z-[100] flex items-center justify-center p-4" style="background-color: rgba(12,21,33,0.8); backdrop-filter: blur(4px);">
    <div class="p-6 rounded-xl shadow-2xl w-full max-w-lg" style="background-color:#1f2937; border:1px solid #334155; box-shadow:0 8px 25px rgba(0,0,0,0.4)">
      <div class="text-center">
        <h3 class="text-2xl font-bold text-green-400 mb-4" data-lang-key="highConvictionModalTitle">High-Conviction Trade</h3>
        <p class="text-gray-300 mb-6" data-lang-key="highConvictionModalText">My analysis is based on countless data points, and this recommendation has the highest possible conviction. However, there is no guarantee of success in the market. Are you ready to proceed?</p>
        <div class="flex justify-center gap-4">
          <button id="confirm-invincible-btn" class="px-6 py-2 bg-green-600 hover:bg-green-700 text-white font-semibold rounded-full transition-transform duration-200 transform hover:scale-105" data-lang-key="yesImReady">Yes, I'm Ready</button>
          <button id="cancel-invincible-btn" class="px-6 py-2 bg-gray-600 hover:bg-gray-700 text-white font-semibold rounded-full transition-transform duration-200 transform hover:scale-105" data-lang-key="cancelButton">Cancel</button>
        </div>
      </div>
    </div>
  </div>
  
  <div id="confirm-delete-modal" class="hidden fixed inset-0 z-[100] flex items-center justify-center p-4" style="background-color: rgba(12,21,33,0.8); backdrop-filter: blur(4px);">
    <div class="p-6 rounded-xl shadow-2xl w-full max-w-sm" style="background-color:#1f2937; border:1px solid #334155; box-shadow:0 8px 25px rgba(0,0,0,0.4)">
      <div class="text-center">
        <h3 class="text-xl font-bold text-red-400 mb-4" data-lang-key="confirmDeleteTitle">Confirm Deletion</h3>
        <p id="confirm-delete-text" class="text-gray-300 mb-6" data-lang-key="confirmDeleteText">Are you sure you want to delete this user? This action cannot be undone.</p>
        <div class="flex justify-center gap-4">
          <button id="confirm-delete-btn" class="px-6 py-2 bg-red-600 hover:bg-red-700 text-white font-semibold rounded-full" data-lang-key="deleteButton">Delete</button>
          <button id="cancel-delete-btn" class="px-6 py-2 bg-gray-600 hover:bg-gray-700 text-white font-semibold rounded-full" data-lang-key="cancelButton">Cancel</button>
        </div>
      </div>
    </div>
  </div>
  <script type="module">
  // 1) Firebase imports
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
  import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-analytics.js";
  import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
  import { getFirestore, doc, getDoc, setDoc, collection, getDocs, deleteDoc, updateDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

  // 2) Translations
  const translations = {
    ar: {
      pageTitle: "JO AI", featuresTitle: "الميزات", aiPowerTitle: "قوة الذكاء الاصطناعي: Ultra AI (ميزة)",
      aiPowerDesc: "يجمع وضع Ultra تلقائيًا بين Gemini و OpenAI ويطبق التحسين الذاتي قبل الإخراج، لتحسين الدقة والوضوح. يعمل دائمًا - وليس زرًا.",
      aiPowerHint: "ملاحظة: الأداء مرتفع ولكن النتائج غير مضمونة، وهذه ليست نصيحة مالية.",
      aiProTitle: "قوة الذكاء الاصطناعي: Ultra AI Pro (ميزة)",
      aiProDesc: "تعزز نسخة Pro من Ultra AI بتمرير تحسين مزدوج وفحص الاتساق قبل الإخراج. يتم تفعيله تلقائيًا مع تمكين Turbo وتوفير مفاتيح Gemini و OpenAI.",
      aiSuperTitle: "قوة الذكاء الاصطناعي: Super Ultra AI (ميزة)",
      aiSuperDesc: "الوضع النهائي: دمج مسودات متعددة + تدقيق نهائي صارم لتقليل الأخطاء وصقل الهيكل. يتم تفعيله تلقائيًا مع تمكين وضعي Turbo و High-Load.",
      feature1: "رؤى مباشرة للسوق - تتبع الأسعار، القيمة السوقية، FDV، وحاملي الرموز.",
      feature2: "تحليلات الرموز - تحليل العرض، اقتصاديات الرموز، وجداول الفتح.",
      feature3: "رؤى مدعومة بالذكاء الاصطناعي - تكتشف الاتجاهات والمشاعر.",
      feature4: "تحليل فني وأساسي - تقييم استقرار الأسعار والمخاطر ونقاط الدخول.",
      feature5: "المنصات والسيولة - قوائم بالمنصات ودرجات السيولة والمحافظ المدعومة.",
      feature6: "تتبع المجتمع - مراقبة التفاعل على تويتر وتليجرام وديسكورد.",
      feature7: "تقارير الأمان والتدقيق - تسليط الضوء على درجات الأمان والتدقيقات السابقة.",
      contactAdminTitle: "اتصل بالمسؤول", contactAdminDesc: "تحتاج إلى مساعدة أو موافقة أسرع؟ تواصل عبر:",
      instagramLabel: "انستغرام", whatsappLabel: "واتساب",
      loginButton: "تسجيل الدخول", loginToJOUAI: "تسجيل الدخول إلى JO AI",
      emailLabel: "البريد الإلكتروني:", passwordLabel: "كلمة المرور:", loginSubmit: "دخول",
      mainAppSubtitle: "أداتك لتحليل السوق المدعومة بالذكاء الاصطناعي", idPrefix: "المعرف: ", logoutButton: "تسجيل الخروج",
      aiAlwaysOn: "يعمل دائمًا", latencyLabel: "الكمون", avgLatencyLabel: "المتوسط",
      analysisQueueLabel: "قائمة انتظار التحليل", chatQueueLabel: "قائمة انتظار الدردشة",
      cancelAnalysis: "إلغاء التحليل", cancelChat: "إلغاء الدردشة",
      providerSectionTitle: "الموفر ومفتاح API", providerLabel: "مزود النموذج:",
      providerGemini: "Google Gemini", providerOpenAI: "OpenAI (ChatGPT)", apiKeyLabel: "مفتاح Gemini API:",
      apiKeyPlaceholder: "أدخل مفتاح Google AI Studio الخاص بك", openaiApiKeyLabel: "مفتاح OpenAI:",
      openaiNote: "هذا يتيح ميزات Ultra AI Pro/Super.", performanceLabel: "الأداء:",
      fastModeLabel: "وضع الاستجابة السريعة", highLoadLabel: "تحمل الحمل العالي",
      turboModeLabel: "وضع Turbo (أقصى سرعة)", performanceHint: "يحسن الكمون وإدارة قائمة الانتظار والتراجع التلقائي تحت الضغط.",
      strategyTitle: "الاستراتيجية", tradingStyleLabel: "أسلوب التداول:",
      scalpingOption: "مضاربة سريعة", swingOption: "تداول متأرجح", investmentOption: "استثمار",
      schoolLabel: "المدرسة التحليلية:", schoolICT: "مدرسة ICT", schoolSMC: "مدرسة SMC",
      schoolTime: "المدرسة الزمنية", schoolElliott: "مدرسة موجات إليوت",
      inputSectionTitle: "الإدخال والصور", inputQueryPlaceholder: "اسألني عن السوق... أي شيء. أعرف الإجابة.",
      uploadImageBtn: "تحميل الصور", actionsTitle: "إجراءات سريعة",
      technicalAnalysisBtn: "تحليل فني", fundamentalAnalysisBtn: "تحليل أساسي",
      directTradeBtn: "توصية تداول", highConvictionBtn: "صفقة عالية القناعة",
      chatTitle: "دردشة GPT المتقدمة", chatHint: "تدعم السياق والصور • حدد الموفر أعلاه",
      chatPlaceholder: "اكتب رسالتك...", uploadImageChatBtn: "إرفاق الصور",
      clearChat: "مسح الدردشة", sendButton: "إرسال",
      manageUsersTitle: "إدارة المستخدمين", okButton: "حسنًا",
      highConvictionModalTitle: "صفقة عالية القناعة",
      highConvictionModalText: "تحليلي يعتمد على عدد لا يحصى من نقاط البيانات، وهذه التوصية لديها أعلى قناعة ممكنة. ومع ذلك، لا يوجد ضمان للنجاح في السوق. هل أنت مستعد للمتابعة؟",
      yesImReady: "نعم، أنا مستعد", cancelButton: "إلغاء",
      confirmDeleteTitle: "تأكيد الحذف", confirmDeleteText: "هل أنت متأكد من أنك تريد حذف هذا المستخدم؟ لا يمكن التراجع عن هذا الإجراء.",
      deleteButton: "حذف"
    },
    en: {
      pageTitle: "JO AI", featuresTitle: "Features", aiPowerTitle: "AI Power: Ultra AI (Feature)",
      aiPowerDesc: "Ultra mode automatically combines Gemini and OpenAI and applies self-refinement before output, to improve accuracy and clarity. Always on—not a button.",
      aiPowerHint: "Note: Performance is high but results are not guaranteed, and this is not financial advice.",
      aiProTitle: "AI Power: Ultra AI Pro (Feature)",
      aiProDesc: "Pro version enhances Ultra AI with a double refinement pass and consistency check before output. Activates automatically with Turbo enabled and both Gemini & OpenAI keys provided.",
      aiSuperTitle: "AI Power: Super Ultra AI (Feature)",
      aiSuperDesc: "The ultimate mode: multi-draft fusion + rigorous final audit to minimize errors and polish structure. Activates automatically with Turbo and High-Load modes enabled.",
      feature1: "Live Market Insights – Tracks prices, market cap, FDV, and token holders.",
      feature2: "Token Analytics – Analyzes supply, tokenomics, and unlock schedules.",
      feature3: "AI-Powered Insights – Detects trends, sentiment.",
      feature4: "Technical & Fundamental Analysis – Evaluates price stability, risks, and entry points.",
      feature5: "Exchange & Liquidity – Lists exchanges, liquidity scores, and supported wallets.",
      feature6: "Community Tracking – Monitors Twitter, Telegram, and Discord engagement.",
      feature7: "Security & Audit Reports – Highlights security scores and past audits.",
      contactAdminTitle: "Contact Admin", contactAdminDesc: "Need help or faster approval? Reach out via:",
      instagramLabel: "Instagram", whatsappLabel: "WhatsApp",
      loginButton: "Login", loginToJOUAI: "Login to JO AI",
      emailLabel: "Email:", passwordLabel: "Password:", loginSubmit: "Login",
      mainAppSubtitle: "Your AI-Powered Market Analysis Tool", idPrefix: "ID: ", logoutButton: "Logout",
      aiAlwaysOn: "Always On", latencyLabel: "Latency", avgLatencyLabel: "Average",
      analysisQueueLabel: "Analysis Queue", chatQueueLabel: "Chat Queue",
      cancelAnalysis: "Cancel Analysis", cancelChat: "Cancel Chat",
      providerSectionTitle: "Provider & API Key", providerLabel: "Model Provider:",
      providerGemini: "Google Gemini", providerOpenAI: "OpenAI (ChatGPT)", apiKeyLabel: "Gemini API Key:",
      apiKeyPlaceholder: "Enter your Google AI Studio key", openaiApiKeyLabel: "OpenAI Key:",
      openaiNote: "This enables Ultra AI Pro/Super features.", performanceLabel: "Performance:",
      fastModeLabel: "Fast Response Mode", highLoadLabel: "High Load Tolerance",
      turboModeLabel: "Turbo Mode (Max Speed)", performanceHint: "Improves latency, queue management, and auto-fallback under pressure.",
      strategyTitle: "Strategy", tradingStyleLabel: "Trading Style:",
      scalpingOption: "Scalping", swingOption: "Swing", investmentOption: "Investment",
      schoolLabel: "Analytical School:", schoolICT: "ICT School", schoolSMC: "SMC School",
      schoolTime: "Time-based School", schoolElliott: "Elliott Wave School",
      inputSectionTitle: "Input & Images", inputQueryPlaceholder: "Ask me about the market... anything. I know the answer.",
      uploadImageBtn: "Upload Images", actionsTitle: "Quick Actions",
      technicalAnalysisBtn: "Technical Analysis", fundamentalAnalysisBtn: "Fundamental Analysis",
      directTradeBtn: "Trade Recommendation", highConvictionBtn: "High-Conviction Trade",
      chatTitle: "Advanced GPT Chat", chatHint: "Supports context & images • Select provider above",
      chatPlaceholder: "Type your message...", uploadImageChatBtn: "Attach Images", clearChat: "Clear Chat", sendButton: "Send",
      manageUsersTitle: "Manage Users", okButton: "OK",
      highConvictionModalTitle: "High-Conviction Trade",
      highConvictionModalText: "My analysis is based on countless data points, and this recommendation has the highest possible conviction. However, there is no guarantee of success in the market. Are you ready to proceed?",
      yesImReady: "Yes, I'm Ready", cancelButton: "Cancel",
      confirmDeleteTitle: "Confirm Deletion", confirmDeleteText: "Are you sure you want to delete this user? This action cannot be undone.",
      deleteButton: "Delete"
    }
  };

  // 3) DOM elements
  const dom = {
    html: document.documentElement, body: document.body,
    langArBtn: document.getElementById('lang-ar'),
    langEnBtn: document.getElementById('lang-en'),
    welcomeScreen: document.getElementById('welcome-screen'),
    proceedToLoginBtn: document.getElementById('proceed-to-login-btn'),
    loginFormContainer: document.getElementById('login-form-container'),
    loginForm: document.getElementById('login-form'),
    loginSubmitBtn: document.getElementById('login-submit-btn'),
    loginSpinner: document.getElementById('login-spinner'),
    mainAppContainer: document.getElementById('main-app-container'),
    logoutBtn: document.getElementById('logout-btn'),
    userIdDisplay: document.getElementById('user-id-display'),

    // Status
    statusBadge: document.getElementById('status-badge'),
    statusWarmup: document.getElementById('status-warmup'),
    statusLatency: document.getElementById('status-latency'),
    statusAvgLatency: document.getElementById('status-avg-latency'),
    statusQueueAnalysis: document.getElementById('status-queue-analysis'),
    statusQueueChat: document.getElementById('status-queue-chat'),
    cancelAnalysisBtn: document.getElementById('cancel-analysis-btn'),
    cancelChatBtn: document.getElementById('cancel-chat-btn'),
    aiPowerPill: document.getElementById('ai-power-pill'),
    aiPowerLabel: document.getElementById('ai-power-label'),

    // Controls
    providerSelect: document.getElementById('provider-select'),
    apiKeyInput: document.getElementById('api-key-input'), // Gemini
    openaiKeyContainer: document.getElementById('openai-key-container'),
    openaiApiKeyInput: document.getElementById('openai-api-key-input'),
    fastModeToggle: document.getElementById('fast-mode-toggle'),
    highLoadToggle: document.getElementById('highload-mode-toggle'),
    turboModeToggle: document.getElementById('turbo-mode-toggle'),

    // Strategy
    tradingStyleSelect: document.getElementById('trading-style-select'),
    schoolSelect: document.getElementById('school-select'),

    // Analysis input
    analyzeBtn: document.getElementById('analyze-btn'),
    fundamentalBtn: document.getElementById('fundamental-btn'),
    directTradeBtn: document.getElementById('direct-trade-btn'),
    highConvictionTradeBtn: document.getElementById('high-conviction-trade-btn'),
    inputQuery: document.getElementById('input-query'),
    uploadBtn: document.getElementById('upload-btn'),
    imageUpload: document.getElementById('image-upload'),
    imagePreviewContainer: document.getElementById('image-preview-container'),
    imagePreviewsWrapper: document.getElementById('image-previews-wrapper'),

    // Output
    outputText: document.getElementById('output-text'),
    loadingSpinner: document.getElementById('loading-spinner'),

    // Chat
    chatMessages: document.getElementById('chat-messages'),
    chatInput: document.getElementById('chat-input'),
    chatSendBtn: document.getElementById('chat-send-btn'),
    chatUploadBtn: document.getElementById('chat-upload-btn'),
    chatImageUpload: document.getElementById('chat-image-upload'),
    chatImagePreviewContainer: document.getElementById('chat-image-preview-container'),
    chatImagePreviewsWrapper: document.getElementById('chat-image-previews-wrapper'),
    chatClearBtn: document.getElementById('chat-clear-btn'),
    chatLoading: document.getElementById('chat-loading'),

    // Admin
    adminPanel: document.getElementById('admin-panel'),
    usersList: document.getElementById('users-list'),

    // Modals
    messageModal: document.getElementById('message-modal'),
    modalText: document.getElementById('modal-text'),
    closeModalBtn: document.getElementById('close-modal-btn'),
    invincibleModal: document.getElementById('invincible-modal'),
    confirmInvincibleBtn: document.getElementById('confirm-invincible-btn'),
    cancelInvincibleBtn: document.getElementById('cancel-invincible-btn'),
    confirmDeleteModal: document.getElementById('confirm-delete-modal'),
    confirmDeleteBtn: document.getElementById('confirm-delete-btn'),
    cancelDeleteBtn: document.getElementById('cancel-delete-btn'),
  };

  // 4) App state
  const state = {
    currentUser: null,
    isApproved: false,
    isAdmin: false,
    currentLanguage: 'ar',
    analysisImages: [],
    chatImages: [],
    chatHistory: [],
    isAnalysisRunning: false,
    isChatRunning: false,
    userToDelete: null,
    analysisAbortController: null,
    chatAbortController: null,
    analysisQueue: 0,
    chatQueue: 0,
    latenciesMs: [],
    firstWarmupShown: false
  };

  // 5) Firebase Config
  const firebaseConfig = {
    apiKey: "AIzaSyD3VmhaqQRpc7MGek5lP2AYkPpsiImtaYQ",
    authDomain: "joai-83ad0.firebaseapp.com",
    projectId: "joai-83ad0",
    storageBucket: "joai-83ad0.appspot.com",
    messagingSenderId: "742596595841",
    appId: "1:742596595841:web:ed41812970be3c16957d80",
    measurementId: "G-PHXKPMVGJ8"
  };

  // 6) Init Firebase
  const app = initializeApp(firebaseConfig);
  try { getAnalytics(app); } catch (e) { console.warn("Firebase Analytics not available in this environment."); }
  const auth = getAuth(app);
  const db = getFirestore(app);

  // 7) Language
  function setLanguage(lang) {
    state.currentLanguage = lang;
    localStorage.setItem('jo-ai-lang', lang);
    const dict = translations[lang];
    dom.html.lang = lang;
    dom.html.dir = lang === 'ar' ? 'rtl' : 'ltr';
    document.querySelectorAll('[data-lang-key]').forEach(el => {
      const key = el.getAttribute('data-lang-key');
      if (dict[key]) {
        if (el.tagName === 'INPUT' || el.tagName === 'TEXTAREA') {
          el.placeholder = dict[key];
        } else {
          el.textContent = dict[key];
        }
      }
    });
    if (lang === 'ar') {
      dom.langArBtn.classList.replace('bg-gray-600', 'bg-blue-600');
      dom.langEnBtn.classList.replace('bg-blue-600', 'bg-gray-600');
    } else {
      dom.langEnBtn.classList.replace('bg-gray-600', 'bg-blue-600');
      dom.langArBtn.classList.replace('bg-blue-600', 'bg-gray-600');
    }
  }

  // 8) UI helpers
  function showScreen(screen) {
    ['welcome', 'login', 'app'].forEach(s => {
      const el = document.getElementById(s === 'login' ? 'login-form-container' : s === 'app' ? 'main-app-container' : `${s}-screen`);
      if (!el) return;
      el.classList.toggle('hidden', s !== screen);
      el.classList.toggle('flex', s !== 'app' && s === screen);
    });
  }

  function showMessageModal(message) {
    dom.modalText.textContent = message;
    dom.messageModal.classList.remove('hidden');
  }
  function hideMessageModal() { dom.messageModal.classList.add('hidden'); }

  function mdToHTML(markdownText) {
    try {
      const raw = window.marked.parse(markdownText ?? '', { breaks: true, gfm: true });
      return window.DOMPurify.sanitize(raw);
    } catch {
      const safe = String(markdownText ?? '').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/\n/g,'<br>');
      return window.DOMPurify.sanitize(safe);
    }
  }

  function fileToBase64(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => resolve(reader.result.split(',')[1]);
      reader.onerror = error => reject(error);
    });
  }

  function updateImagePreviews(type) {
    const wrapper = type === 'analysis' ? dom.imagePreviewsWrapper : dom.chatImagePreviewsWrapper;
    const container = type === 'analysis' ? dom.imagePreviewContainer : dom.chatImagePreviewContainer;
    const images = type === 'analysis' ? state.analysisImages : state.chatImages;
    wrapper.innerHTML = '';
    if (images.length > 0) {
      images.forEach((img, index) => {
        const previewEl = document.createElement('div');
        previewEl.className = 'relative';
        previewEl.innerHTML = `
          <img src="${URL.createObjectURL(img.file)}" class="image-preview-thumbnail" alt="Preview">
          <button data-type="${type}" data-index="${index}" class="remove-image-btn text-xs">X</button>`;
        wrapper.appendChild(previewEl);
      });
      container.classList.remove('hidden');
    } else {
      container.classList.add('hidden');
    }
  }

  function computeAIMode() {
    const hasGemini = (dom.apiKeyInput.value || '').trim().length > 0;
    const hasOpenAI = (dom.openaiApiKeyInput.value || '').trim().length > 0;
    const turbo = dom.turboModeToggle.checked;
    const highLoad = dom.highLoadToggle.checked;
    if (turbo && highLoad && hasGemini && hasOpenAI) return 'Super Ultra AI';
    if (turbo && hasGemini && hasOpenAI) return 'Ultra AI Pro';
    return 'Ultra AI';
  }
  function updateAIPowerUI() {
    const mode = computeAIMode();
    dom.aiPowerLabel.textContent = mode;
  }

  function setStatusBusy(type, busy) {
    if (type === 'analysis') {
      state.isAnalysisRunning = busy;
      state.analysisQueue += busy ? 1 : -1;
      dom.statusQueueAnalysis.textContent = Math.max(0, state.analysisQueue);
      dom.cancelAnalysisBtn.classList.toggle('hidden', !busy);
    } else {
      state.isChatRunning = busy;
      state.chatQueue += busy ? 1 : -1;
      dom.statusQueueChat.textContent = Math.max(0, state.chatQueue);
      dom.cancelChatBtn.classList.toggle('hidden', !busy);
    }
    dom.statusBadge.textContent = (state.isAnalysisRunning || state.isChatRunning) ? 'Working...' : 'Idle';
  }

  function recordLatency(ms) {
    state.latenciesMs.push(ms);
    if (state.latenciesMs.length > 10) state.latenciesMs.shift();
    const avg = state.latenciesMs.reduce((a,b)=>a+b,0)/(state.latenciesMs.length || 1);
    dom.statusLatency.textContent = `${Math.round(ms)} ms`;
    dom.statusAvgLatency.textContent = `${Math.round(avg)} ms`;
  }
      // 9) Auth & users
  onAuthStateChanged(auth, async (user) => {
    if (user) {
      const userRef = doc(db, "users", user.uid);
      const userDoc = await getDoc(userRef);
      
      state.currentUser = user;
      state.isAdmin = user.email === "admin@jouai.com"; // Set admin role

      if (state.isAdmin) {
        state.isApproved = true;
      } else if (userDoc.exists()) {
        state.isApproved = userDoc.data().approved === true;      } else {
        // First time login for a non-admin, create their record
        await setDoc(userRef, { email: user.email, approved: false, role: 'user' });
        state.isApproved = false;
      }

      if (state.isApproved) {
        dom.userIdDisplay.textContent = `${translations[state.currentLanguage].idPrefix} ${user.uid}`;
        showScreen('app');
        if (state.isAdmin) {
          dom.adminPanel.classList.remove('hidden');
          await loadUsersForAdmin();
        } else {
          dom.adminPanel.classList.add('hidden');
        }
      } else {
        const msg = state.currentLanguage === 'ar' ? 'حسابك قيد المراجعة. يرجى الاتصال بالمسؤول للتفعيل.' : 'Your account is pending approval. Please contact the administrator.';
        showMessageModal(msg);
        await signOut(auth);
      }
    } else {
      state.currentUser = null;
      state.isAdmin = false;
      state.isApproved = false;
      showScreen('welcome');
      dom.adminPanel.classList.add('hidden');
    }
  });

  async function handleLogin(e) {
    e.preventDefault();
    dom.loginSpinner.classList.remove('hidden');
    dom.loginSubmitBtn.disabled = true;
    const email = dom.loginForm.email.value;
    const password = dom.loginForm.password.value;

    try {
      await signInWithEmailAndPassword(auth, email, password);
      // onAuthStateChanged will handle the UI transition and approval check
    } catch (error) {
      console.error("Login failed:", error);
      let message = state.currentLanguage === 'ar' ? 'فشل تسجيل الدخول. يرجى التحقق من بياناتك.' : 'Login failed. Please check your credentials.';
      if (error.code === 'auth/user-not-found' || error.code === 'auth/wrong-password' || error.code === 'auth/invalid-credential') {
        message = state.currentLanguage === 'ar' ? 'البريد الإلكتروني أو كلمة المرور غير صحيحة.' : 'Incorrect email or password.';
      }
      showMessageModal(message);
    } finally {
      dom.loginSpinner.classList.add('hidden');
      dom.loginSubmitBtn.disabled = false;
    }
  }

  async function handleLogout() {
    try {
      await signOut(auth);
      state.chatHistory = [];
      dom.chatMessages.innerHTML = '';
    } catch (error) {      console.error("Logout failed:", error);
    }
  }
  
  // --- ADMIN PANEL ---
  async function loadUsersForAdmin() {
    if (!state.isAdmin) return;
    dom.usersList.innerHTML = `<div class="flex justify-center items-center p-4"><div class="loading-animation w-8 h-8 border-4 rounded-full"></div></div>`;
    try {
      const usersCollection = collection(db, "users");
      const querySnapshot = await getDocs(usersCollection);
      const users = [];
      querySnapshot.forEach(doc => {
        if (doc.data().email !== "admin@jouai.com") {
          users.push({ id: doc.id, ...doc.data() });
        }
      });

      if (users.length === 0) {
        dom.usersList.innerHTML = `<p class="text-gray-400 text-center">No users found.</p>`;
        return;
      }
      dom.usersList.innerHTML = users.map(user => `
        <div class="p-4 bg-gray-800 rounded-lg flex items-center justify-between">
          <div>
            <p class="font-semibold text-white">${user.email}</p>
            <p class="text-xs text-gray-400">${user.id}</p>
          </div>
          <div class="flex items-center gap-3">
            <label class="flex items-center cursor-pointer">
              <span class="mr-2 text-sm font-medium ${user.approved ? 'text-green-400' : 'text-gray-400'}">${user.approved ? 'Approved' : 'Pending'}</span>
              <div class="relative">
                <input type="checkbox" data-uid="${user.id}" class="sr-only peer approve-toggle" ${user.approved ? 'checked' : ''}>
                <div class="w-11 h-6 bg-gray-600 rounded-full peer peer-focus:ring-4 peer-focus:ring-green-800 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-0.5 after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-600"></div>
              </div>
            </label>
            <button data-uid="${user.id}" class="delete-user-btn px-3 py-1 bg-red-600 hover:bg-red-700 text-white text-xs font-bold rounded-md">Delete</button>
          </div>
        </div>      `).join('');
    } catch (error) {
      console.error("Failed to load users:", error);      dom.usersList.innerHTML = `<p class="text-red-400 text-center">Error loading users.</p>`;
    }
  }
  
  async function handleUserApproval(uid, isApproved) {
    if (!state.isAdmin) return;
    try {
      const userRef = doc(db, "users", uid);
      await updateDoc(userRef, { approved: isApproved });    } catch(error) {
      console.error("Failed to update user approval", error);
      showMessageModal("Failed to update user status.");
      loadUsersForAdmin();
    }
  }

  function confirmUserDeletion(uid) {
    state.userToDelete = uid;
    dom.confirmDeleteModal.classList.remove('hidden');
  }
  
  async function deleteUser() {
    if (!state.isAdmin || !state.userToDelete) return;
    const uid = state.userToDelete;
    try {
      await deleteDoc(doc(db, "users", uid));
      showMessageModal("User deleted successfully.");
      loadUsersForAdmin();
    } catch (error) {
      console.error("Failed to delete user:", error);
      showMessageModal("Failed to delete user from database.");
    } finally {
      state.userToDelete = null;
      dom.confirmDeleteModal.classList.add('hidden');
    }
  }

  // --- MAIN LOGIC - API CALLS ---
  async function callGenerativeAI({ apiKey, provider, systemPrompt, userQuery, images = [], history = [] }) {
    if (provider === 'openai') {
      // OpenAI call logic would go here
      return "OpenAI integration is a placeholder.";
    }
    
    // Default to Gemini
    const model = images.length > 0 || history.some(m => m.images?.length > 0) ? 'gemini-1.5-flash-latest' : 'gemini-1.5-flash-latest';    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;

    const contents = [];
    if (history.length > 0) {      history.forEach(msg => {
        const parts = [{ text: msg.text }];
        if (msg.images) {
          msg.images.forEach(img => parts.push({ inline_data: { mime_type: img.mimeType, data: img.base64 }}));
        }
        contents.push({ role: msg.role === 'user' ? 'user' : 'model', parts });
      });
    }
    
    const userParts = [{ text: userQuery }];
    images.forEach(img => userParts.push({ inline_data: { mime_type: img.mimeType, data: img.base64 }}));    contents.push({ role: 'user', parts: userParts });

    const payload = { contents, systemInstruction: { parts: [{ text: systemPrompt }] } };

    try {
      const response = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error?.message || `HTTP error! status: ${response.status}`);
      }

      const result = await response.json();
      if (result.candidates?.[0]?.content?.parts?.[0]?.text) {
        return result.candidates[0].content.parts[0].text;
      } else {
        return "The model did not provide a text response. It might have been blocked.";
      }
    } catch (error) {
      console.error('AI API Call Error:', error);
      throw error;
    }
  }

  async function handleAnalysisRequest(type) {
    if (!state.isApproved) return;
    const query = dom.inputQuery.value.trim();
    const geminiKey = dom.apiKeyInput.value.trim();

    if (!query && state.analysisImages.length === 0) {
      showMessageModal(state.currentLanguage === 'ar' ? 'الرجاء إدخال استعلام أو تحميل صورة.' : 'Please enter a query or upload an image.');
      return;
    }
    if (!geminiKey) {
      showMessageModal(state.currentLanguage === 'ar' ? 'الرجاء إدخال مفتاح Gemini API.' : 'Please enter a Gemini API key.');
      return;
    }

    dom.loadingSpinner.classList.remove('hidden');
    dom.outputText.innerHTML = '';
    setStatusBusy('analysis', true);

    try {
      const tradingStyle = document.getElementById('trading-style-select').value;
      const school = document.getElementById('school-select').value;
      const systemPrompt = `You are an expert financial analyst. Your task is to provide a ${type}. Your analysis should be based on the ${school} school of thought, tailored for a ${tradingStyle} trading style. Provide clear, actionable insights in well-structured Markdown format.`;
      
      const resultText = await callGenerativeAI({
        apiKey: geminiKey,
        provider: 'gemini',
        systemPrompt,
        userQuery: query || "Analyze the attached image(s).",
        images: state.analysisImages
      });      dom.outputText.innerHTML = mdToHTML(resultText);
    } catch (error) {
      dom.outputText.innerHTML = `<p class="text-red-400">An error occurred: ${error.message}</p>`;
    } finally {
      dom.loadingSpinner.classList.add('hidden');
      setStatusBusy('analysis', false);
      state.analysisImages = [];
      updateImagePreviews('analysis');
    }  }

  async function handleChatSend() {
    if (!state.isApproved) return;
    const message = dom.chatInput.value.trim();
    if (!message && state.chatImages.length === 0) return;

    const geminiKey = dom.apiKeyInput.value.trim();
    if (!geminiKey) {
      showMessageModal(state.currentLanguage === 'ar' ? 'الرجاء إدخال مفتاح Gemini API أولاً.' : 'Please enter a Gemini API key first.');
      return;
    }

    const userMessage = { role: 'user', text: message, images: [...state.chatImages] };
    state.chatHistory.push(userMessage);
    appendChatMessage('user', message, state.chatImages);

    dom.chatInput.value = '';
    state.chatImages = [];
    updateImagePreviews('chat');
    dom.chatLoading.classList.remove('hidden');
    dom.chatSendBtn.disabled = true;
    setStatusBusy('chat', true);

    try {
      const systemPrompt = "You are a helpful financial assistant. Continue the conversation naturally and answer any questions.";
      const resultText = await callGenerativeAI({
        apiKey: geminiKey,
        provider: 'gemini',
        systemPrompt,
        userQuery: message,
        images: userMessage.images,
        history: state.chatHistory.slice(0, -1) // Send history without the current message
      });
      state.chatHistory.push({ role: 'assistant', text: resultText });
      appendChatMessage('assistant', resultText);
    } catch (error) {
      appendChatMessage('assistant', `Error: ${error.message}`);
    } finally {
      dom.chatLoading.classList.add('hidden');
      dom.chatSendBtn.disabled = false;
      setStatusBusy('chat', false);
    }
  }

  function appendChatMessage(role, text, images = []) {
    const row = document.createElement('div');
    row.className = `chat-row ${role}`;
    let imagesHTML = '';
    if (images.length > 0) {
      imagesHTML += '<div class="chat-images-container">';
      images.forEach(img => {
        imagesHTML += `<img src="${URL.createObjectURL(img.file)}" class="chat-image-thumb" alt="User upload">`;
      });
      imagesHTML += '</div>';    }
    const bubble = document.createElement('div');
    bubble.className = `chat-bubble chat-bubble-${role}`;
    bubble.innerHTML = mdToHTML(text) + imagesHTML;
    row.appendChild(bubble);
    dom.chatMessages.appendChild(row);
    dom.chatMessages.scrollTop = dom.chatMessages.scrollHeight;
  }

  // --- EVENT LISTENERS ---
  function setupEventListeners() {
    dom.langArBtn.addEventListener('click', () => setLanguage('ar'));
    dom.langEnBtn.addEventListener('click', () => setLanguage('en'));
    dom.proceedToLoginBtn.addEventListener('click', () => showScreen('login'));
    dom.loginForm.addEventListener('submit', handleLogin);
    dom.logoutBtn.addEventListener('click', handleLogout);
    dom.closeModalBtn.addEventListener('click', hideMessageModal);
    
    dom.uploadBtn.addEventListener('click', () => dom.imageUpload.click());
    dom.chatUploadBtn.addEventListener('click', () => dom.chatImageUpload.click());
    dom.imageUpload.addEventListener('change', (e) => handleImageUpload(e, 'analysis'));
    dom.chatImageUpload.addEventListener('change', (e) => handleImageUpload(e, 'chat'));
    
    dom.highConvictionTradeBtn.addEventListener('click', () => dom.invincibleModal.classList.remove('hidden'));
    dom.cancelInvincibleBtn.addEventListener('click', () => dom.invincibleModal.classList.add('hidden'));
    dom.confirmInvincibleBtn.addEventListener('click', () => {
      dom.invincibleModal.classList.add('hidden');
      handleAnalysisRequest('High-Conviction Trade');
    });
    
    dom.analyzeBtn.addEventListener('click', () => handleAnalysisRequest('Technical Analysis'));
    dom.fundamentalBtn.addEventListener('click', () => handleAnalysisRequest('Fundamental Analysis'));
    dom.directTradeBtn.addEventListener('click', () => handleAnalysisRequest('Trade Recommendation'));
    
    dom.chatSendBtn.addEventListener('click', handleChatSend);
    dom.chatInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        handleChatSend();
      }
    });
    dom.chatClearBtn.addEventListener('click', () => {
      state.chatHistory = [];
      dom.chatMessages.innerHTML = '';
    });
    
    document.body.addEventListener('click', (e) => {
      if (e.target.matches('.remove-image-btn')) handleImageRemoval(e);
      if (e.target.matches('.delete-user-btn')) confirmUserDeletion(e.target.dataset.uid);
    });
    document.body.addEventListener('change', (e) => {
      if (e.target.matches('.approve-toggle')) handleUserApproval(e.target.dataset.uid, e.target.checked);
    });
    
    dom.confirmDeleteBtn.addEventListener('click', deleteUser);
    dom.cancelDeleteBtn.addEventListener('click', () => {
      state.userToDelete = null;
      dom.confirmDeleteModal.classList.add('hidden');
    });
  }

  async function handleImageUpload(event, type) {
    for (const file of event.target.files) {
      const base64 = await fileToBase64(file);
      const imageArray = type === 'analysis' ? state.analysisImages : state.chatImages;
      imageArray.push({ file, base64, mimeType: file.type });
    }
    updateImagePreviews(type);
  }
  
  function handleImageRemoval(event) {
    const index = parseInt(event.target.dataset.index, 10);
    const type = event.target.closest('#image-preview-container') ? 'analysis' : 'chat';    if (type === 'analysis') {
      state.analysisImages.splice(index, 1);      updateImagePreviews('analysis');
    } else {
      state.chatImages.splice(index, 1);      updateImagePreviews('chat');
    }
  }

  // --- INITIALIZATION ---
  function init() {
    const savedLang = localStorage.getItem('jo-ai-lang') || 'ar';
    setLanguage(savedLang);
    setupEventListeners();
  }

  init();
</script>
</body>
