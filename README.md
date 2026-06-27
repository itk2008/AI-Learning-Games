<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
<title>Court Breaker: The Hoop Shrine</title>
<style>
  :root{
    --stone-dark:#1d1810;
    --stone:#2b2319;
    --stone-light:#3d3424;
    --gold:#e0a830;
    --gold-bright:#f4c44a;
    --court-orange:#e8772e;
    --net-teal:#3fb8af;
    --paper:#f3ead2;
    --danger:#d9534f;
    --success:#4caf7d;
    --shadow:rgba(0,0,0,.45);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    font-family:'Trebuchet MS','Segoe UI',sans-serif;
    background:radial-gradient(circle at 50% 0%, #3a2f1c 0%, #15110a 70%);
    color:var(--paper);
    min-height:100vh;
    display:flex;
    align-items:flex-start;
    justify-content:center;
    padding:24px 12px 60px;
  }
  #app{
    width:100%;
    max-width:880px;
  }
  h1,h2,h3{
    font-family:'Georgia',serif;
    letter-spacing:.5px;
    margin:0 0 8px;
  }
  .panel{
    background:linear-gradient(180deg,var(--stone) 0%, var(--stone-dark) 100%);
    border:3px solid var(--gold);
    border-radius:16px;
    padding:28px 26px;
    box-shadow:0 10px 30px var(--shadow), inset 0 0 40px rgba(0,0,0,.3);
    position:relative;
    overflow:hidden;
  }
  .panel::before{
    content:"";
    position:absolute; inset:0;
    background:
      repeating-linear-gradient(45deg, rgba(255,255,255,.015) 0 2px, transparent 2px 6px);
    pointer-events:none;
  }
  .glow-title{
    color:var(--gold-bright);
    text-shadow:0 0 14px rgba(244,196,74,.5);
    font-size:1.9rem;
  }
  .subtitle{color:#c9bd9f; font-size:.95rem; margin-bottom:18px;}
  .btn{
    background:linear-gradient(180deg,var(--court-orange),#b85620);
    color:#fff;
    border:none;
    border-radius:10px;
    padding:13px 22px;
    font-size:1rem;
    font-weight:700;
    cursor:pointer;
    box-shadow:0 4px 0 #7a3a14, 0 6px 14px rgba(0,0,0,.4);
    transition:transform .08s ease;
  }
  .btn:hover{transform:translateY(-2px);}
  .btn:active{transform:translateY(1px); box-shadow:0 2px 0 #7a3a14;}
  .btn.secondary{
    background:linear-gradient(180deg,#4a4030,#312a1d);
    box-shadow:0 4px 0 #1f1a11, 0 6px 14px rgba(0,0,0,.4);
  }
  .btn.teal{
    background:linear-gradient(180deg,var(--net-teal),#27827b);
    box-shadow:0 4px 0 #1a5b56, 0 6px 14px rgba(0,0,0,.4);
  }
  .btn:disabled{opacity:.4; cursor:not-allowed; box-shadow:none; transform:none;}
  .row{display:flex; gap:12px; flex-wrap:wrap; align-items:center;}
  .map-grid{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:14px;
    margin:22px 0;
  }
  .shrine-card{
    background:var(--stone-light);
    border:2px solid #5a4d33;
    border-radius:12px;
    padding:14px 10px;
    text-align:center;
    position:relative;
    transition:.15s;
  }
  .shrine-card.locked{opacity:.45;}
  .shrine-card.active{border-color:var(--gold-bright); box-shadow:0 0 18px rgba(244,196,74,.4);}
  .shrine-card.done{border-color:var(--success);}
  .shrine-card svg{width:54px; height:54px; margin-bottom:6px;}
  .shrine-card .label{font-size:.78rem; color:#d9cba8; font-weight:600;}
  .badge{
    display:inline-block;
    font-size:.7rem;
    padding:2px 8px;
    border-radius:20px;
    background:var(--gold);
    color:#2b2319;
    font-weight:700;
    margin-top:6px;
  }
  .status-row{display:flex; justify-content:space-between; align-items:center; margin-bottom:14px; flex-wrap:wrap; gap:8px;}
  .pill{
    background:rgba(255,255,255,.08);
    border:1px solid rgba(255,255,255,.15);
    border-radius:20px;
    padding:5px 14px;
    font-size:.82rem;
  }
  .question-box{
    background:#fffaf0;
    color:#241c10;
    border-radius:12px;
    padding:22px;
    margin:18px 0;
    font-size:1.08rem;
    line-height:1.55;
    box-shadow:inset 0 0 0 2px #e3d4a8;
  }
  .question-box .qmeta{font-size:.78rem; color:#8a6e2f; font-weight:700; text-transform:uppercase; letter-spacing:.6px; margin-bottom:8px;}
  .answers{display:flex; flex-direction:column; gap:10px; margin:14px 0;}
  .answer-btn{
    background:#2c2517;
    border:2px solid #5a4d33;
    color:var(--paper);
    border-radius:10px;
    padding:13px 16px;
    text-align:left;
    cursor:pointer;
    font-size:.98rem;
    transition:.12s;
  }
  .answer-btn:hover{border-color:var(--gold-bright); background:#3a3120;}
  .answer-btn.correct{background:#1f3d2c; border-color:var(--success);}
  .answer-btn.wrong{background:#3d2020; border-color:var(--danger);}
  .answer-btn:disabled{cursor:default;}
  input.text-answer{
    width:160px;
    padding:11px 14px;
    border-radius:8px;
    border:2px solid #5a4d33;
    background:#fffaf0;
    color:#241c10;
    font-size:1rem;
    margin-right:10px;
  }
  .hint-area{
    margin-top:14px;
    border-top:1px dashed #5a4d33;
    padding-top:14px;
  }
  .hint-box{
    background:rgba(63,184,175,.12);
    border:1px solid var(--net-teal);
    border-radius:10px;
    padding:12px 14px;
    margin-top:10px;
    font-size:.92rem;
    color:#d7f3f0;
  }
  .feedback{
    margin-top:14px;
    padding:14px 16px;
    border-radius:10px;
    font-size:.95rem;
  }
  .feedback.good{background:rgba(76,175,125,.18); border:1px solid var(--success);}
  .feedback.bad{background:rgba(217,83,79,.15); border:1px solid var(--danger);}
  .progress-bar{
    height:10px;
    background:#1a1510;
    border-radius:6px;
    overflow:hidden;
    margin:10px 0 18px;
    border:1px solid #5a4d33;
  }
  .progress-fill{
    height:100%;
    background:linear-gradient(90deg,var(--court-orange),var(--gold-bright));
    transition:width .3s ease;
  }
  .timer-wrap{display:flex; align-items:center; gap:10px; margin-bottom:14px;}
  .timer-bar{flex:1; height:14px; background:#1a1510; border-radius:8px; border:1px solid #5a4d33; overflow:hidden;}
  .timer-fill{height:100%; background:linear-gradient(90deg,var(--net-teal),var(--gold-bright)); transition:width .25s linear;}
  ul.tight{margin:6px 0 0 18px; padding:0;}
  ul.tight li{margin-bottom:6px;}
  .grid2{display:grid; grid-template-columns:1fr 1fr; gap:18px;}
  @media(max-width:640px){
    .map-grid{grid-template-columns:repeat(2,1fr);}
    .grid2{grid-template-columns:1fr;}
    body{font-size:18px;}
    .panel{padding:22px 18px;}
    .glow-title{font-size:1.6rem;}
    .subtitle{font-size:1.05rem; line-height:1.5;}
    .question-box{font-size:1.15rem; line-height:1.6; padding:18px;}
    .question-box .qmeta{font-size:.85rem;}
    .answer-btn{font-size:1.08rem; padding:15px 16px;}
    .btn{font-size:1.05rem; padding:15px 20px;}
    .pill{font-size:.92rem; padding:6px 14px;}
    .hint-box{font-size:1.02rem; padding:14px 16px;}
    .feedback{font-size:1.05rem; padding:16px;}
    .small-note{font-size:.9rem;}
    .report-card{font-size:1rem;}
    .shrine-card .label{font-size:.88rem;}
    .badge{font-size:.78rem;}
    input.text-answer{font-size:1.1rem; width:100%; margin-bottom:10px;}
  }
  .report-card{
    background:var(--stone-light);
    border:2px solid #5a4d33;
    border-radius:12px;
    padding:16px 18px;
    margin-bottom:14px;
  }
  .bar-track{height:16px; background:#1a1510; border-radius:8px; overflow:hidden; border:1px solid #5a4d33; margin-top:6px;}
  .bar-fill{height:100%;}
  .fade-in{animation:fadeIn .4s ease;}
  @keyframes fadeIn{from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);}}
  .small-note{font-size:.8rem; color:#a99a73; margin-top:4px;}
  .close-x{position:absolute; top:10px; right:14px; cursor:pointer; font-size:1.2rem; color:#c9bd9f; background:none; border:none;}
  .overlay{
    position:fixed; inset:0; background:rgba(0,0,0,.7);
    display:flex; align-items:center; justify-content:center; z-index:50; padding:16px;
  }
  .overlay .panel{max-width:520px;}
  .star-row{font-size:1.4rem; letter-spacing:3px;}
</style>
</head>
<body>
<div id="app"></div>

<script>
/* ============================================================
   COURT BREAKER: THE HOOP SHRINE
   A linear functions SAT practice game
   ============================================================ */

// ---------- ICONS ----------
const ICONS = {
  coords: `<svg viewBox="0 0 64 64"><rect x="2" y="2" width="60" height="60" rx="10" fill="#3d3424" stroke="#e0a830" stroke-width="2"/><line x1="10" y1="50" x2="56" y2="50" stroke="#e0a830" stroke-width="2"/><line x1="14" y1="54" x2="14" y2="10" stroke="#e0a830" stroke-width="2"/><circle cx="22" cy="42" r="3.5" fill="#e8772e"/><circle cx="32" cy="32" r="3.5" fill="#e8772e"/><circle cx="42" cy="22" r="3.5" fill="#e8772e"/><line x1="22" y1="42" x2="42" y2="22" stroke="#3fb8af" stroke-width="2" stroke-dasharray="3 2"/></svg>`,
  word: `<svg viewBox="0 0 64 64"><rect x="2" y="2" width="60" height="60" rx="10" fill="#3d3424" stroke="#e0a830" stroke-width="2"/><circle cx="32" cy="24" r="9" fill="#e8772e"/><path d="M16 50c0-10 7-16 16-16s16 6 16 16" fill="none" stroke="#3fb8af" stroke-width="4" stroke-linecap="round"/></svg>`,
  explain: `<svg viewBox="0 0 64 64"><rect x="2" y="2" width="60" height="60" rx="10" fill="#3d3424" stroke="#e0a830" stroke-width="2"/><circle cx="32" cy="30" r="14" fill="none" stroke="#e8772e" stroke-width="3"/><text x="32" y="36" font-size="20" text-anchor="middle" fill="#f4c44a" font-family="Georgia">?</text><rect x="28" y="48" width="8" height="6" fill="#3fb8af"/></svg>`,
  transform: `<svg viewBox="0 0 64 64"><rect x="2" y="2" width="60" height="60" rx="10" fill="#3d3424" stroke="#e0a830" stroke-width="2"/><path d="M14 44 L28 18 L38 34 L50 14" fill="none" stroke="#e8772e" stroke-width="3"/><path d="M14 50 L28 28 L38 42 L50 24" fill="none" stroke="#3fb8af" stroke-width="3" stroke-dasharray="4 2"/></svg>`,
  camera: `<svg viewBox="0 0 24 24" width="20" height="20"><rect x="2" y="6" width="20" height="14" rx="2" fill="#3fb8af"/><circle cx="12" cy="13" r="5" fill="#1d1810"/><rect x="8" y="3" width="8" height="4" rx="1" fill="#3fb8af"/></svg>`,
  ball: `<svg viewBox="0 0 24 24" width="20" height="20"><circle cx="12" cy="12" r="10" fill="#e8772e"/><path d="M2 12h20M12 2v20M5 5c3 3 3 14 0 14M19 5c-3 3-3 14 0 14" stroke="#1d1810" stroke-width="1.4" fill="none"/></svg>`,
};

// ---------- QUESTION BANKS ----------
// Each shrine: 6-question pool. type: 'mc' (multiple choice) or 'num' (numeric text input)

const SHRINE1 = { // Forming linear function from coordinates / table
  key: 'coords', title: 'Shrine of Coordinates', icon: ICONS.coords,
  intro: `Ancient stone tablets here show pairs of numbers — points on a line. Your job: figure out the rule (the function) that connects them. Find the slope (rate of change) and the starting value (y-intercept), then build the equation y = mx + b.`,
  pool: [
    {
      id:'c1', type:'mc',
      prompt:`A coach records the number of free throws Maya has practiced and her total practice time. The table below shows the data.<br><br>
      <table style="width:100%;border-collapse:collapse;margin-top:8px;">
      <tr style="background:#e3d4a8;"><th style="padding:6px;border:1px solid #c9b888;">Time (minutes), x</th><th style="padding:6px;border:1px solid #c9b888;">Free throws made, y</th></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">0</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">2</td></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">5</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">12</td></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">10</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">22</td></tr>
      </table><br>Which equation correctly models y in terms of x?`,
      choices:[
        'y = 2x + 2',
        'y = 10x + 2',
        'y = 2x + 10',
        'y = 0.5x + 2'
      ],
      correct:0,
      hints:[
        "Slope tells us the rate of change — how much y changes every time x increases by a fixed amount. Look at two rows: as x goes from 0 to 5 (a change of 5), how much does y change?",
        "y went from 2 to 12 — that's a change of +10 over a change of +5 in x. Slope = change in y ÷ change in x = 10 ÷ 5. What's that?",
        "Slope = 10 ÷ 5 = 2. Now look at the row where x = 0 — that y-value IS your b (y-intercept), since y = mx + b becomes y = m(0) + b = b. So b = 2. Plug m and b into y = mx + b."
      ],
      explain:"Slope = (12-2)/(5-0) = 10/5 = 2. When x=0, y=2, so b=2. Equation: y = 2x + 2."
    },
    {
      id:'c2', type:'mc',
      prompt:`A photography club tracks how many photo prints, y, are left in a printer after x prints have been made. Two points on this line are (2, 46) and (6, 30).<br><br>Which equation models this situation?`,
      choices:[
        'y = -4x + 54',
        'y = 4x + 38',
        'y = -8x + 62',
        'y = -4x + 38'
      ],
      correct:0,
      hints:[
        "Find the slope first using the slope formula: m = (y₂ - y₁) / (x₂ - x₁). Use the two points (2,46) and (6,30).",
        "m = (30 - 46) / (6 - 2) = -16 / 4 = -4. The slope is -4 (it's negative because prints are running OUT). Now use y = mx + b and plug in one point — like (2,46) — to solve for b.",
        "46 = -4(2) + b → 46 = -8 + b → b = 54. So the equation is y = -4x + 54."
      ],
      explain:"Slope = (30-46)/(6-2) = -16/4 = -4. Using point (2,46): 46 = -4(2)+b → b=54. Equation: y=-4x+54."
    },
    {
      id:'c3', type:'mc',
      prompt:`The points (0, 8) and (4, 8) both lie on line k. Which statement is true about line k?`,
      choices:[
        'Line k is horizontal with a slope of 0.',
        'Line k is vertical with an undefined slope.',
        'Line k has a slope of 4.',
        'Line k has a slope of 8.'
      ],
      correct:0,
      hints:[
        "Use the slope formula: m = (y₂-y₁)/(x₂-x₁). What do you get when both y-values are the same number?",
        "(8-8)/(4-0) = 0/4. Any number divided into 0 (where the bottom isn't 0) equals... what?",
        "0/4 = 0. A slope of 0 means the line is perfectly flat — horizontal. (A vertical line is the one with undefined slope, which happens when the x-values are the same instead.)"
      ],
      explain:"Slope = (8-8)/(4-0) = 0/4 = 0 → horizontal line."
    },
    {
      id:'c4', type:'num',
      prompt:`A line passes through the points (1, 5) and (3, 13). What is the slope of this line? (Enter just the number.)`,
      answer:'4',
      hints:[
        "Slope formula: m = (y₂ - y₁) / (x₂ - x₁). Plug in (1,5) as point 1 and (3,13) as point 2.",
        "m = (13 - 5) / (3 - 1) = 8 / 2. What does that simplify to?",
        "8 ÷ 2 = 4. The slope is 4."
      ],
      explain:"m = (13-5)/(3-1) = 8/2 = 4."
    },
    {
      id:'c5', type:'mc',
      prompt:`Table of values for function h:<br><br>
      <table style="width:100%;border-collapse:collapse;margin-top:8px;">
      <tr style="background:#e3d4a8;"><th style="padding:6px;border:1px solid #c9b888;">x</th><th style="padding:6px;border:1px solid #c9b888;">h(x)</th></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">-2</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">1</td></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">0</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">7</td></tr>
      <tr><td style="padding:6px;border:1px solid #c9b888;text-align:center;">2</td><td style="padding:6px;border:1px solid #c9b888;text-align:center;">13</td></tr>
      </table><br>What is h(x)?`,
      choices:[
        'h(x) = 3x + 7',
        'h(x) = 7x + 3',
        'h(x) = 6x + 7',
        'h(x) = 3x + 1'
      ],
      correct:0,
      hints:[
        "Pick two rows to find the slope — try x=0 (h=7) and x=2 (h=13). What's the change in h(x) divided by the change in x?",
        "(13-7)/(2-0) = 6/2 = 3. That's your slope. Since x=0 gives h(x)=7 directly, what does that tell you about b?",
        "Since plugging x=0 into h(x)=mx+b just leaves b, and h(0)=7, then b=7. So h(x) = 3x + 7."
      ],
      explain:"Slope = (13-7)/(2-0)=3. h(0)=7=b. h(x)=3x+7."
    },
    {
      id:'c6', type:'mc',
      prompt:`A line contains the points (4, 9) and (4, -2). What can you conclude?`,
      choices:[
        'The line is vertical, so it is NOT a function — its slope is undefined.',
        'The slope of the line is 0.',
        'The slope of the line is 4.',
        'The slope of the line is 11.'
      ],
      correct:0,
      hints:[
        "Try the slope formula: m = (y₂-y₁)/(x₂-x₁). What happens to the denominator when both x-values are the same?",
        "(−2−9)/(4−4) = −11/0. Dividing by zero is undefined in math — it breaks the formula.",
        "When the x-values don't change but y does, you get a vertical line. Vertical lines have undefined slope and actually fail the 'function' test (one x can't map to two different y's)."
      ],
      explain:"m = (-2-9)/(4-4) = -11/0, undefined → vertical line, not a function."
    }
  ]
};

const SHRINE2 = { // Forming linear function from word problem
  key:'word', title:'Shrine of the Storyteller', icon: ICONS.word,
  intro: `These walls are covered in old stories — about ticket prices, court rentals, camera batteries, and team budgets. Hidden inside every story is a starting amount and a rate of change. Translate the words into y = mx + b.`,
  pool:[
    {
      id:'w1', type:'mc',
      prompt:`A gym charges a one-time $15 registration fee plus $8 per basketball clinic session attended. Which function gives the total cost C(s), in dollars, for attending s sessions?`,
      choices:[
        'C(s) = 8s + 15',
        'C(s) = 15s + 8',
        'C(s) = 8s - 15',
        'C(s) = 23s'
      ],
      correct:0,
      hints:[
        "What part of the cost happens no matter what — even if you attend 0 sessions? That's your starting value, b.",
        "The $15 fee is paid once regardless of sessions, so b = 15. The $8 per session is the rate that changes with each session — that's your slope, m, multiplied by s.",
        "Total = (rate × sessions) + flat fee = 8s + 15."
      ],
      explain:"Flat fee ($15) = b. Per-session cost ($8) = m. C(s) = 8s + 15."
    },
    {
      id:'w2', type:'mc',
      prompt:`A photographer starts a shoot with a memory card that already has 40 photos saved on it. She takes 6 new photos per minute. Which function models the total number of photos P(t) on the card after t minutes of shooting?`,
      choices:[
        'P(t) = 6t + 40',
        'P(t) = 40t + 6',
        'P(t) = 46t',
        'P(t) = 6t - 40'
      ],
      correct:0,
      hints:[
        "What's already true at t = 0 minutes, before she takes any new photos? That number is your b.",
        "At t=0, there are already 40 photos — that's b=40. The 6 photos per minute is the rate of change as time passes — that's your slope, multiplied by t.",
        "Total photos = (rate × time) + starting amount = 6t + 40."
      ],
      explain:"Starting photos (40) = b. Rate (6/min) = m. P(t) = 6t + 40."
    },
    {
      id:'w3', type:'mc',
      prompt:`A team's water cooler holds 20 gallons. During a hot practice, players drink water at a rate of 1.5 gallons every 10 minutes. Which function best models the amount of water W(t), in gallons, remaining in the cooler after t minutes?`,
      choices:[
        'W(t) = 20 - 0.15t',
        'W(t) = 20 + 0.15t',
        'W(t) = 1.5t - 20',
        'W(t) = 20 - 1.5t'
      ],
      correct:0,
      hints:[
        "The cooler STARTS at 20 gallons — that's b. As time passes, is the amount of water going up or down? That tells you whether your slope should be positive or negative.",
        "It's going down (being drunk), so the slope is negative. Now: the rate given is '1.5 gallons every 10 minutes' — that's not yet a per-minute rate. You need gallons per ONE minute.",
        "1.5 gallons ÷ 10 minutes = 0.15 gallons per minute. So the slope is -0.15. W(t) = 20 - 0.15t."
      ],
      explain:"Rate = 1.5/10 = 0.15 gal/min, decreasing. W(t) = 20 - 0.15t."
    },
    {
      id:'w4', type:'mc',
      prompt:`A rec center rents out its court. It costs $40 for the first hour, and then an additional $25 for each additional hour after that. Which function models the total rental cost R(h), in dollars, where h is the number of additional hours after the first?`,
      choices:[
        'R(h) = 25h + 40',
        'R(h) = 40h + 25',
        'R(h) = 65h',
        'R(h) = 25h - 40'
      ],
      correct:0,
      hints:[
        "Notice the problem defines h as the additional hours AFTER the first one — so at h=0, you've still paid for that first hour. What's the cost when h=0?",
        "When h=0, you've only paid the flat $40 — that's b=40. Each additional hour after that adds $25 — that's your rate, m, times h.",
        "R(h) = 25h + 40."
      ],
      explain:"At h=0 (no extra hours), cost is the flat $40 = b. Extra hours cost $25 each = m. R(h)=25h+40."
    },
    {
      id:'w5', type:'mc',
      prompt:`A camera's battery is at 85% charge. While shooting in burst mode, the battery drains by 2 percentage points every 3 minutes. Which function models the battery charge B(t), in percent, after t minutes of shooting?`,
      choices:[
        'B(t) = 85 - (2/3)t',
        'B(t) = 85 + (2/3)t',
        'B(t) = 85 - 2t',
        'B(t) = (2/3)t - 85'
      ],
      correct:0,
      hints:[
        "What's the starting charge at t=0? That's b.",
        "b = 85. Now, the rate is '2 percentage points every 3 minutes' — that means you need to convert this to a per-minute rate by dividing.",
        "2 ÷ 3 = 2/3 per minute, and it's decreasing (draining), so slope is negative. B(t) = 85 - (2/3)t."
      ],
      explain:"Rate = 2/3 per min, decreasing. B(t) = 85 - (2/3)t."
    },
    {
      id:'w6', type:'num',
      prompt:`A team fundraiser started with $120 already donated. Supporters then donate at a steady rate, and after 4 days, the total raised was $360. Assuming the rate of donations per day stayed constant, what was the rate of donation, in dollars per day? (Enter just the number.)`,
      answer:'60',
      hints:[
        "You know the starting amount (b = 120) and you know the total after 4 days (360). The rate is the slope — how much changed over those 4 days.",
        "Total change in money = 360 - 120 = 240. That change happened over 4 days. Rate = change in money ÷ change in days.",
        "240 ÷ 4 = 60. The rate is $60 per day."
      ],
      explain:"Change = 360-120 = 240 over 4 days. Rate = 240/4 = 60 dollars/day."
    }
  ]
};

const SHRINE3 = { // Explaining slope/y-intercept/x-intercept in context
  key:'explain', title:'Shrine of Meaning', icon: ICONS.explain,
  intro: `Here, numbers alone won't open the door — you must explain what they MEAN in plain English. Every slope and intercept tells a real-world story. The shrine wants to hear that story.`,
  pool:[
    {
      id:'e1', type:'mc',
      prompt:`The function C(m) = 0.5m + 20 models the cost, in dollars, of renting a basketball machine for m minutes. What does the value 20 represent in this context?`,
      choices:[
        'The flat fee charged before any rental minutes are used',
        'The cost of one minute of use',
        'The total number of minutes available',
        'The maximum amount the rental can cost'
      ],
      correct:0,
      hints:[
        "In y = mx + b form, 20 is playing the role of b — the y-intercept. What does the y-intercept represent in a real-world cost situation? Think about what the cost is when the input (minutes) is 0.",
        "If m (minutes) = 0, then C(0) = 0.5(0) + 20 = 20. So 20 is the cost even before any minutes pass — what kind of fee is that called?",
        "It's the flat/starting fee — the cost you pay no matter what, before usage starts. That's answer 1."
      ],
      explain:"20 is the y-intercept = cost when minutes = 0 = the flat starting fee."
    },
    {
      id:'e2', type:'mc',
      prompt:`The function H(t) = -3t + 45 models the height, in feet, of a drone capturing aerial photos of a game, t seconds after it starts descending. What does the slope, -3, represent?`,
      choices:[
        "The drone's height decreases by 3 feet every second",
        "The drone starts 3 feet off the ground",
        "The drone will land after 3 seconds",
        "The drone's height increases by 3 feet every second"
      ],
      correct:0,
      hints:[
        "The slope always represents a RATE of change — how the output (height) changes per unit of the input (time). Is the slope positive or negative here, and what does that sign suggest about height over time?",
        "The slope is -3, which is negative — meaning height is decreasing as time increases. So for every 1 second that passes, what happens to height?",
        "Height drops by 3 feet for every second that passes — that's what 'slope = -3' means in this real-world context."
      ],
      explain:"Slope -3 means height decreases 3 ft per second."
    },
    {
      id:'e3', type:'mc',
      prompt:`The function P(d) = 12d represents the total points P a player has scored after d games, assuming they score the same number of points every game. Which statement correctly interprets the y-intercept of this function?`,
      choices:[
        "The y-intercept is 0, meaning the player has 0 total points before playing any games",
        "The y-intercept is 12, meaning the player scores 12 points before the season starts",
        "There is no y-intercept in this function",
        "The y-intercept represents the player's average points per game"
      ],
      correct:0,
      hints:[
        "This function is written as P(d) = 12d. Compare it to y = mx + b. What number is playing the role of b here, even though it's not written out?",
        "When there's no number added or subtracted at the end, b is secretly 0. So what is P(0)?",
        "P(0) = 12(0) = 0. The y-intercept is 0, meaning before any games are played, total points scored is 0 — which makes complete sense."
      ],
      explain:"P(d)=12d has b=0 (hidden). P(0)=0 → 0 points before any games."
    },
    {
      id:'e4', type:'mc',
      prompt:`A photography business' weekly profit is modeled by the function R(x) = 45x - 300, where x is the number of photo sessions booked that week. What does the x-intercept of this function represent?`,
      choices:[
        "The number of sessions needed in a week to break even (profit = 0)",
        "The amount of profit made per session",
        "The fixed weekly cost of running the business",
        "The maximum number of sessions that can be booked"
      ],
      correct:0,
      hints:[
        "The x-intercept is the point where the OUTPUT (in this case, profit, R(x)) equals 0. What would profit equal to zero mean for a business in plain English?",
        "If R(x) = 0, the business isn't making a profit or a loss — it's exactly even. What's the term for that situation?",
        "That's called the 'break-even point' — the number of sessions needed so total earnings exactly cover costs, with $0 profit or loss."
      ],
      explain:"x-intercept is where R(x)=0 → profit is exactly zero → break-even point."
    },
    {
      id:'e5', type:'mc',
      prompt:`The function V(y) = 1200 - 80y models the value, in dollars, of a basketball team's used equipment y years after purchase. What does the slope -80 mean in this context?`,
      choices:[
        "The equipment's value decreases by $80 each year",
        "The equipment originally cost $80",
        "The equipment will be worthless after 80 years",
        "The equipment's value increases by $80 each year"
      ],
      correct:0,
      hints:[
        "Slope represents rate of change of the output per unit of input. Here the output is value (dollars) and input is years. Is -80 making the value go up or down as years increase?",
        "Negative slope means the value is decreasing as years pass. So for every additional year, what happens to the dollar value?",
        "Value drops by $80 for each year that passes — depreciation, basically."
      ],
      explain:"Slope -80 = value decreases by $80 per year."
    },
    {
      id:'e6', type:'mc',
      prompt:`A coach uses the function S(p) = 2p + 1 to estimate a player's shooting score, S, based on p hours of weekly practice. A teammate says: "This means every hour of practice gives you 2 extra points on your shooting score, no matter how much you already practice." Is the teammate's interpretation of the slope correct?`,
      choices:[
        "Yes — the slope of 2 means shooting score increases by 2 points for every additional hour of practice",
        "No — the slope means the score starts at 2 points before any practice",
        "No — the slope means a player needs at least 2 hours to see any improvement",
        "Yes — the slope of 2 means the player already has a perfect score"
      ],
      correct:0,
      hints:[
        "Focus only on what the number 2 is doing in S(p) = 2p + 1 — is it being multiplied by p (making it the slope) or standing alone (making it the y-intercept)?",
        "Since 2 is multiplied by p, it's the slope — the rate of change. What does a slope of 2 mean about how output changes per unit of input?",
        "A slope of 2 means: for every 1 additional hour of practice, the shooting score goes up by 2 points. The teammate is correct."
      ],
      explain:"Coefficient of p is the slope (2) = score increases 2 pts per hour of practice."
    }
  ]
};

const SHRINE4 = { // Transformations f(x), g(x)
  key:'transform', title:'Shrine of Reflection', icon: ICONS.transform,
  intro: `The final shrine bends and warps lines like light through a camera lens. Functions f(x) and g(x) are related by a transformation — a shift, stretch, or flip. Substitute carefully and solve.`,
  pool:[
    {
      id:'t1', type:'mc',
      prompt:`If f(x) = 3x + 4 and g(x) = f(x) - 5, what is g(x)?`,
      choices:[
        'g(x) = 3x - 1',
        'g(x) = 3x + 9',
        'g(x) = -2x + 4',
        'g(x) = 3x - 5'
      ],
      correct:0,
      hints:[
        "g(x) = f(x) - 5 means: take the whole expression for f(x), then subtract 5 from it. Write out f(x) first, then subtract 5 from the entire thing.",
        "f(x) = 3x + 4, so g(x) = (3x + 4) - 5. Combine the constant terms: 4 - 5.",
        "4 - 5 = -1. So g(x) = 3x - 1."
      ],
      explain:"g(x) = f(x)-5 = (3x+4)-5 = 3x-1."
    },
    {
      id:'t2', type:'mc',
      prompt:`If f(x) = 2x - 6, what is f(x + 3)?`,
      choices:[
        'f(x+3) = 2x',
        'f(x+3) = 2x - 3',
        'f(x+3) = 2x + 3',
        'f(x+3) = 2x - 9'
      ],
      correct:0,
      hints:[
        "f(x+3) means: everywhere you see an 'x' in the original function, replace it with '(x+3)'. Try rewriting f(x) = 2x - 6 with (x+3) substituted in for x.",
        "f(x+3) = 2(x+3) - 6. Now distribute the 2 across (x+3): 2 times x, and 2 times 3.",
        "2(x+3) = 2x + 6. So f(x+3) = 2x + 6 - 6. Combine the constants: 6 - 6 = 0. f(x+3) = 2x."
      ],
      explain:"f(x+3)=2(x+3)-6 = 2x+6-6 = 2x."
    },
    {
      id:'t3', type:'num',
      prompt:`If f(x) = 4x + 7, what is the value of f(2)? (Enter just the number.)`,
      answer:'15',
      hints:[
        "f(2) means substitute the number 2 in for every x in the function.",
        "f(2) = 4(2) + 7. Solve the multiplication first.",
        "4(2) = 8, then 8 + 7 = 15."
      ],
      explain:"f(2)=4(2)+7=8+7=15."
    },
    {
      id:'t4', type:'mc',
      prompt:`The graph of g(x) is the graph of f(x) shifted up 6 units. If f(x) = -2x + 1, what is g(x)?`,
      choices:[
        'g(x) = -2x + 7',
        'g(x) = -2x - 5',
        'g(x) = 4x + 1',
        'g(x) = -2x + 6'
      ],
      correct:0,
      hints:[
        "Shifting a graph UP by some number of units means adding that number to the whole function. So g(x) = f(x) + 6.",
        "g(x) = (-2x + 1) + 6. Combine the constants: 1 + 6.",
        "1 + 6 = 7. So g(x) = -2x + 7. Notice the slope (-2) doesn't change — only the y-intercept shifts when you move a graph up or down."
      ],
      explain:"Shift up 6 means g(x)=f(x)+6 = (-2x+1)+6 = -2x+7."
    },
    {
      id:'t5', type:'mc',
      prompt:`If f(x) = 5x - 2 and g(x) = -f(x), what is g(x)?`,
      choices:[
        'g(x) = -5x + 2',
        'g(x) = 5x + 2',
        'g(x) = -5x - 2',
        'g(x) = 2 - 5x + 5x'
      ],
      correct:0,
      hints:[
        "g(x) = -f(x) means take the ENTIRE expression for f(x) and multiply it by -1 (flip every sign).",
        "f(x) = 5x - 2, so g(x) = -(5x - 2). Distribute the negative sign across both terms inside the parentheses.",
        "-(5x) = -5x, and -(-2) = +2. So g(x) = -5x + 2."
      ],
      explain:"g(x) = -f(x) = -(5x-2) = -5x+2."
    },
    {
      id:'t6', type:'num',
      prompt:`If f(x) = 3x - 4 and g(x) = f(x) + 2x, what is the slope of g(x)? (Enter just the number.)`,
      answer:'5',
      hints:[
        "First write out g(x) fully: g(x) = f(x) + 2x = (3x - 4) + 2x. Combine the x-terms together.",
        "3x + 2x = 5x. So g(x) = 5x - 4.",
        "In y = mx + b form, the slope is the number multiplied by x. Here that's 5."
      ],
      explain:"g(x) = (3x-4)+2x = 5x-4. Slope = 5."
    }
  ]
};

const SHRINES = [SHRINE1, SHRINE2, SHRINE3, SHRINE4];
const QUESTIONS_PER_RUN = 4;

// ---------- STATE ----------
let state = {
  screen: 'title', // title, overview, map, shrine_intro, quiz, bonus, shrine_result, final
  currentShrineIdx: 0,
  unlocked: [true, false, false, false],
  completed: [false, false, false, false],
  shrineStats: SHRINES.map(()=>({
    firstTry:0, hintTier1:0, hintTier2:0, hintTier3:0, wrong:0, totalAsked:0, bonusCorrect:0, bonusAttempted:0
  })),
  runQuestions: [],
  qIndex: 0,
  hintTier: 0,
  answered: false,
  bonusMode:false,
  bonusQueue:[],
  bonusIdx:0,
  bonusCorrectCount:0,
  bonusResults:[]
};

function shuffle(arr){
  const a = arr.slice();
  for(let i=a.length-1;i>0;i--){
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
  return a;
}

function pickRun(pool){
  return shuffle(pool).slice(0, QUESTIONS_PER_RUN);
}

// ---------- RENDER ----------
const app = document.getElementById('app');

function render(){
  if(state.screen==='title') return renderTitle();
  if(state.screen==='overview') return renderOverview();
  if(state.screen==='map') return renderMap();
  if(state.screen==='shrine_intro') return renderShrineIntro();
  if(state.screen==='quiz') return renderQuiz();
  if(state.screen==='bonus_intro') return renderBonusIntro();
  if(state.screen==='bonus') return renderBonus();
  if(state.screen==='shrine_result') return renderShrineResult();
  if(state.screen==='final') return renderFinal();
}

function renderTitle(){
  app.innerHTML = `
  <div class="panel fade-in" style="text-align:center;">
    <div style="font-size:3.4rem; margin-bottom:6px;">🏛️🏀</div>
    <h1 class="glow-title">COURT BREAKER</h1>
    <h3 style="color:#d9cba8; margin-bottom:18px;">The Hoop Shrine</h3>
    <p class="subtitle" style="max-width:520px; margin:0 auto 20px;">
      Legend tells of an ancient arena buried beneath the city — sealed shut by four broken
      shrines. Each shrine guards a secret about <b>linear functions</b>. Solve them, and the
      Hoop Shrine will open once more.
    </p>
    <button class="btn" onclick="goTo('overview')">Begin the Quest</button>
    <div style="margin-top:28px; font-size:.75rem; color:#8a7a55; letter-spacing:.5px;">Developed by Iron Kim</div>
  </div>`;
}

function renderOverview(){
  app.innerHTML = `
  <div class="panel fade-in">
    <button class="close-x" onclick="goTo('title')">✕</button>
    <h2 class="glow-title">The Four Shrines</h2>
    <p class="subtitle">Before you enter, the old caretaker explains what each shrine will test. This is exactly what shows up on the real exam.</p>
    <div class="grid2">
      <div class="report-card">
        <div class="row">${ICONS.coords}<b style="margin-left:8px;">1. Shrine of Coordinates</b></div>
        <p style="font-size:.9rem; color:#d9cba8;">Build a linear equation from two points or a table. Find the slope (rate of change), then the y-intercept (starting value).</p>
      </div>
      <div class="report-card">
        <div class="row">${ICONS.word}<b style="margin-left:8px;">2. Shrine of the Storyteller</b></div>
        <p style="font-size:.9rem; color:#d9cba8;">Turn a word problem into a function. Find what's "flat/fixed" (b) and what's "per unit" (m).</p>
      </div>
      <div class="report-card">
        <div class="row">${ICONS.explain}<b style="margin-left:8px;">3. Shrine of Meaning</b></div>
        <p style="font-size:.9rem; color:#d9cba8;">Explain in plain English what the slope, y-intercept, or x-intercept actually represents in real life.</p>
      </div>
      <div class="report-card">
        <div class="row">${ICONS.transform}<b style="margin-left:8px;">4. Shrine of Reflection</b></div>
        <p style="font-size:.9rem; color:#d9cba8;">Given f(x) and g(x), substitute carefully to transform or evaluate functions.</p>
      </div>
    </div>
    <div style="background:rgba(244,196,74,.1); border:1px solid var(--gold); border-radius:10px; padding:14px 16px; margin-top:6px;">
      <b style="color:var(--gold-bright);">The Method, every time:</b>
      <ul class="tight" style="font-size:.9rem; color:#e8dcc0;">
        <li>Identify the slope (m) — the rate of change.</li>
        <li>Identify the y-intercept (b) — the starting value, when input = 0.</li>
        <li>Build y = mx + b, or substitute carefully if transforming a function.</li>
        <li>Re-read the question — does it want a number, an equation, or an explanation?</li>
      </ul>
    </div>
    <div style="margin-top:18px; text-align:center;">
      <button class="btn" onclick="goTo('map')">Enter the Shrine Map →</button>
    </div>
  </div>`;
}

function renderMap(){
  let cards = SHRINES.map((s, i)=>{
    let cls = 'shrine-card';
    if(!state.unlocked[i]) cls += ' locked';
    else if(state.completed[i]) cls += ' done';
    else cls += ' active';
    let badge = state.completed[i] ? '<div class="badge">CLEARED</div>' : (state.unlocked[i] ? '<div class="badge" style="background:#3fb8af;color:#0b2a27;">READY</div>' : '<div class="badge" style="background:#5a4d33;color:#bdb190;">LOCKED</div>');
    let clickable = state.unlocked[i] ? `onclick="enterShrine(${i})"` : '';
    return `<div class="${cls}" style="${state.unlocked[i]?'cursor:pointer':''}" ${clickable}>
      ${s.icon}
      <div class="label">${s.title}</div>
      ${badge}
    </div>`;
  }).join('');

  let allDone = state.completed.every(c=>c);

  app.innerHTML = `
  <div class="panel fade-in">
    <div class="status-row">
      <h2 class="glow-title" style="margin:0;">Hoop Shrine Map</h2>
      <span class="pill">${state.completed.filter(c=>c).length} / 4 shrines cleared</span>
    </div>
    <p class="subtitle">Tap a glowing shrine to enter. Shrines unlock in order on your first run — but once cleared, you can always go back and replay for new questions.</p>
    <div class="map-grid">${cards}</div>
    ${allDone ? `<div style="text-align:center; margin-top:10px;"><button class="btn" onclick="goTo('final')">🏆 View Final Report</button></div>` : ''}
    <div style="text-align:center; margin-top:14px;">
      <button class="btn secondary" onclick="goTo('overview')">Review the Method</button>
    </div>
  </div>`;
}

function enterShrine(i){
  state.currentShrineIdx = i;
  state.screen = 'shrine_intro';
  render();
}

function renderShrineIntro(){
  const s = SHRINES[state.currentShrineIdx];
  app.innerHTML = `
  <div class="panel fade-in" style="text-align:center;">
    <div style="font-size:2.6rem;">${s.icon}</div>
    <h2 class="glow-title">${s.title}</h2>
    <p class="subtitle" style="max-width:560px; margin:0 auto 18px;">${s.intro}</p>
    <div class="row" style="justify-content:center;">
      <button class="btn" onclick="startQuiz()">Step Inside →</button>
      <button class="btn secondary" onclick="goTo('map')">Back to Map</button>
    </div>
  </div>`;
}

function startQuiz(){
  const s = SHRINES[state.currentShrineIdx];
  state.runQuestions = pickRun(s.pool);
  state.qIndex = 0;
  state.hintTier = 0;
  state.answered = false;
  state.screen = 'quiz';
  render();
}

function currentQuestion(){
  return state.runQuestions[state.qIndex];
}

function renderQuiz(){
  const s = SHRINES[state.currentShrineIdx];
  const q = currentQuestion();
  const progressPct = Math.round((state.qIndex / state.runQuestions.length)*100);

  let answerHtml = '';
  if(q.type==='mc'){
    answerHtml = `<div class="answers" id="answersWrap">` +
      q.choices.map((c,i)=>`<button class="answer-btn" data-i="${i}" onclick="selectAnswer(${i})">${c}</button>`).join('') +
      `</div>`;
  } else {
    answerHtml = `<div style="margin-top:10px;">
      <input type="text" class="text-answer" id="numAnswer" placeholder="Your answer">
      <button class="btn" onclick="submitNumAnswer()">Submit</button>
    </div>`;
  }

  app.innerHTML = `
  <div class="panel fade-in">
    <div class="status-row">
      <span class="pill">${s.title}</span>
      <span class="pill">Question ${state.qIndex+1} of ${state.runQuestions.length}</span>
    </div>
    <div class="progress-bar"><div class="progress-fill" style="width:${progressPct}%;"></div></div>
    <div class="question-box">
      <div class="qmeta">${q.type==='mc' ? 'Choose the best answer' : 'Type your answer'}</div>
      ${q.prompt}
    </div>
    ${answerHtml}
    <div id="feedbackArea"></div>
    <div class="hint-area">
      <button class="btn teal" id="hintBtn" onclick="showHint()">💡 Need a hint? (Tier ${state.hintTier+1 > 3 ? 3 : state.hintTier+1})</button>
      <div id="hintBox"></div>
    </div>
  </div>`;
  renderHints();
}

function renderHints(){
  const box = document.getElementById('hintBox');
  if(!box) return;
  const q = currentQuestion();
  let html = '';
  for(let i=0;i<state.hintTier;i++){
    html += `<div class="hint-box"><b>Hint ${i+1}:</b> ${q.hints[i]}</div>`;
  }
  box.innerHTML = html;
  const btn = document.getElementById('hintBtn');
  if(btn && state.hintTier>=3){
    btn.disabled = true;
    btn.innerText = '💡 All hints revealed';
  }
}

function showHint(){
  if(state.hintTier < 3){
    state.hintTier++;
    const shrineIdx = state.currentShrineIdx;
    if(state.hintTier===1) state.shrineStats[shrineIdx].hintTier1++;
    if(state.hintTier===2) state.shrineStats[shrineIdx].hintTier2++;
    if(state.hintTier===3) state.shrineStats[shrineIdx].hintTier3++;
    renderHints();
  }
}

function selectAnswer(i){
  if(state.answered) return;
  state.answered = true;
  const q = currentQuestion();
  const correct = (i === q.correct);
  finalizeAnswer(correct);
  // visual feedback on buttons
  const wrap = document.getElementById('answersWrap');
  const btns = wrap.querySelectorAll('.answer-btn');
  btns.forEach((b,idx)=>{
    b.disabled = true;
    if(idx===q.correct) b.classList.add('correct');
    else if(idx===i && !correct) b.classList.add('wrong');
  });
  showFeedback(correct, q);
}

function submitNumAnswer(){
  if(state.answered) return;
  const val = document.getElementById('numAnswer').value.trim();
  const q = currentQuestion();
  const correct = (val !== '' && val.replace(/\s/g,'') === q.answer);
  state.answered = true;
  finalizeAnswer(correct);
  document.getElementById('numAnswer').disabled = true;
  showFeedback(correct, q, val);
}

function finalizeAnswer(correct){
  const stats = state.shrineStats[state.currentShrineIdx];
  stats.totalAsked++;
  if(correct && state.hintTier===0) stats.firstTry++;
  if(!correct) stats.wrong++;
}

function showFeedback(correct, q, userVal){
  const area = document.getElementById('feedbackArea');
  const isLast = state.qIndex === state.runQuestions.length - 1;
  area.innerHTML = `
    <div class="feedback ${correct?'good':'bad'}">
      <b>${correct ? '✅ Correct!' : '❌ Not quite.'}</b>
      <div style="margin-top:6px;">${q.explain}</div>
      ${(!correct) ? `<div class="small-note">The correct answer ${q.type==='mc' ? 'was: "' + q.choices[q.correct] + '"' : 'was: ' + q.answer}</div>` : ''}
    </div>
    <div style="text-align:right; margin-top:14px;">
      <button class="btn" onclick="nextQuestion()">${isLast ? 'See Shrine Results →' : 'Next Question →'}</button>
    </div>
  `;
  document.getElementById('hintBtn').disabled = true;
}

function nextQuestion(){
  if(state.qIndex < state.runQuestions.length - 1){
    state.qIndex++;
    state.hintTier = 0;
    state.answered = false;
    render();
  } else {
    state.screen = 'bonus_intro';
    render();
  }
}

function renderBonusIntro(){
  const stats = state.shrineStats[state.currentShrineIdx];
  app.innerHTML = `
  <div class="panel fade-in" style="text-align:center;">
    <h2 class="glow-title">⚡ Bonus Challenge</h2>
    <p class="subtitle" style="max-width:480px; margin:0 auto 18px;">
      The shrine rumbles — a final timed trial appears! Answer fast for bonus glory.
      Don't worry: <b>there's no penalty</b> if you run out of time or skip — the shrine is
      already cleared either way.
    </p>
    <button class="btn" onclick="startBonus()">Start Bonus Round (Timed)</button>
    <div style="margin-top:10px;">
      <button class="btn secondary" onclick="finishShrine()">Skip Bonus, Continue →</button>
    </div>
  </div>`;
}

function startBonus(){
  const stats = state.shrineStats[state.currentShrineIdx];
  const s = SHRINES[state.currentShrineIdx];
  // determine correct count from the just-finished run
  const justPlayedCorrect = state.runQuestions.length - state.shrineStats[state.currentShrineIdx].wrongThisRun || 0;
  state.screen = 'bonus';
  state.bonusIdx = 0;
  state.bonusCorrectCount = 0;
  state.bonusResults = [];

  // decide: reuse same questions or fresh ones, based on correctCountThisRun tracked separately
  const correctThisRun = state.lastRunCorrectCount;
  if(correctThisRun >= 3){
    // fresh question(s) not used in this run
    const usedIds = state.runQuestions.map(q=>q.id);
    const fresh = s.pool.filter(q=>!usedIds.includes(q.id));
    state.bonusQueue = shuffle(fresh).slice(0,2);
    if(state.bonusQueue.length===0) state.bonusQueue = shuffle(state.runQuestions).slice(0,2);
  } else {
    state.bonusQueue = state.runQuestions.slice(0,2);
  }
  state.bonusTimeLeft = 30;
  renderBonusQuestion();
}

let bonusTimerHandle = null;

function renderBonusQuestion(){
  clearInterval(bonusTimerHandle);
  const q = state.bonusQueue[state.bonusIdx];
  state.bonusAnswered = false;
  state.bonusTimeLeft = 30;

  let answerHtml = '';
  if(q.type==='mc'){
    answerHtml = `<div class="answers" id="bAnswersWrap">` +
      q.choices.map((c,i)=>`<button class="answer-btn" data-i="${i}" onclick="bonusSelect(${i})">${c}</button>`).join('') +
      `</div>`;
  } else {
    answerHtml = `<div style="margin-top:10px;">
      <input type="text" class="text-answer" id="bNumAnswer" placeholder="Your answer">
      <button class="btn" onclick="bonusSubmitNum()">Submit</button>
    </div>`;
  }

  app.innerHTML = `
  <div class="panel fade-in">
    <div class="status-row">
      <span class="pill">⚡ Bonus Round</span>
      <span class="pill">Question ${state.bonusIdx+1} of ${state.bonusQueue.length}</span>
    </div>
    <div class="timer-wrap">
      <span style="font-size:.85rem;">⏱</span>
      <div class="timer-bar"><div class="timer-fill" id="timerFill" style="width:100%;"></div></div>
    </div>
    <div class="question-box">${q.prompt}</div>
    ${answerHtml}
    <div id="bFeedbackArea"></div>
  </div>`;

  let total = 30;
  bonusTimerHandle = setInterval(()=>{
    state.bonusTimeLeft -= 0.2;
    const pct = Math.max(0,(state.bonusTimeLeft/total)*100);
    const fill = document.getElementById('timerFill');
    if(fill) fill.style.width = pct+'%';
    if(state.bonusTimeLeft<=0){
      clearInterval(bonusTimerHandle);
      if(!state.bonusAnswered) bonusTimeUp();
    }
  }, 200);
}

function bonusTimeUp(){
  if(state.bonusAnswered) return;
  state.bonusAnswered = true;
  state.bonusResults.push(false);
  const area = document.getElementById('bFeedbackArea');
  const q = state.bonusQueue[state.bonusIdx];
  if(area) area.innerHTML = `<div class="feedback bad"><b>⏰ Time's up!</b><div style="margin-top:6px;">${q.explain}</div></div>
  <div style="text-align:right;margin-top:14px;"><button class="btn" onclick="nextBonus()">Continue →</button></div>`;
  disableBonusInputs();
}

function disableBonusInputs(){
  const wrap = document.getElementById('bAnswersWrap');
  if(wrap) wrap.querySelectorAll('button').forEach(b=>b.disabled=true);
  const input = document.getElementById('bNumAnswer');
  if(input) input.disabled = true;
}

function bonusSelect(i){
  if(state.bonusAnswered) return;
  state.bonusAnswered = true;
  clearInterval(bonusTimerHandle);
  const q = state.bonusQueue[state.bonusIdx];
  const correct = (i===q.correct);
  state.bonusResults.push(correct);
  if(correct) state.bonusCorrectCount++;
  const wrap = document.getElementById('bAnswersWrap');
  wrap.querySelectorAll('.answer-btn').forEach((b,idx)=>{
    b.disabled = true;
    if(idx===q.correct) b.classList.add('correct');
    else if(idx===i && !correct) b.classList.add('wrong');
  });
  bonusFeedback(correct,q);
}

function bonusSubmitNum(){
  if(state.bonusAnswered) return;
  const val = document.getElementById('bNumAnswer').value.trim();
  const q = state.bonusQueue[state.bonusIdx];
  const correct = (val!=='' && val.replace(/\s/g,'')===q.answer);
  state.bonusAnswered = true;
  clearInterval(bonusTimerHandle);
  state.bonusResults.push(correct);
  if(correct) state.bonusCorrectCount++;
  document.getElementById('bNumAnswer').disabled = true;
  bonusFeedback(correct,q);
}

function bonusFeedback(correct,q){
  const area = document.getElementById('bFeedbackArea');
  const isLast = state.bonusIdx === state.bonusQueue.length-1;
  area.innerHTML = `<div class="feedback ${correct?'good':'bad'}"><b>${correct?'⚡ Nailed it!':'❌ Not this time.'}</b>
  <div style="margin-top:6px;">${q.explain}</div></div>
  <div style="text-align:right;margin-top:14px;"><button class="btn" onclick="nextBonus()">${isLast?'Finish Bonus →':'Next →'}</button></div>`;
}

function nextBonus(){
  if(state.bonusIdx < state.bonusQueue.length-1){
    state.bonusIdx++;
    renderBonusQuestion();
  } else {
    const stats = state.shrineStats[state.currentShrineIdx];
    stats.bonusAttempted += state.bonusQueue.length;
    stats.bonusCorrect += state.bonusCorrectCount;
    finishShrine();
  }
}

function finishShrine(){
  state.completed[state.currentShrineIdx] = true;
  if(state.currentShrineIdx + 1 < SHRINES.length){
    state.unlocked[state.currentShrineIdx+1] = true;
  }
  state.screen = 'shrine_result';
  render();
}

function renderShrineResult(){
  const s = SHRINES[state.currentShrineIdx];
  const stats = state.shrineStats[state.currentShrineIdx];
  const acc = stats.totalAsked ? Math.round((stats.firstTry/stats.totalAsked)*100) : 0;
  app.innerHTML = `
  <div class="panel fade-in" style="text-align:center;">
    <div style="font-size:2.4rem;">🔓</div>
    <h2 class="glow-title">${s.title} — Cleared!</h2>
    <p class="subtitle">The seal breaks and golden light pours out. Great work!</p>
    <div class="report-card" style="text-align:left; max-width:420px; margin:14px auto;">
      <div>✅ First-try correct: <b>${stats.firstTry} / ${stats.totalAsked}</b></div>
      <div>💡 Hints used: <b>${stats.hintTier1+stats.hintTier2+stats.hintTier3}</b></div>
      ${stats.bonusAttempted ? `<div>⚡ Bonus round: <b>${stats.bonusCorrect} / ${stats.bonusAttempted}</b></div>` : ''}
    </div>
    <button class="btn" onclick="goTo('map')">Return to the Map →</button>
  </div>`;
}

function renderFinal(){
  const labels = SHRINES.map(s=>s.title);
  let rows = SHRINES.map((s,i)=>{
    const stats = state.shrineStats[i];
    const acc = stats.totalAsked ? (stats.firstTry/stats.totalAsked) : 0;
    const pct = Math.round(acc*100);
    let color = pct>=75 ? 'var(--success)' : (pct>=40 ? 'var(--gold)' : 'var(--danger)');
    let level = pct>=75 ? 'Strong 💪' : (pct>=40 ? 'Developing 🌱' : 'Needs Work 🛠️');
    const totalHints = stats.hintTier1+stats.hintTier2+stats.hintTier3;
    return `
    <div class="report-card">
      <div class="row" style="justify-content:space-between;">
        <b>${s.title}</b><span style="color:${color}; font-weight:700;">${level}</span>
      </div>
      <div class="bar-track"><div class="bar-fill" style="width:${pct}%; background:${color};"></div></div>
      <div class="small-note">First-try accuracy: ${pct}% (${stats.firstTry}/${stats.totalAsked}) • Hints used: ${totalHints} ${stats.bonusAttempted ? `• Bonus: ${stats.bonusCorrect}/${stats.bonusAttempted}` : ''}</div>
    </div>`;
  }).join('');

  // find weakest area for takeaway text
  let weakest = null, weakestPct = 101;
  SHRINES.forEach((s,i)=>{
    const stats = state.shrineStats[i];
    const acc = stats.totalAsked ? (stats.firstTry/stats.totalAsked)*100 : 0;
    if(acc < weakestPct){ weakestPct = acc; weakest = s.title; }
  });
  let strongest = null, strongestPct = -1;
  SHRINES.forEach((s,i)=>{
    const stats = state.shrineStats[i];
    const acc = stats.totalAsked ? (stats.firstTry/stats.totalAsked)*100 : 0;
    if(acc > strongestPct){ strongestPct = acc; strongest = s.title; }
  });

  app.innerHTML = `
  <div class="panel fade-in">
    <h2 class="glow-title" style="text-align:center;">🏆 The Hoop Shrine Opens!</h2>
    <p class="subtitle" style="text-align:center;">Here's your tutor's-eye-view report card.</p>
    ${rows}
    <div style="background:rgba(244,196,74,.1); border:1px solid var(--gold); border-radius:10px; padding:14px 16px; margin-top:10px;">
      <b style="color:var(--gold-bright);">Takeaway for your tutor:</b>
      <p style="font-size:.92rem; color:#e8dcc0; margin:8px 0 0;">
        Strongest area: <b>${strongest}</b> (${Math.round(strongestPct)}% first-try). 
        Most room to grow: <b>${weakest}</b> (${Math.round(weakestPct)}% first-try) — worth reviewing together next session.
      </p>
    </div>
    <div class="row" style="justify-content:center; margin-top:18px;">
      <button class="btn" onclick="goTo('map')">Replay Any Shrine</button>
      <button class="btn secondary" onclick="resetGame()">Restart Whole Quest</button>
    </div>
    <div style="border-top:1px dashed #5a4d33; margin-top:22px; padding-top:18px; text-align:center;">
      <p class="small-note" style="margin-bottom:10px;">Send this report card to your tutor:</p>
      <div class="row" style="justify-content:center;">
        <button class="btn teal" onclick="emailReport()">📧 Email Iron my results</button>
        <button class="btn teal" onclick="textReport()">📱 Text Iron my results</button>
      </div>
      <p class="small-note" style="margin-top:10px;">This opens your phone or computer's own email/messages app with the report pre-filled — just hit send.</p>
    </div>
  </div>`;
}

function buildReportText(){
  let lines = ["COURT BREAKER — Joe's Hoop Shrine Report Card", ""];
  SHRINES.forEach((s,i)=>{
    const stats = state.shrineStats[i];
    const pct = stats.totalAsked ? Math.round((stats.firstTry/stats.totalAsked)*100) : 0;
    const totalHints = stats.hintTier1+stats.hintTier2+stats.hintTier3;
    lines.push(`${s.title}: ${pct}% first-try (${stats.firstTry}/${stats.totalAsked}), ${totalHints} hints used${stats.bonusAttempted ? `, bonus ${stats.bonusCorrect}/${stats.bonusAttempted}` : ''}`);
  });
  let weakest = null, weakestPct = 101, strongest = null, strongestPct = -1;
  SHRINES.forEach((s,i)=>{
    const stats = state.shrineStats[i];
    const acc = stats.totalAsked ? (stats.firstTry/stats.totalAsked)*100 : 0;
    if(acc < weakestPct){ weakestPct = acc; weakest = s.title; }
    if(acc > strongestPct){ strongestPct = acc; strongest = s.title; }
  });
  lines.push("");
  lines.push(`Strongest: ${strongest} (${Math.round(strongestPct)}%)`);
  lines.push(`Needs work: ${weakest} (${Math.round(weakestPct)}%)`);
  return lines.join("\n");
}

function emailReport(){
  const body = buildReportText();
  const subject = "Joe's Court Breaker Report Card";
  const url = `mailto:irontjkim@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
  window.location.href = url;
}

function textReport(){
  const body = buildReportText();
  const url = `sms:+12014009220?&body=${encodeURIComponent(body)}`;
  window.location.href = url;
}

function resetGame(){
  state = {
    screen:'title', currentShrineIdx:0,
    unlocked:[true,false,false,false],
    completed:[false,false,false,false],
    shrineStats: SHRINES.map(()=>({firstTry:0,hintTier1:0,hintTier2:0,hintTier3:0,wrong:0,totalAsked:0,bonusCorrect:0,bonusAttempted:0})),
    runQuestions:[], qIndex:0, hintTier:0, answered:false
  };
  render();
}

function goTo(screen){
  state.screen = screen;
  render();
}

// track correct count per run for bonus branching logic
const _origFinalize = finalizeAnswer;
let runCorrectTracker = 0;
function finalizeAnswerWrapped(correct){
  _origFinalize(correct);
}

// Patch: track run correctness via a side counter
let runCorrectCount = 0;
const originalFinalize = finalizeAnswer;
finalizeAnswer = function(correct){
  originalFinalize(correct);
  if(state.qIndex === 0) runCorrectCount = 0;
  if(correct) runCorrectCount++;
  state.lastRunCorrectCount = runCorrectCount;
};

render();
</script>
</body>
</html>
