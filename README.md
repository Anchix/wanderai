# wanderai
AI Travel Itinerary Planner — A free, lightweight web app that generates personalized travel itineraries. Users enter their destination, origin, budget, trip duration, and travel preferences. The AI then creates a day-by-day itinerary with places to visit, food recommendations, and travel tips.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>WanderAI — Your Smart Travel Planner</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg: #0a0a0f;
      --surface: #12121a;
      --card: #1a1a26;
      --border: #2a2a3a;
      --accent: #f5a623;
      --accent2: #e84393;
      --text: #f0f0f0;
      --muted: #888899;
      --success: #4ade80;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* BG stars */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        radial-gradient(1px 1px at 10% 20%, rgba(245,166,35,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 80% 10%, rgba(232,67,147,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 50% 80%, rgba(245,166,35,0.2) 0%, transparent 100%),
        radial-gradient(1px 1px at 90% 60%, rgba(255,255,255,0.15) 0%, transparent 100%),
        radial-gradient(1px 1px at 30% 50%, rgba(255,255,255,0.1) 0%, transparent 100%);
      pointer-events: none;
      z-index: 0;
    }

    header {
      position: relative;
      z-index: 1;
      padding: 2rem 2rem 0;
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    .logo {
      font-family: 'Playfair Display', serif;
      font-size: 1.6rem;
      font-weight: 900;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .logo-dot {
      width: 8px; height: 8px;
      background: var(--accent);
      border-radius: 50%;
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); opacity: 1; }
      50% { transform: scale(1.5); opacity: 0.6; }
    }

    /* HERO */
    .hero {
      position: relative;
      z-index: 1;
      text-align: center;
      padding: 3rem 1.5rem 2rem;
    }

    .hero h1 {
      font-family: 'Playfair Display', serif;
      font-size: clamp(2.2rem, 6vw, 4rem);
      font-weight: 900;
      line-height: 1.15;
      margin-bottom: 1rem;
    }

    .hero h1 span {
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .hero p {
      color: var(--muted);
      font-size: 1.05rem;
      max-width: 480px;
      margin: 0 auto 2.5rem;
      line-height: 1.7;
    }

    /* MAIN CARD */
    .main-card {
      position: relative;
      z-index: 1;
      max-width: 680px;
      margin: 0 auto 4rem;
      padding: 0 1.5rem;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 2.5rem;
      box-shadow: 0 20px 60px rgba(0,0,0,0.5);
    }

    /* DESTINATION INPUT */
    .dest-wrap {
      position: relative;
      margin-bottom: 1.5rem;
    }

    .dest-wrap label {
      display: block;
      font-size: 0.78rem;
      font-weight: 500;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 0.6rem;
    }

    .dest-input-row {
      display: flex;
      gap: 0.75rem;
    }

    input[type="text"], input[type="number"], select {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-size: 1rem;
      padding: 0.85rem 1.2rem;
      width: 100%;
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    input[type="text"]:focus, input[type="number"]:focus, select:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(245,166,35,0.12);
    }

    select option { background: var(--surface); }

    .btn-primary {
      background: linear-gradient(135deg, var(--accent), #e8821a);
      border: none;
      border-radius: 12px;
      color: #0a0a0f;
      cursor: pointer;
      font-family: 'DM Sans', sans-serif;
      font-size: 1rem;
      font-weight: 700;
      padding: 0.85rem 1.8rem;
      transition: transform 0.15s, box-shadow 0.15s;
      white-space: nowrap;
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 25px rgba(245,166,35,0.4);
    }

    .btn-primary:active { transform: translateY(0); }

    /* QUESTIONS SECTION */
    #questions-section { display: none; }
    #questions-section.visible { display: block; }

    .divider {
      height: 1px;
      background: var(--border);
      margin: 2rem 0;
    }

    .questions-title {
      font-family: 'Playfair Display', serif;
      font-size: 1.2rem;
      margin-bottom: 1.5rem;
      color: var(--text);
    }

    .q-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.2rem;
      margin-bottom: 1.5rem;
    }

    @media (max-width: 520px) {
      .q-grid { grid-template-columns: 1fr; }
      .card { padding: 1.5rem; }
    }

    .q-field label {
      display: block;
      font-size: 0.78rem;
      font-weight: 500;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--muted);
      margin-bottom: 0.5rem;
    }

    .q-field.full { grid-column: 1 / -1; }

    .radio-group {
      display: flex;
      gap: 0.75rem;
      flex-wrap: wrap;
    }

    .radio-btn {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      color: var(--muted);
      cursor: pointer;
      font-family: 'DM Sans', sans-serif;
      font-size: 0.88rem;
      padding: 0.5rem 1rem;
      transition: all 0.15s;
      user-select: none;
    }

    .radio-btn.selected {
      background: rgba(245,166,35,0.15);
      border-color: var(--accent);
      color: var(--accent);
    }

    .btn-generate {
      background: linear-gradient(135deg, var(--accent2), #c0155a);
      border: none;
      border-radius: 12px;
      color: #fff;
      cursor: pointer;
      font-family: 'DM Sans', sans-serif;
      font-size: 1.05rem;
      font-weight: 700;
      padding: 1rem 2rem;
      width: 100%;
      transition: transform 0.15s, box-shadow 0.15s;
      margin-top: 0.5rem;
    }

    .btn-generate:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 30px rgba(232,67,147,0.4);
    }

    /* LOADING */
    #loading { display: none; text-align: center; padding: 2rem 0; }
    #loading.visible { display: block; }

    .spinner {
      width: 44px; height: 44px;
      border: 3px solid var(--border);
      border-top-color: var(--accent);
      border-radius: 50%;
      margin: 0 auto 1rem;
      animation: spin 0.8s linear infinite;
    }

    @keyframes spin { to { transform: rotate(360deg); } }

    .loading-text {
      color: var(--muted);
      font-size: 0.95rem;
    }

    .loading-text span {
      color: var(--accent);
      font-weight: 500;
    }

    /* RESULT */
    #result-section { display: none; }
    #result-section.visible { display: block; }

    .result-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.5rem;
      flex-wrap: wrap;
      gap: 1rem;
    }

    .result-header h2 {
      font-family: 'Playfair Display', serif;
      font-size: 1.4rem;
    }

    .result-header h2 span { color: var(--accent); }

    .btn-reset {
      background: transparent;
      border: 1px solid var(--border);
      border-radius: 10px;
      color: var(--muted);
      cursor: pointer;
      font-family: 'DM Sans', sans-serif;
      font-size: 0.85rem;
      padding: 0.5rem 1.1rem;
      transition: border-color 0.2s, color 0.2s;
    }

    .btn-reset:hover { border-color: var(--accent); color: var(--accent); }

    #itinerary-output {
      color: var(--text);
      font-size: 0.97rem;
      line-height: 1.8;
      white-space: pre-wrap;
    }

    #itinerary-output h2, #itinerary-output h3 {
      font-family: 'Playfair Display', serif;
      color: var(--accent);
      margin: 1.2rem 0 0.5rem;
    }

    #itinerary-output strong { color: var(--accent2); }

    #itinerary-output ul, #itinerary-output ol {
      padding-left: 1.5rem;
      margin: 0.4rem 0;
    }

    /* VISA BADGE */
    .visa-badge {
      display: inline-flex;
      align-items: center;
      gap: 0.4rem;
      background: rgba(74,222,128,0.12);
      border: 1px solid rgba(74,222,128,0.3);
      border-radius: 20px;
      color: var(--success);
      font-size: 0.82rem;
      font-weight: 500;
      padding: 0.3rem 0.9rem;
      margin-bottom: 1.2rem;
    }

    /* ERROR */
    .error-msg {
      background: rgba(232,67,147,0.1);
      border: 1px solid rgba(232,67,147,0.3);
      border-radius: 10px;
      color: #f87171;
      font-size: 0.9rem;
      padding: 1rem;
      margin-top: 1rem;
      display: none;
    }

    .error-msg.visible { display: block; }

    /* FOOTER */
    footer {
      position: relative;
      z-index: 1;
      text-align: center;
      color: var(--muted);
      font-size: 0.8rem;
      padding: 1.5rem;
    }

    footer a { color: var(--accent); text-decoration: none; }
  </style>
</head>
<body>

<header>
  <div class="logo-dot"></div>
  <div class="logo">WanderAI</div>
</header>

<div class="hero">
  <h1>Plan your dream trip<br/>with <span>AI magic ✈️</span></h1>
  <p>Tell us where you want to go. Answer a few quick questions. Get a personalized itinerary in seconds.</p>
</div>

<div class="main-card">
  <div class="card">

    <!-- STEP 1: Destination -->
    <div id="step1">
      <div class="dest-wrap">
        <label>🌍 Where do you want to go?</label>
        <div class="dest-input-row">
          <input type="text" id="destination" placeholder="e.g. Paris, Bali, Tokyo..." />
          <button class="btn-primary" onclick="showQuestions()">Next →</button>
        </div>
      </div>
      <div class="error-msg" id="dest-error">Please enter a destination first!</div>
    </div>

    <!-- STEP 2: Questions -->
    <div id="questions-section">
      <div class="divider"></div>
      <p class="questions-title">✏️ A few quick details…</p>

      <div class="q-grid">
        <div class="q-field">
          <label>🏠 Traveling from (City/Country)</label>
          <input type="text" id="origin" placeholder="e.g. Mumbai, India" />
        </div>

        <div class="q-field">
          <label>📅 How many days?</label>
          <input type="number" id="days" placeholder="e.g. 7" min="1" max="60" />
        </div>

        <div class="q-field">
          <label>💰 Budget (approx. in USD)</label>
          <input type="number" id="budget" placeholder="e.g. 2000" min="100" />
        </div>

        <div class="q-field">
          <label>👥 Number of travelers</label>
          <input type="number" id="travelers" placeholder="e.g. 2" min="1" max="50" />
        </div>

        <div class="q-field full">
          <label>🧳 Trip type</label>
          <div class="radio-group" id="triptype">
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Adventure</div>
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Relaxation</div>
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Culture & History</div>
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Food & Nightlife</div>
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Family</div>
            <div class="radio-btn" onclick="selectRadio('triptype', this)">Honeymoon</div>
          </div>
        </div>

        <div class="q-field full">
          <label>🛂 Do you need visa information?</label>
          <div class="radio-group" id="visainfo">
            <div class="radio-btn" onclick="selectRadio('visainfo', this)">Yes, include visa tips</div>
            <div class="radio-btn" onclick="selectRadio('visainfo', this)">No, skip it</div>
          </div>
        </div>
      </div>

      <div class="error-msg" id="form-error">Please fill in destination, origin, days, and budget.</div>

      <button class="btn-generate" onclick="generateItinerary()">🗺️ Generate My Itinerary</button>
    </div>

    <!-- LOADING -->
    <div id="loading">
      <div class="divider"></div>
      <div class="spinner"></div>
      <p class="loading-text">Crafting your perfect trip to <span id="loading-dest"></span>…</p>
    </div>

    <!-- RESULT -->
    <div id="result-section">
      <div class="divider"></div>
      <div class="result-header">
        <h2>Your Itinerary for <span id="result-dest"></span></h2>
        <button class="btn-reset" onclick="resetAll()">← Start Over</button>
      </div>
      <div id="itinerary-output"></div>
    </div>

  </div>
</div>

<footer>
  Built with ❤️ using <a href="https://anthropic.com" target="_blank">Claude AI</a> by Anthropic &nbsp;·&nbsp; Free to use
</footer>

<script>
  // ---- Radio button selection ----
  function selectRadio(groupId, el) {
    document.querySelectorAll(`#${groupId} .radio-btn`).forEach(b => b.classList.remove('selected'));
    el.classList.add('selected');
  }

  function getSelected(groupId) {
    const sel = document.querySelector(`#${groupId} .radio-btn.selected`);
    return sel ? sel.textContent.trim() : null;
  }

  // ---- Show questions after destination ----
  function showQuestions() {
    const dest = document.getElementById('destination').value.trim();
    const err = document.getElementById('dest-error');
    if (!dest) { err.classList.add('visible'); return; }
    err.classList.remove('visible');
    document.getElementById('questions-section').classList.add('visible');
    document.getElementById('questions-section').scrollIntoView({ behavior: 'smooth', block: 'start' });
  }

  // ---- Main generate function ----
  async function generateItinerary() {
    const destination = document.getElementById('destination').value.trim();
    const origin      = document.getElementById('origin').value.trim();
    const days        = document.getElementById('days').value.trim();
    const budget      = document.getElementById('budget').value.trim();
    const travelers   = document.getElementById('travelers').value.trim() || '1';
    const tripType    = getSelected('triptype') || 'General';
    const visaInfo    = getSelected('visainfo') || 'No, skip it';
    const formErr     = document.getElementById('form-error');

    if (!destination || !origin || !days || !budget) {
      formErr.classList.add('visible');
      return;
    }
    formErr.classList.remove('visible');

    // Show loading
    document.getElementById('questions-section').style.display = 'none';
    document.getElementById('step1').style.display = 'none';
    document.getElementById('loading').classList.add('visible');
    document.getElementById('loading-dest').textContent = destination;
    document.getElementById('result-section').classList.remove('visible');

    const includeVisa = visaInfo.includes('Yes');

    const prompt = `You are an expert travel planner. Create a detailed, day-by-day travel itinerary based on the following:

- Destination: ${destination}
- Traveling from: ${origin}
- Duration: ${days} days
- Budget: approximately $${budget} USD total for ${travelers} traveler(s)
- Trip type/vibe: ${tripType}
- Include visa information: ${includeVisa ? 'Yes' : 'No'}

Please structure the itinerary as follows:
1. A short exciting intro (2-3 sentences) about the destination
2. ${includeVisa ? 'Visa & Entry Requirements section (brief, practical)' : ''}
3. Budget Breakdown (accommodation, food, transport, activities - rough estimates)
4. Day-by-day itinerary for all ${days} days (each day: morning, afternoon, evening with specific place names and tips)
5. Top 3 local food must-tries
6. 3 practical travel tips for someone coming from ${origin}

Keep it friendly, specific, and genuinely useful. Use emojis sparingly to make it readable.`;

    try {
      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          messages: [{ role: "user", content: prompt }]
        })
      });

      const data = await response.json();
      const text = data.content?.map(b => b.text || '').join('') || "Sorry, couldn't generate itinerary. Please try again.";

      // Render
      document.getElementById('loading').classList.remove('visible');
      document.getElementById('result-dest').textContent = destination;
      document.getElementById('itinerary-output').innerHTML = formatItinerary(text);
      document.getElementById('result-section').classList.add('visible');
      document.getElementById('result-section').scrollIntoView({ behavior: 'smooth', block: 'start' });

    } catch (err) {
      document.getElementById('loading').classList.remove('visible');
      document.getElementById('questions-section').style.display = 'block';
      document.getElementById('step1').style.display = 'block';
      formErr.textContent = "Something went wrong. Please check your API key or try again.";
      formErr.classList.add('visible');
    }
  }

  // ---- Format the AI response ----
  function formatItinerary(text) {
    return text
      .replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')
      .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
      .replace(/^#{1,2} (.+)$/gm, '<h2>$1</h2>')
      .replace(/^#{3,4} (.+)$/gm, '<h3>$1</h3>')
      .replace(/\n/g, '<br/>');
  }

  // ---- Reset ----
  function resetAll() {
    document.getElementById('destination').value = '';
    document.getElementById('origin').value = '';
    document.getElementById('days').value = '';
    document.getElementById('budget').value = '';
    document.getElementById('travelers').value = '';
    document.querySelectorAll('.radio-btn').forEach(b => b.classList.remove('selected'));
    document.getElementById('questions-section').classList.remove('visible');
    document.getElementById('questions-section').style.display = '';
    document.getElementById('step1').style.display = '';
    document.getElementById('result-section').classList.remove('visible');
    document.getElementById('loading').classList.remove('visible');
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  // Also allow Enter key on destination input
  document.getElementById('destination').addEventListener('keydown', e => {
    if (e.key === 'Enter') showQuestions();
  });
</script>
</body>
</html>
