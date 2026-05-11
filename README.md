<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Hex Herbalist — สารานุกรมสมุนไพร</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Sarabun:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0e120d;
    --bg2: #141a12;
    --bg3: #1a2218;
    --card: #1e2a1c;
    --card2: #243022;
    --border: rgba(120,160,80,0.18);
    --border2: rgba(120,160,80,0.35);
    --accent: #7ab840;
    --accent2: #a8d060;
    --accent3: #c8e88a;
    --gold: #d4a843;
    --gold2: #f0c86a;
    --red: #c0402a;
    --red2: #e05535;
    --blue: #4488bb;
    --blue2: #66aadd;
    --text: #d8e8c8;
    --text2: #9ab888;
    --text3: #6a8858;
    --muted: #4a6040;
    --warm: #e8d4a0;
    --cool: #8abcdc;
    --font-main: 'Sarabun', sans-serif;
    --font-display: 'Playfair Display', serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-main);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Particle canvas */
  #particles-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  /* Header */
  .site-header {
    position: relative;
    z-index: 10;
    text-align: center;
    padding: 4rem 2rem 2rem;
    border-bottom: 1px solid var(--border);
  }

  .site-header::before {
    content: '';
    position: absolute;
    top: 0; left: 50%; transform: translateX(-50%);
    width: 300px; height: 2px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
  }

  .header-symbol {
    font-size: 2.5rem;
    margin-bottom: 0.5rem;
    display: inline-block;
    animation: float 4s ease-in-out infinite;
    filter: drop-shadow(0 0 12px rgba(122,184,64,0.5));
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-8px); }
  }

  .site-title {
    font-family: var(--font-display);
    font-size: 3.5rem;
    font-weight: 700;
    letter-spacing: 0.05em;
    background: linear-gradient(135deg, var(--accent2) 0%, var(--gold2) 50%, var(--accent3) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1.1;
  }

  .site-subtitle {
    font-size: 0.95rem;
    color: var(--text2);
    letter-spacing: 0.3em;
    text-transform: uppercase;
    margin-top: 0.4rem;
  }

  /* Search bar */
  .search-container {
    position: relative;
    z-index: 10;
    max-width: 600px;
    margin: 2rem auto;
    padding: 0 1.5rem;
  }

  .search-wrap {
    position: relative;
    display: flex;
    align-items: center;
  }

  .search-icon {
    position: absolute;
    left: 1.2rem;
    color: var(--text3);
    font-size: 1.1rem;
    pointer-events: none;
  }

  .search-input {
    width: 100%;
    background: var(--card);
    border: 1px solid var(--border2);
    border-radius: 999px;
    padding: 0.8rem 1.2rem 0.8rem 3rem;
    color: var(--text);
    font-family: var(--font-main);
    font-size: 1rem;
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
  }

  .search-input::placeholder { color: var(--muted); }
  .search-input:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(122,184,64,0.15);
  }

  /* Filter chips */
  .filter-bar {
    position: relative;
    z-index: 10;
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
    justify-content: center;
    max-width: 900px;
    margin: 0 auto 2rem;
    padding: 0 1.5rem;
  }

  .filter-chip {
    padding: 0.35rem 1rem;
    border-radius: 999px;
    border: 1px solid var(--border);
    background: var(--card);
    color: var(--text2);
    font-family: var(--font-main);
    font-size: 0.82rem;
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
  }

  .filter-chip:hover { border-color: var(--accent); color: var(--accent2); }
  .filter-chip.active {
    background: var(--accent);
    border-color: var(--accent);
    color: #0e120d;
    font-weight: 600;
  }

  /* Main layout */
  .main-layout {
    position: relative;
    z-index: 10;
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: 0;
    max-width: 1400px;
    margin: 0 auto;
    min-height: calc(100vh - 250px);
  }

  /* Sidebar */
  .sidebar {
    border-right: 1px solid var(--border);
    padding: 1.5rem;
    overflow-y: auto;
    max-height: calc(100vh - 250px);
    position: sticky;
    top: 0;
  }

  .sidebar-title {
    font-size: 0.7rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--text3);
    margin-bottom: 1rem;
    padding-left: 0.5rem;
  }

  .herb-list {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .herb-list-item {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.65rem 0.75rem;
    border-radius: 10px;
    cursor: pointer;
    transition: all 0.2s;
    border: 1px solid transparent;
  }

  .herb-list-item:hover {
    background: var(--card);
    border-color: var(--border);
  }

  .herb-list-item.active {
    background: var(--card2);
    border-color: var(--accent);
  }

  .herb-list-item.hidden { display: none; }

  .herb-icon {
    width: 32px; height: 32px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 1rem;
    flex-shrink: 0;
  }

  .herb-list-info { flex: 1; min-width: 0; }
  .herb-list-name { font-size: 0.88rem; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .herb-list-category { font-size: 0.72rem; color: var(--text3); }

  .effect-badge {
    font-size: 0.62rem;
    padding: 0.15rem 0.4rem;
    border-radius: 4px;
    font-weight: 600;
    flex-shrink: 0;
  }

  .effect-cool { background: rgba(68,136,187,0.2); color: var(--cool); }
  .effect-hot { background: rgba(192,64,42,0.2); color: var(--red2); }
  .effect-neutral { background: rgba(122,184,64,0.2); color: var(--accent2); }

  /* Detail panel */
  .detail-panel {
    padding: 2rem 2.5rem;
    overflow-y: auto;
    max-height: calc(100vh - 250px);
  }

  .detail-placeholder {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 60vh;
    text-align: center;
    color: var(--text3);
  }

  .detail-placeholder .big-symbol { font-size: 4rem; margin-bottom: 1rem; opacity: 0.4; }
  .detail-placeholder p { font-size: 1rem; color: var(--text3); }

  /* Herb detail */
  .herb-detail { display: none; animation: fadeInUp 0.4s ease; }
  .herb-detail.active { display: block; }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .detail-header {
    display: flex;
    align-items: flex-start;
    gap: 1.5rem;
    margin-bottom: 2rem;
    padding-bottom: 2rem;
    border-bottom: 1px solid var(--border);
  }

  .detail-icon-wrap {
    width: 80px; height: 80px;
    border-radius: 20px;
    display: flex; align-items: center; justify-content: center;
    font-size: 2.8rem;
    flex-shrink: 0;
    position: relative;
    overflow: hidden;
  }

  .detail-icon-wrap::after {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 20px;
    border: 1px solid var(--border2);
  }

  .detail-title-group { flex: 1; }

  .detail-name-en {
    font-family: var(--font-display);
    font-size: 2rem;
    font-weight: 700;
    color: var(--accent3);
    line-height: 1.1;
  }

  .detail-name-th {
    font-size: 1.1rem;
    color: var(--text2);
    margin-top: 0.2rem;
  }

  .detail-meta {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
    margin-top: 0.75rem;
  }

  .meta-tag {
    font-size: 0.75rem;
    padding: 0.25rem 0.75rem;
    border-radius: 999px;
    border: 1px solid;
    font-weight: 500;
  }

  /* Tabs */
  .detail-tabs {
    display: flex;
    gap: 0;
    margin-bottom: 1.5rem;
    border-bottom: 1px solid var(--border);
  }

  .detail-tab {
    padding: 0.6rem 1.2rem;
    font-size: 0.85rem;
    cursor: pointer;
    color: var(--text3);
    border-bottom: 2px solid transparent;
    transition: all 0.2s;
    white-space: nowrap;
    font-family: var(--font-main);
    background: none;
    border-top: none; border-left: none; border-right: none;
  }

  .detail-tab:hover { color: var(--text2); }
  .detail-tab.active {
    color: var(--accent2);
    border-bottom-color: var(--accent);
  }

  .tab-content { display: none; }
  .tab-content.active { display: block; animation: fadeInUp 0.3s ease; }

  /* Description */
  .herb-desc {
    font-size: 0.95rem;
    line-height: 1.8;
    color: var(--text2);
    margin-bottom: 1.5rem;
    padding: 1.25rem;
    background: var(--card);
    border-radius: 12px;
    border: 1px solid var(--border);
  }

  /* Properties grid */
  .properties-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .property-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem;
    transition: border-color 0.2s;
  }

  .property-card:hover { border-color: var(--border2); }

  .property-icon { font-size: 1.3rem; margin-bottom: 0.5rem; }
  .property-title { font-size: 0.78rem; letter-spacing: 0.1em; text-transform: uppercase; color: var(--text3); margin-bottom: 0.35rem; }
  .property-text { font-size: 0.9rem; color: var(--text2); line-height: 1.6; }

  /* Usage steps */
  .usage-method {
    margin-bottom: 1.5rem;
    background: var(--card);
    border-radius: 12px;
    border: 1px solid var(--border);
    overflow: hidden;
  }

  .usage-method-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.85rem 1.1rem;
    background: var(--card2);
    border-bottom: 1px solid var(--border);
    cursor: pointer;
    transition: background 0.2s;
  }

  .usage-method-header:hover { background: var(--bg3); }

  .method-label { font-size: 0.88rem; font-weight: 600; color: var(--accent2); flex: 1; }
  .method-arrow { color: var(--text3); font-size: 0.75rem; transition: transform 0.25s; }
  .usage-method.open .method-arrow { transform: rotate(180deg); }

  .usage-method-body {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.35s ease;
  }

  .usage-method.open .usage-method-body { max-height: 600px; }

  .method-steps {
    padding: 1rem 1.1rem;
    font-size: 0.9rem;
    color: var(--text2);
    line-height: 1.7;
  }

  /* Warning box */
  .warning-box {
    background: rgba(192,64,42,0.1);
    border: 1px solid rgba(192,64,42,0.3);
    border-radius: 12px;
    padding: 1rem 1.1rem;
    font-size: 0.9rem;
    color: var(--warm);
    line-height: 1.7;
  }

  .warning-box strong {
    display: block;
    color: var(--red2);
    font-size: 0.78rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 0.4rem;
  }

  /* Progress bar for properties */
  .prop-bar-wrap { margin-top: 0.5rem; }
  .prop-bar {
    height: 4px;
    border-radius: 2px;
    background: var(--border);
    overflow: hidden;
    margin-top: 0.3rem;
  }

  .prop-bar-fill {
    height: 100%;
    border-radius: 2px;
    transition: width 1s ease;
    width: 0;
  }

  /* Responsive */
  @media (max-width: 900px) {
    .main-layout { grid-template-columns: 1fr; }
    .sidebar { max-height: 250px; position: static; }
    .detail-panel { max-height: none; }
    .site-title { font-size: 2.2rem; }
  }

  /* Floating ornament particles */
  .ornament {
    position: fixed;
    pointer-events: none;
    z-index: 1;
    font-size: 1.5rem;
    opacity: 0;
    animation: drift 18s ease-in-out infinite;
  }

  @keyframes drift {
    0% { opacity: 0; transform: translateY(100vh) rotate(0deg); }
    10% { opacity: 0.15; }
    90% { opacity: 0.1; }
    100% { opacity: 0; transform: translateY(-100px) rotate(360deg); }
  }

  /* Glow on active sidebar item */
  .herb-list-item.active .herb-icon { box-shadow: 0 0 16px rgba(122,184,64,0.4); }

  /* No results */
  .no-results {
    text-align: center;
    padding: 2rem;
    color: var(--text3);
    font-size: 0.9rem;
    display: none;
  }
</style>
</head>
<body>

<canvas id="particles-canvas"></canvas>

<!-- Floating ornaments -->
<div class="ornament" style="left:5%; animation-delay:0s; animation-duration:20s;">🌿</div>
<div class="ornament" style="left:20%; animation-delay:4s; animation-duration:16s;">🍃</div>
<div class="ornament" style="left:40%; animation-delay:8s; animation-duration:22s;">✦</div>
<div class="ornament" style="left:60%; animation-delay:2s; animation-duration:18s;">🌱</div>
<div class="ornament" style="left:75%; animation-delay:12s; animation-duration:15s;">🍀</div>
<div class="ornament" style="left:88%; animation-delay:6s; animation-duration:24s;">🌸</div>

<header class="site-header">
  <div class="header-symbol">🌿</div>
  <h1 class="site-title">Hex Herbalist</h1>
  <p class="site-subtitle">สารานุกรมสมุนไพรและพืชสมุนไพร</p>
</header>

<div class="search-container">
  <div class="search-wrap">
    <span class="search-icon">🔍</span>
    <input class="search-input" type="text" id="searchInput" placeholder="ค้นหาสมุนไพร... (ภาษาไทยหรืออังกฤษ)">
  </div>
</div>

<div class="filter-bar" id="filterBar">
  <button class="filter-chip active" data-filter="all">ทั้งหมด</button>
  <button class="filter-chip" data-filter="berry">ผลเบอร์รี่</button>
  <button class="filter-chip" data-filter="mushroom">เห็ด</button>
  <button class="filter-chip" data-filter="herb">สมุนไพร</button>
  <button class="filter-chip" data-filter="cool">ฤทธิ์เย็น</button>
  <button class="filter-chip" data-filter="hot">ฤทธิ์ร้อน</button>
  <button class="filter-chip" data-filter="neutral">ฤทธิ์กลาง</button>
  <button class="filter-chip" data-filter="immune">เสริมภูมิคุ้มกัน</button>
  <button class="filter-chip" data-filter="digestive">ระบบย่อยอาหาร</button>
  <button class="filter-chip" data-filter="wound">สมานแผล</button>
  <button class="filter-chip" data-filter="nerve">ระบบประสาท</button>
  <button class="filter-chip" data-filter="danger">⚠ อันตราย</button>
</div>

<div class="main-layout">
  <aside class="sidebar">
    <p class="sidebar-title">รายการสมุนไพร</p>
    <div class="herb-list" id="herbList"></div>
    <div class="no-results" id="noResults">ไม่พบสมุนไพรที่ค้นหา</div>
  </aside>

  <main class="detail-panel" id="detailPanel">
    <div class="detail-placeholder" id="placeholder">
      <div class="big-symbol">🌿</div>
      <p>เลือกสมุนไพรจากรายการเพื่อดูข้อมูล</p>
    </div>
    <div id="herbDetails"></div>
  </main>
</div>

<script>
const herbs = [
  // ─── BERRIES ───────────────────────────────────────────────
  {
    id: 'blueberry',
    nameEn: 'Blueberry',
    nameTh: 'บลูเบอร์รี',
    icon: '🫐',
    iconBg: 'linear-gradient(135deg,#1a1a4a,#2a2a6a)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ต้านอนุมูลอิสระ', 'บำรุงสายตา', 'immune'],
    filterTags: ['berry', 'cool', 'immune'],
    description: 'ผลทรงกลมขนาดเล็ก ผิวสีน้ำเงินเข้มจนถึงสีดำ มีผงสีขาวเคลือบผิว เนื้อในเขียวอ่อนถึงขาว รสชาติหวานอมเปรี้ยว ใบรูปรีสีเขียวสด ในฤดูใบไม้ร่วงใบจะเปลี่ยนเป็นสีแดงสด ต้นและดอกเป็นไม้พุ่มสูงประมาณ 1-3 ม. ดอกมีรูปทรงคล้ายระฆังคว่ำ สีขาวหรือชมพูอ่อน',
    properties: [
      { icon: '👁', title: 'บำรุงสายตา', text: 'มีสารแอนโทไซนินสูง เพิ่มการไหลเวียนของเลือดในตาและลดความล้าจากการใช้ตา', level: 90 },
      { icon: '🛡', title: 'ต้านอนุมูลอิสระ', text: 'ต้านอนุมูลอิสระและชลอการอักเสบของร่างกาย', level: 85 },
      { icon: '🚽', title: 'ระบบทางเดินปัสสาวะ', text: 'ยับยั้งแบคทีเรียในกระเพาะปัสสาวะ', level: 70 },
      { icon: '🩸', title: 'ควบคุมน้ำตาล', text: 'ลดอินซูลิน ควบคุมเบาหวาน', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ล้างผลสะอาดแล้วผึ่งให้แห้ง ตากแดดจัด หรืออบที่ 50-60°C จนผลหดตัวและเหนียวหนึบ ใบสามารถตากแห้งและชงชาได้ เก็บในภาชนะที่แห้งสนิทเพื่อถนอมสาร' },
      { title: '🍵 การต้ม', steps: 'นำผลแห้งหรือใบ 1-2 ช้อนโต๊ะ ต้มน้ำสะอาด ไฟอ่อน 10 นาที หรือชงน้ำร้อน ปิดฝาทิ้งไว้ ช่วยแก้ท้องเสียอย่างอ่อน บรรเทาอาการกระเพาะปัสสาวะอักเสบและลดไข้' },
      { title: '🔨 การบด', steps: 'นำผลตากแห้งมาบดเป็นผงละเอียด หรือบดใบแห้งเป็นผงนวล ผงใบใช้ชงดื่มเพื่อลดระดับน้ำตาลในเลือด ผงผลบดผสมน้ำดื่มเพื่อบำรุงระบบประสาทและความจำ' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ผลสดหรือแห้งในแอลกอฮอล ทิ้งไว้ 4-6 สัปดาห์ (ใช้หยดผสมน้ำดื่มเพื่อบำรุงหัวใจและหลอดเลือด)\nดองน้ำผึ้ง: แช่ผลสดในน้ำผึ้งแท้ ทิ้งไว้ 2 สัปดาห์ (จิบแก้ไอบำรุงปอด เสริมภูมิคุ้มกัน)' },
    ],
    warning: 'ทานในปริมาณที่เหมาะสม มันหวาน ไม่เหมาะกับผู้ใช้ยาละลายลิ่มเลือด ทานมากไปทำให้ท้องผูกได้'
  },
  {
    id: 'blackberry',
    nameEn: 'Blackberry',
    nameTh: 'แบล็กเบอร์รี',
    icon: '🫐',
    iconBg: 'linear-gradient(135deg,#1a0a1a,#3a0a2a)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['สมานแผล', 'เสริมภูมิคุ้มกัน', 'wound', 'immune'],
    filterTags: ['berry', 'cool', 'immune', 'wound'],
    description: 'ผลเป็นผลกลุ่มประกอบด้วยผลย่อยเม็ดเล็กๆ รวมกัน ผิวเป็นมันวาว เมื่ออ่อนเป็นสีเขียว เปลี่ยนเป็นแดงและดำสนิดเมื่อสุกเต็มที่ รสชาติหวานอมเปรี้ยวและมีกลิ่นหอมเฉพาะตัว ลำต้นมีหนามแหลมคมกิ่งก้านยาวและยืดหยุ่น',
    properties: [
      { icon: '🩹', title: 'สมานแผลและแก้ท้องเสีย', text: 'ใบและรากมีสารคันดินสูง ช่วยสมานแผลในปากและหยุดอาการท้องเสีย', level: 80 },
      { icon: '🛡', title: 'ต้านอนุมูลอิสระ', text: 'มีสารแอนโทสินินสูง ช่วยป้องกันการเสื่อมของเซลล์และบำรุงผิวพรรณ', level: 88 },
      { icon: '💪', title: 'เสริมภูมิคุ้มกัน', text: 'มีวิตามินซีและวิตามินเคสูง ช่วยให้เลือดแข็งตัวได้ดีและป้องกันหวัด', level: 85 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บใบอ่อนหรือผลสุกล้างสะอาด ผึ่งในที่ร่มที่มีลมโกรก หรืออบอุณหภูมิต่ำ อบจนแห้งสนิด เก็บใส่โหลปิดสนิดเพื่อถนอม ไว้ใช้ชงชา แก้เจ็บคอหรือเหงือกอักเสบ' },
      { title: '🍵 การต้ม', steps: 'ใบแห้ง 1-2 ช้อนโต๊ะหรือรากแห้งต้มในน้ำสะอาดด้วยไฟอ่อน 10-15 นาที ดื่มหยุดอาการท้องเสียหรือกลั้วปากแก้เจ็บคอและรักษาแผลในช่องปาก' },
      { title: '🔨 การบด', steps: 'นำใบแห้งที่กรอบสนิดมาตำหรือบดให้เป็นผงละเอียด ใช้ผงใบโรยบนแผลสดเพื่อช่วยห้ามเลือดหรือผสมน้ำสะอาดพอกผิวเพื่อลดอาการอักเสบจากแมลงสัตว์กัดต่อย' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ผลหรือรากในแอลกอฮอล ทิ้งไว้ 4-6 สัปดาห์ (ใช้หยดผสมน้ำดื่ม 5-10 หยดเพื่อบำรุงระบบย่อยและลดไข้)\nดองน้ำส้มสายชู: แช่ผลสุกในน้ำส้มสายชูหมักทิ้งไว้ 3-4 สัปดาห์ (ใช้ผสมน้ำกลั้วคอแก้เจ็บคออย่างแรง)' },
    ],
    warning: 'การดื่มน้ำส้มมากเกินไปอาจเกิดท้องผูกหรือระคายเคืองกระเพาะอาหาร'
  },
  {
    id: 'elderberry',
    nameEn: 'Elderberry',
    nameTh: 'เอลเดอร์เบอร์รี',
    icon: '🍇',
    iconBg: 'linear-gradient(135deg,#1a0a2a,#3a1050)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ต้านไวรัส', 'ลดไข้', 'immune'],
    filterTags: ['berry', 'cool', 'immune'],
    description: 'หนึ่งในสมุนไพรที่ได้รับความนิยมสูงสุดในการแก้ไข้หวัดและเสริมภูมิคุ้มกัน ดอกออกเป็นช่อกระจุกแบนขนาดใหญ่สีขาวครีม มีกลิ่นหอมละมุนคล้ายน้ำผึ้งและเลมอน ผลทรงกลมขนาดเล็กออกเป็นพวงระย้า เมื่อสุกเต็มที่จะออกสีม่วงเข้มเกือบดำ',
    properties: [
      { icon: '🦠', title: 'ต้านไวรัส', text: 'มีสารแอนโทไซยานิลช่วยยับยั้งการก่อตัวของไวรัสไข้หวัดใหญ่', level: 92 },
      { icon: '💪', title: 'เสริมภูมิคุ้มกัน', text: 'กระตุ้นการสร้างเม็ดเลือดขาวให้ทำงานได้ดีขึ้น', level: 88 },
      { icon: '🌡', title: 'ลดไข้', text: 'ลดไข้ ขับเหงื่อ ระบายความร้อนในร่างกาย', level: 80 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บช่อดอกหรือผลสุกล้างสะอาด ผึ่งในที่ร่มจนแห้งสนิท แห้งแล้วเก็บใส่โหล ไว้ชงชาดื่มในหน้าหนาว' },
      { title: '🍵 การต้ม', steps: 'นำผลแห้ง 1-2 ช้อนโต๊ะต้มในน้ำร้อนไฟอ่อน ปิดฝาทิ้งไว้ 30 นาที ต้องต้มให้สุก ดื่มบรรเทาอาการหวัดขัดจมูก และลดอาการไข้ให้ลดเร็วขึ้น' },
      { title: '🔨 การบด', steps: 'นำผลที่ตากแห้งมาบดเป็นผง ผสมน้ำดื่มหรือโรยในอาหารเพื่อบำรุงสายตาและหัวใจ' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำเชื่อม: ต้มผลสุกดับกลิ่น กรอกน้ำแล้วผสมน้ำผึ้งแท้ จิบวันละ 1-2 ช้อนโต๊ะ ป้องกันไข้หวัด\nดองเหล้า: แช่ผลแห้งในแอลกอฮอล 4-6 สัปดาห์ หยดผสมน้ำ 5-10 หยด เพื่อกระตุ้นระบบน้ำเหลือง' },
    ],
    warning: 'ห้ามกินดิบเพราะมีสารประกอบไซยาไนท์ อาจทำให้เกิดอาการคลื่นไส้อาเจียน ผู้มีโรคภูมิแพ้ตัวเองต้องระวัง'
  },
  {
    id: 'goldenCurrant',
    nameEn: 'Golden Currant',
    nameTh: 'โกลเดนเคอแรนท์',
    icon: '🌼',
    iconBg: 'linear-gradient(135deg,#2a1a00,#4a3000)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ต้านการอักเสบ', 'บำรุงโลหิต', 'immune'],
    filterTags: ['berry', 'cool', 'immune'],
    description: 'ดอกสีเหลืองทองกลิ่นหอมหวานคล้ายวนิลา ดอกของมันสามารถนำไปทำเป็นเทียนหอม รวมถึงสารปรุงแต่งได้ ผลทรงกลม เมื่อสุกจะมีสีต่างตามสายพันธุ์ เหลือง ส้ม แดง ดำสนิท รสชาติหวานอมเปรี้ยวจัด',
    properties: [
      { icon: '🔥', title: 'ต้านการอักเสบ', text: 'ช่วยลดการอักเสบในลำคอ บำรุงโลหิต เสริมเม็ดเลือดแดงและป้องกันหวัด', level: 78 },
      { icon: '🌿', title: 'ระบายท้อง', text: 'ผลมีกากใยสูง ระบายท้องอย่างอ่อน ระบายสารพิษในไตได้', level: 65 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บผลสุกที่สมบูรณ์หรือใบอ่อน ผึ่งในที่ร่มจนใบแห้งกรอบ ส่วนผลอบในอุณหภูมิต่ำจนแห้งหนึบคล้ายลูกเกด เก็บวิตามิน C' },
      { title: '🍵 การต้ม', steps: 'นำเปลือกต้นหรือรากแห้ง 5 กรัม ต้มในน้ำสะอาด หรือใช้ผลแห้งชงในน้ำสะอาด ขับปัสสาวะ แก้ร้อนใน ลดอาการบวมน้ำ สูตรง่าย: ผสมมินต์ป่า ใช้ใบ 1 หยิบมือ ชงปิดฝา 5-10 นาที แล้วผสม Golden Currant 5 กรัม' },
      { title: '🔨 การบด', steps: 'นำใบแห้งหรือเปลือกต้นที่ตากจนกรอบสนิท มาตำบดให้เป็นผงละเอียด ใช้เปลือกผสมน้ำพอกบริเวณที่เป็นฝีเพื่อดูดหนอง หรือลดบวม' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำเชื่อม: เคี่ยวผลสุกกับน้ำตาลหรือน้ำผึ้งจนข้น จิบเพื่อแก้เจ็บคอ ขับเสมหะ เสริมภูมิคุ้มกันในเด็ก\nดองเหล้า: แช่เปลือกต้น หรือราก 4-6 สัปดาห์ กระตุ้นการไหลเวียนโลหิต และบำรุงไต' },
    ],
    warning: 'ผลดิบหรือยังไม่สุก รวมถึงผลสีดำ มีกรดสูง อาจระคายเคืองกระเพาะอาหาร หากทานผลสุกมากเกินไปจะทำให้ท้องเสีย'
  },
  {
    id: 'evergreenHuckleberry',
    nameEn: 'Evergreen Huckleberry',
    nameTh: 'เอเวอร์กรีนฮัคเคิลเบอร์รี',
    icon: '🍵',
    iconBg: 'linear-gradient(135deg,#0a1a0a,#1a3010)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ควบคุมน้ำตาล', 'บำรุงสายตา', 'immune'],
    filterTags: ['berry', 'cool', 'immune'],
    description: 'มีความทนทานและใบเขียวตลอดปี ผลทรงกลมขนาดเล็กประมาณ 6-10 มม. เมื่อสุกจะมีสีดำหรือม่วงเข้มจัด ผิวเป็นมันวาว รสหวานอมเปรี้ยว ใบหนาแข็ง ผิวเป็นมันเงา ขอบใบหยักละเอียดรูปไข่ ไม่ผลัดใบ',
    properties: [
      { icon: '🩸', title: 'ควบคุมน้ำตาล', text: 'ใบช่วยควบคุมน้ำตาลในเลือด ต้านอนุมูลอิสระ', level: 80 },
      { icon: '👁', title: 'บำรุงสายตา', text: 'ผลมีสารไซโทไซยานิน ช่วยบำรุงสายตา บำรุงเซลล์สมอง', level: 85 },
      { icon: '🩹', title: 'สมานเนื้อเยื่อ', text: 'ใบมีสารเทนนิน ช่วยหยุดอาการท้องเสียและสมานแผล', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บใบตากที่สมบูรณ์หรือผลสุก ตากในที่ร่มจนแห้งกรอบ ผลอบให้หนึบ เก็บโหลชงชาดื่ม ช่วยควบคุมน้ำตาลหรือบำรุงสายตา' },
      { title: '🍵 การต้ม', steps: 'นำใบแห้ง 1-2 ช้อนโต๊ะ ด้วยไฟอ่อน 10-15 นาที ลดระดับน้ำตาลในเลือด หรือกรวกปากรักษาร้อนใน' },
      { title: '🔨 การบด', steps: 'นำใบแห้งกรอบ บดให้เป็นผงเนื้อนวล ใช้ผงชงดื่มเพื่อแก้ท้องเสีย ผสมน้ำสะอาดเพื่อพอกผิวหนังเพื่อลดอักเสบผิวหนัง' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ใบหรือผลแห้ง ทิ้งไว้ 4-6 สัปดาห์\nดองน้ำผึ้ง: แช่ผลสุกในน้ำผึ้งแท้ ทิ้งไว้ในที่มืด 2 สัปดาห์ ใช้จิบแก้ไอบำรุงปอด เสริมภูมิคุ้มกัน' },
    ],
    warning: 'ถ้าทานร่วมยาแผนปัจจุบัน เช่นยาปฏิชีวนะ ห้ามให้ทานร่วมเด็ดขาด เลือกเอาอย่างใดอย่างหนึ่ง'
  },
  {
    id: 'parasol',
    nameEn: 'Parasol Mushroom',
    nameTh: 'เห็ดร่ม',
    icon: '🍄',
    iconBg: 'linear-gradient(135deg,#2a1a0a,#4a3018)',
    categories: ['mushroom', 'neutral'],
    effect: 'neutral',
    effectLabel: 'ฤทธิ์กลาง',
    tags: ['เสริมภูมิคุ้มกัน', 'โปรตีนสูง', 'immune'],
    filterTags: ['mushroom', 'neutral', 'immune'],
    description: 'หมวกเห็ดเมื่อยังอ่อนเป็นรูปไข่สีน้ำตาล เมื่อบานออกจะกว้างได้ถึง 10-40 ซม. พื้นผิวมีเกล็ดสีน้ำตาลเข้มกระจายบนพื้นสีขาวหรือครีม ก้านสูงยาว 20-40 ซม. ลายคล้ายหนังงู มีวงแหวนลวดลายตรงก้านที่เลื่อนขึ้นลงได้',
    properties: [
      { icon: '💪', title: 'เสริมภูมิคุ้มกัน', text: 'ปกป้องเซลล์จากการถูกทำลาย ยับยั้งแบคทีเรียบางชนิด', level: 80 },
      { icon: '🥩', title: 'โปรตีนสูง', text: 'มีโปรตีน วิตามินสูง เหมาะสำหรับผสมตัวยาอื่นๆ', level: 88 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'หั่นหมวกเห็ดเป็นแผ่นบาง วางเรียงตะแกรง ตากแดดจนแห้ง แยกหมวกเห็ดกับก้าน เมื่อแห้งแล้วให้เก็บใส่โหลแยกกัน' },
      { title: '🍵 การต้ม', steps: 'นำเห็ดสดหรือแห้งต้มในน้ำสะอาดจนเดือด ก้านเคี่ยวให้นานที่สุด (เพื่อดึงตัวยาออกมา) แล้วตัดทิ้ง 15 นาที กินเฉพาะหมวกเห็ด' },
      { title: '🔨 การบด', steps: 'นำก้านหรือหมวกเห็ดที่ตากแห้งมาบดจนละเอียดจนเนื้อนวล ใช้เป็นผงนัว ชงหรือต้มดื่ม เหมาะสำหรับผสมกับยาตัวอื่นๆ' },
    ],
    warning: 'ผ่านความร้อนให้สุกก่อนรับประทานเสมอ ทั้งเห็ดดิบและที่ตากแล้ว (ผสมเป็นผงแล้วต้มหรือชง)'
  },
  {
    id: 'americanGinseng',
    nameEn: 'American Ginseng',
    nameTh: 'โสมอเมริกัน',
    icon: '🌱',
    iconBg: 'linear-gradient(135deg,#0a2a0a,#183018)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['บำรุงประสาท', 'ปรับสมดุล', 'ควบคุมน้ำตาล'],
    filterTags: ['herb', 'cool', 'digestive'],
    description: 'มีราก ลำต้น ใบ ดอกและผล รากอวบสีเหลืองอ่อน มีรอยย่นตามขวาง รูปร่างคล้ายคน สั้นและป้อมกว่าโสมตะวันออก เป็นพืชล้มลุกสูง 20-50 ซม. ใบมี 5 แฉก ผลสุกสีแดงสดมีเมล็ด',
    properties: [
      { icon: '🧠', title: 'บำรุงประสาท', text: 'เสริมความจำและผ่อนคลาย ปรับสมดุลร่างกายช่วยให้ทนต่อความเครียดและเหนื่อยล้า', level: 85 },
      { icon: '🫁', title: 'บำรุงปอด', text: 'ระบายความร้อนในร่างกาย บรรเทาอาการคอแห้งกระหายน้ำ', level: 78 },
      { icon: '🩸', title: 'ควบคุมน้ำตาล', text: 'ควบคุมน้ำตาลในเลือด ปรับอินซูลินในเลือด', level: 82 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ล้างรากให้สะอาดหั่นแผ่นบาง ตากแห้งจนสนิทหรืออบอุณหภูมิต่ำจนกรอบ เก็บใส่ภาชนะแห้งสนิทจะเก็บได้เป็นปี' },
      { title: '🍵 การต้ม', steps: 'นำโสมแห้ง 3-5 แผ่นต้มในน้ำสะอาดด้วยไฟอ่อน 15 นาที ใส่ถ้วยปิดฝาจนน้ำเดือด น้ำซุปช่วยบำรุงปอด บรรเทาอาการอ่อนเพลีย โดยไม่เพิ่มความดันหรือทำให้ตัวร้อน' },
      { title: '🔨 การบด', steps: 'นำรากที่ตากจนแห้งสนิทมาบดจนแห้งละเอียด ใช้ผงโสมชงดื่มด้วยน้ำอุ่น หรือบรรจุเม็ดยา เสริมความจำ ปรับสมดุลน้ำตาลในเลือด' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: นำโสมแห้งแช่แอลกอฮอล (วอดก้า) ทิ้งไว้ 4-6 สัปดาห์ ใช้หยดผสมน้ำดื่ม สำหรับผู้ป่วยเสียเลือด\nดองน้ำผึ้ง: แช่โสมสดหรือแห้งในน้ำผึ้งแท้ 2-4 สัปดาห์ ใช้จิบแก้ไอ' },
    ],
    warning: 'ห้ามใช้กับผู้ป่วยที่มีสภาวะเสียเลือดเยอะ (ถึงขั้นสาหัส) ปริมาณมากอาจจะเป็นยานอนหลับ'
  },
  {
    id: 'wildMint',
    nameEn: 'Wild Mint',
    nameTh: 'มินต์ป่า',
    icon: '🌿',
    iconBg: 'linear-gradient(135deg,#052a10,#0a4018)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ขับลม', 'แก้หวัด', 'digestive'],
    filterTags: ['herb', 'cool', 'digestive'],
    description: 'ลำต้นเป็นเหลี่ยมสี่มุม ตามข้อมีขนอ่อนๆ เลื้อยไปตามดิน ชูยอดตั้งตรง สูงประมาณ 10-60 ซม. ใบรูปไข่หรือรีขอบใบหยักฟันเลื่อย ส่งกลิ่นหอมเย็นจัดเมื่อขยี้ ดอกกระจุกกลมตามซอกใบ',
    properties: [
      { icon: '💨', title: 'ขับลม', text: 'ขับลมและช่วยย่อย แก้ท้องอืดท้องเฟ้อ กระตุ้นการทำงานของลำไส้', level: 88 },
      { icon: '🤧', title: 'แก้หวัด', text: 'บรรเทาอาการหวัด ลดการขัดจมูก บรรเทาอาการไอ', level: 82 },
      { icon: '🦠', title: 'ฆ่าเชื้อ', text: 'แก้ปวดลดการอักเสบ ฆ่าเชื้อแบคทีเรีย ลดอาการปวดหัว', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บกิ่งที่มีใบสมบูรณ์ ล้างสะอาดและเก็บในร่มมีลมโกรก (ห้ามตากแดดจัด เพื่อเก็บน้ำมันหอมระเหย)' },
      { title: '🍵 การชง', steps: 'ใช้ทั้งใบสดหรือแห้งก็ได้ ชงในน้ำร้อน (ไม่ร้อนจัด) ปิดฝาทิ้งไว้ 5 นาที ดื่มขับลม ฆ่าเชื้อในปากได้เมื่อเย็นแล้ว' },
      { title: '🔨 การบด', steps: 'บดใบแห้งให้หยาบ ใส่โหลปิดสนิท ใช้ชงชาดื่มแก้หวัด บรรเทาอาการไอ บดละเอียดจนมีน้ำมันออกมา ใช้พอกบริเวณขมับแก้อาการปวดหัว' },
    ],
    warning: 'ไม่ควรใช้น้ำมันถูบริเวณจมูก อาจทำให้เส้นประสาทแตกทำให้เลือดกำเดาไหล'
  },
  {
    id: 'yarrow',
    nameEn: 'Yarrow',
    nameTh: 'ยาโรว์',
    icon: '🌸',
    iconBg: 'linear-gradient(135deg,#2a1a1a,#3a0a20)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ห้ามเลือด', 'ลดไข้', 'wound'],
    filterTags: ['herb', 'cool', 'wound'],
    description: 'ดอกออกเป็นช่อกระจุกแน่นที่ปลายยอด ดอกย่อยมีขนาดเล็กจิ๋ว ส่วนใหญ่มีสีขาวหรือชมพูอ่อน ใบมีลักษณะเด่น คือใบหยักละเอียดซอย คล้ายขนนกหรือใบผักชี มีเหง้าใต้ดินที่แพร่ขยายได้รวดเร็ว',
    properties: [
      { icon: '🩸', title: 'ห้ามเลือด', text: 'เด่นที่สุด ใช้รักษาแผลสด และห้ามเลือดได้แบบชะงักเลย รวมถึงยังฆ่าเชื้อในบาดแผลได้', level: 95 },
      { icon: '🌡', title: 'ลดไข้', text: 'ช่วยระบบทางเดินอาหาร ช่วยเจริญอาหาร ขับลม และลดการเกร็งของลำไส้', level: 80 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บส่วนดอกและใบในช่วงที่ดอกเริ่มบาน ล้างให้สะอาด พึ่งในที่ร่มที่แห้งสนิท ห้ามตากแดด เมื่อแห้งเก็บใส่โหลปิดสนิท' },
      { title: '🍵 การต้ม', steps: 'นำใบและดอกแห้ง 1-2 ช้อนชา ชงในน้ำร้อนหรือต้มไฟอ่อนประมาณ 5 นาที ปิดฝาเพื่อกันตัวยาระเหย จะช่วยลดไข้ ขับเหงื่อ และช่วยระบบย่อยอาหาร' },
      { title: '🔨 การบด', steps: 'นำใบสด มาขยี้หรือตำในโกร่งบดยาจนแหลก ใช้ใบสดบด พอก หรือขยี้ บริเวณแผลที่มีเลือดไหล จะทำการห้ามเลือดหรือหยุดเลือดในทันที' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ดอกและใบแห้งในแอลกอฮอล์ ทิ้งไว้ 4-6 สัปดาห์ ใช้หยดผสมน้ำดื่ม 5-10 หยด เพื่อปรับสมดุลรอบเดือน\nดองน้ำมัน: แช่สมุนไพรแห้งในน้ำมันมะกอก ทิ้งไว้ในที่อุ่น 2-4 สัปดาห์' },
    ],
    warning: 'ไม่ควรดื่มหรือทาติดต่อเป็นเวลานานไป เพราะจะทำให้ผิวไวต่อแสงแดดได้'
  },
  {
    id: 'creepingThyme',
    nameEn: 'Creeping Thyme',
    nameTh: 'ไทม์เลื้อย',
    icon: '🌺',
    iconBg: 'linear-gradient(135deg,#2a0a0a,#4a1a1a)',
    categories: ['herb', 'hot'],
    effect: 'hot',
    effectLabel: 'ฤทธิ์ร้อน',
    tags: ['ต้านเชื้อ', 'แก้ไอ', 'digestive'],
    filterTags: ['herb', 'hot', 'digestive'],
    description: 'ทามไม้เลื้อย พืชคลุมดินตระกูลเดียวกับมินต์ ลำต้นเป็นไม้พุ่มเตี้ยเลื้อยไปตามดิน สูง 2-10 ซม. ใบมีขนาดเล็กมากรูปไข่หรือรี ขยี้แล้วจะมีกลิ่นหอม ดอกออกเป็นช่อกระจุกที่ปลายยอด สีชมพูเข้ม ม่วงหรือขาว',
    properties: [
      { icon: '🦠', title: 'ต้านเชื้อ', text: 'ยับยั้งแบคทีเรียและเชื้อรา แก้ไอขับเสมหะ บรรเทาอาการหลอดลมอักเสบ', level: 85 },
      { icon: '💨', title: 'ย่อยอาหาร', text: 'ลดท้องอืดท้องเฟ้อ ขับลม กลิ่นหอมระเหยลดความเครียดทำให้นอนหลับสบาย', level: 80 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ใช้สมุนไพรที่เป็นช่อกระจุกปลายยอดหนึ่งส่วน รากหนึ่งส่วน ตากในแดนร่มลมโกรก (ห้ามแดดจัด) เก็บใส่ภาชนะแยกดอกและราก' },
      { title: '🍵 การชง', steps: 'นิยมใช้การชงมากกว่าต้มเพื่อไม่ให้ความร้อนทำลายตัวยา ชงบรรเทาอักเสบ ช่วยย่อยอาหาร' },
      { title: '🔨 การบดและพอก', steps: 'นำดอกไม้แห้งบดละเอียดจนเป็นผงผสมน้ำอุ่น ใช้พอกบริเวณแผลเก่า' },
      { title: '🍾 การสกัด', steps: 'สกัดน้ำมัน: ใช้รากแห้งบดเป็นผงผสมน้ำมันงาหรือแอพพริคอต ดองในไหเก็บในที่มืด 4 สัปดาห์ นำมาทาบริเวณที่ขึ้นเชื้อรา' },
    ],
    warning: 'ไม่ควรใช้ต่อเนื่องเป็นเวลานาน อาจมีอาการแพ้ ผื่นขึ้น คลื่นไส้'
  },
  {
    id: 'chanterelle',
    nameEn: 'Chanterelle',
    nameTh: 'เห็ดแชนเทอเรล',
    icon: '🍄',
    iconBg: 'linear-gradient(135deg,#2a1800,#4a2c00)',
    categories: ['mushroom', 'neutral'],
    effect: 'neutral',
    effectLabel: 'ฤทธิ์กลาง',
    tags: ['บำรุงสายตา', 'เสริมกระดูก', 'immune'],
    filterTags: ['mushroom', 'neutral', 'immune'],
    description: 'ดอกมีรูปร่างคล้ายกรวยหรือแตร ขอบดอกหยักเป็นคลื่น สีเหลืองทองหรือเหลืองส้มสดใส เนื้อเห็ดแน่นขาวนวล ครีปดอกไม่เป็นแผ่นบางแต่เป็นสันนูน วิ่งยาวลงไปจนถึงก้านดอก ขึ้นตามพื้นดินในป่าสนหรือป่าผลัดใบ',
    properties: [
      { icon: '👁', title: 'บำรุงสายตา', text: 'มีวิตามินเอและเบต้าแคโรทีนสูง บำรุงสายตาและป้องกันโรคตาบอดตอนกลางคืน', level: 88 },
      { icon: '🦴', title: 'เสริมสร้างกระดูก', text: 'เป็นแหล่งธรรมชาติของวิตามินดี ช่วยในการดูดซึมแคลเซียม', level: 82 },
      { icon: '🛡', title: 'ต้านอนุมูลอิสระ', text: 'มีสารโพลีซคาไรด์ เสริมสร้างระบบภูมิคุ้มกันและลดการอักเสบ', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ทำความสะอาด (ห้ามให้เปียกโชก) ใช้แปรงค่อยๆ ปัดเบาๆ หั่นเป็นชิ้นหนา ผึ่งในที่แห้งมีลมโกรก หรืออบในอุณหภูมิต่ำ เก็บในโหลมิดชิด ระวังขึ้นรา' },
      { title: '🍵 การต้ม', steps: 'นำเห็ดสดหรือแห้งมาต้มทำเป็นน้ำสต็อกหรือปรุงเป็นซุปเห็ด' },
      { title: '🔨 การบด', steps: 'นำเห็ดแห้งสนิทมาบดเป็นผงละเอียด ใช้เป็นเครื่องโรยหน้าในอาหารเพื่อเพิ่มกลิ่นหอม' },
    ],
    warning: 'ระวังเห็ดพิษลักษณะคล้ายกันอย่าง Jack\'o Lantern ซึ่งขึ้นบนขอนไม้และมีครีบดอกเป็นแผ่นบาง'
  },
  {
    id: 'cardinalFlower',
    nameEn: 'Cardinal Flower',
    nameTh: 'ดอกการ์ดินัล',
    icon: '🌹',
    iconBg: 'linear-gradient(135deg,#2a0000,#4a0808)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น+กลาง',
    tags: ['ขยายหลอดลม', 'เลิกบุหรี่', 'อันตราย'],
    filterTags: ['herb', 'cool'],
    description: 'พืชสมุนไพรที่โดดเด่นเรื่องความสวยงามและสรรพคุณทางยาที่รุนแรง ดอกออกเป็นช่อกระเปาะยาวตามลำต้น สีแดงสดจัดคล้ายชุดของพระคาร์ดินัล กลีบดอกมีรูปร่างเฉพาะตัว มีน้ำยางสีขาวซึมออกมาเมื่อหักลำต้น',
    properties: [
      { icon: '🫁', title: 'ขยายหลอดลม', text: 'มีสารโรเวลีนช่วยขยายหลอดลม กระตุ้นการหายใจและขับเสมหะอย่างแรง', level: 90 },
      { icon: '🚬', title: 'ถอนนิโคตีน', text: 'สารกัดถูกใช้เพื่อระงับการอยากบุหรี่ มีฤทธิ์ต่อตัวรับในสมองคล้ายนิโคตีนแต่ไม่ทำให้เสพติดเท่า', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บใบและดอก ล้างสะอาด แขวนในที่ร่ม มีลมโกรกและมืดสนิท ป้องกันไม่ให้แสงทำลายสารอัลคาลอยด์' },
      { title: '🍵 การต้ม', steps: 'ใช้ใบแห้ง 1-2 ช้อนชา ต้มในน้ำร้อนเพื่อจิบช่วยขับเสมหะ ขยายหลอดลม (ใช้ปริมาณน้อย)' },
      { title: '🍾 การสกัด', steps: 'ใช้ดอกแช่ในแอลกอฮอล ทิ้งไว้ 2-4 สัปดาห์ ใช้ 1-3 หยดผสมน้ำดื่มผ่อนคลาย ระงับอาการไอ' },
    ],
    warning: '⚠️ ใช้เกินขนาดอาจทำให้คลื่นไส้ อาเจียนรุนแรง ตัวสั่น เหงื่อออก ถึงขั้นระบบหายใจล้มเหลว ต้องระวังอัตราส่วนมาก'
  },
  {
    id: 'burdock',
    nameEn: 'Burdock Root',
    nameTh: 'รากโกโบ',
    icon: '🌱',
    iconBg: 'linear-gradient(135deg,#1a0a00,#301800)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ฟอกเลือด', 'รักษาผิวหนัง', 'wound'],
    filterTags: ['herb', 'cool', 'wound'],
    description: 'รากเป็นส่วนใช้ยาหลัก แก้วเรียงยาวเหมือนแท่งไม้ ยาวได้ถึง 60-100 ซม. เปลือกนอกสีน้ำตาลเข้มหรือเทา เนื้อในสีขาวนวล ผิวสัมผัสกรอบ ใบมีขนาดใหญ่รูปหัวใจ ขอบใบหยักเป็นคลื่น ดอกเป็นช่อกระจุกสีม่วงแดง',
    properties: [
      { icon: '🩸', title: 'ฟอกเลือด', text: 'ช่วยขับสารพิษออกจากกระแสเลือด และกระตุ้นน้ำเหลือง', level: 85 },
      { icon: '🧴', title: 'รักษาผิวหนัง', text: 'บรรเทาอาการผื่นคัน สิวอักเสบ และโรคผิวหนังเรื้อรัง', level: 80 },
      { icon: '🫀', title: 'บำรุงตับ', text: 'มีใยอาหารสูง ช่วยระบบย่อยอาหารและบำรุงตับ', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ล้างรากให้สะอาดหั่นเป็นแผ่นบางหรือเส้นยาว ตากแดดจัดจนแห้งสนิท หรืออบจนกรอบ แห้งแล้วเก็บใส่ภาชนะปิดสนิท ใช้ชงชาดื่มเพื่อล้างพิษในเลือด' },
      { title: '🍵 การต้ม', steps: 'นำรากแห้งประมาณ 5-10 กรัม ต้มในน้ำสะอาดด้วยไฟอ่อน 30 นาที รากมีความแข็งต้องใช้เวลาเคี่ยวนาน ดื่มซุปจะช่วยขับปัสสาวะ และร้อนใน และบำรุงระบบย่อยอาหาร' },
      { title: '🔨 การบด', steps: 'นำรากที่ตากจนแห้งสนิทและกรอบ มาตำหรือบดให้เป็นผงละเอียดจนนวล ใช้ผงผสมน้ำอุ่นพอกบริเวณที่เป็นสิวหรือผื่นคัน' },
    ],
    warning: 'หากใช้ปริมาณที่มากจะทำให้เกิดสภาวะขาดน้ำที่รุนแรง ให้กินอาหารโจ๊กหรือ Parasol Mushroom ร่วม'
  },

  // ─── NEW BERRIES ────────────────────────────────────────────
  {
    id: 'creekplum',
    nameEn: 'Creekplum',
    nameTh: 'ครีกพลัม',
    icon: '🍑',
    iconBg: 'linear-gradient(135deg,#2a0a00,#4a1800)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['สมานแผล', 'แก้ไอ', 'wound'],
    filterTags: ['berry', 'cool', 'wound', 'digestive'],
    description: 'ผลทรงกลมขนาดเล็ก ผิวเรียบเป็นมัน สีส้มอมแดง ผลแดงเข้มเมื่อสุก มีนวลสีขาว เนื้อสีเหลืองรสชาติเปรี้ยวจัด แต่มีความหวานผสมอยู่ มีเมล็ดเดี่ยวแข็งอยู่กลางผล ดอกสีขาว 5 กลีบ ออกเป็นช่อกระจุก กลิ่นหอมอ่อนๆ ลำต้นเป็นไม้พุ่มแตกกิ่งหนาแน่น กิ่งอาจมีหนามแหลมสั้นๆ',
    properties: [
      { icon: '🩹', title: 'สมานแผลและห้ามเลือด', text: 'เปลือกมีสารแทนนินสูง ช่วยห้ามเลือดและสมานแผล คล้ายยาโรว์แต่ใช้เป็นส่วนผสมเสริม', level: 65 },
      { icon: '🫁', title: 'ระบบทางเดินหายใจ', text: 'น้ำต้มจากเปลือกและรากช่วยบรรเทาอาการไอเจ็บคอ', level: 72 },
      { icon: '💨', title: 'ระบบทางเดินอาหาร', text: 'ผลสุกช่วยระบายท้อง เปลือกต้นช่วยหยุดอาการท้องเสีย ผสมกับเห็ด Parasol ทำซุปได้', level: 68 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บผลสุกแยกเมล็ดออก เก็บเปลือกต้นและใบล้างสะอาด ผึ่งในที่ร่มจนแห้งสนิท เปลือกตากให้แห้งกรอบ เก็บในโหลปิดมิดชิด ไม่ให้โดนแสง เพื่อถนอมสารกลุ่มฟลาโวนอยด์ ไว้ชงแก้ไอ สมานแผลในปาก' },
      { title: '🍵 การต้ม', steps: 'เปลือกแห้ง 5-10 กรัม ต้มด้วยไฟอ่อน 15-20 นาที ดื่มบรรเทาอาการไอเรื้อรัง หรือกลั้วปากรักษาความสะอาดปาก คล้ายของ Wild Mint' },
      { title: '🔨 การบด', steps: 'นำเปลือกแห้งบด ผสมน้ำเพื่อพอกผิวหนัง หรือใช้พอกเพื่อห้ามเลือด ไม่เหมาะแทนยาโรว์ เอาไว้ผสมยาอื่นๆ ดีกว่า' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำเชื่อม: เคี่ยวผลสุกกับน้ำผึ้งจนงวด จิบเพื่อขับเสมหะ ระบายท้อง เหมาะสำหรับไอเรื้อรัง\nดองเหล้า: แช่เปลือกทิ้งไว้ 4-6 สัปดาห์ ใช้หยดผสมน้ำดื่มเพื่อกระตุ้นการเจริญอาหาร บำรุงธาตุ เหมาะผู้ป่วยพักฟื้น' },
    ],
    warning: 'ห้ามเคี้ยวเมล็ดหรือใบ เพราะมีสารรีโซไซ อาจทำให้เกิดพิษไซยาไนด์ ผลสุกมากเกินไปทำให้ท้องเสีย'
  },
  {
    id: 'redRaspberry',
    nameEn: 'Red Raspberry',
    nameTh: 'ราสพ์เบอร์รีแดง',
    icon: '🍓',
    iconBg: 'linear-gradient(135deg,#2a0010,#4a0820)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['บำรุงเลือด', 'บำรุงมดลูก', 'wound', 'immune'],
    filterTags: ['berry', 'cool', 'wound', 'immune'],
    description: 'ผลกลุ่มสีแดงสดทรงกรวยตั้ง รสชาติหวานอมเปรี้ยว กลิ่นหอมเป็นเอกลักษณ์ ใบประกอบมี 3-5 ใบย่อยขอบฟันเลื่อย ลำต้นไม้พุ่มมีหนามแหลมละเอียด ควรสวมถุงมือเมื่อเก็บ',
    properties: [
      { icon: '🩸', title: 'บำรุงเลือดและมดลูก', text: 'ใบมีสารสกีน ปรับสมดุลกล้ามเนื้อมดลูก บำรุงเลือด ดื่มเพื่อบำรุงมดลูกและลดปวดประจำเดือน', level: 82 },
      { icon: '🦷', title: 'รักษาแผลในปาก', text: 'วิตามินสูง ช่วยรักษาแผลในปาก ปัญหาเหงือก', level: 75 },
      { icon: '🛡', title: 'ต้านอนุมูลอิสระ', text: 'สารคีโตนต้านอนุมูลอิสระ ลดไขมันส่วนเกิน', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บใบอ่อนที่สมบูรณ์ ล้างสะอาดผึ่งในร่มจนใบแห้งกรอบ (ดื่มบำรุงมดลูก ลดปวดประจำเดือน) ผลนำไปอบในอุณหภูมิต่ำจนแห้งสนิท' },
      { title: '🍵 การต้ม', steps: 'นำใบแห้ง 1-2 ช้อนโต๊ะต้มในน้ำเดือดอ่อนๆ 10 นาที ดื่มแก้ท้องเสีย กลั้วปากแก้ร้อนใน' },
      { title: '🔨 การบด', steps: 'นำใบแห้งกรอบสนิทบดจนละเอียดเนื้อนวล ผสมน้ำสะอาดพอกเพื่อลดอักเสบ' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: ใช้ใบแห้งแช่แอลกอฮอล ทิ้งไว้ 4-6 สัปดาห์ หยดผสมน้ำดื่มกระตุ้นการไหลเวียนของเลือด\nดองน้ำผึ้ง: แช่ผลสุกในน้ำผึ้งทิ้งไว้ในที่มืด 2 สัปดาห์ แก้ไอ บำรุงปอด แก้เจ็บคอ' },
    ],
    warning: 'การดื่มเข้มข้นเกินไปอาจทำให้เกิดอาการคลื่นไส้แก่ผู้ที่กระเพาะบาง'
  },
  {
    id: 'evergreenBerry',
    nameEn: 'Evergreen Berry',
    nameTh: 'เอเวอร์กรีนเบอร์รี',
    icon: '🌲',
    iconBg: 'linear-gradient(135deg,#052010,#0a3818)',
    categories: ['berry', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['แก้ปวด', 'ต้านเชื้อ', 'wound', 'digestive'],
    filterTags: ['berry', 'cool', 'wound', 'digestive'],
    description: 'ผลทรงกลมสีแดงสดขนาดประมาณ 6-10 มม. เนื้อในมีลักษณะคล้ายแป้ง รสชาติหวานเล็กน้อย กลิ่นหอมแรง ใบหนาแข็งเป็นมันเงาเขียวตลอดปี รูปไข่ขอบใบหยักมน เมื่อขยี้จะมีกลิ่นหอมเย็นชัดเจน ต้นเป็นไม้พุ่มเตี้ยแพร่คลุมดิน สูงประมาณ 10-15 ซม.',
    properties: [
      { icon: '💊', title: 'แก้ปวดอักเสบ', text: 'มีสารเมทีนซิลิไซด์คล้ายแอสไพรินธรรมชาติ ช่วยลดปวดกล้ามเนื้อและไขข้อ', level: 85 },
      { icon: '🌬', title: 'ขยายหลอดลม', text: 'กลิ่นหอมเย็นช่วยขยายหลอดลม บรรเทาอาการขัดจมูกและหวัด', level: 80 },
      { icon: '🦠', title: 'ฆ่าเชื้อ', text: 'มีฤทธิ์ฆ่าเชื้อแบคทีเรียในช่องปากและผิวหนัง ขับลม แก้ท้องอืดเกร็ง', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บใบแก่ที่สมบูรณ์ล้างสะอาด ผึ่งในที่ร่มมีลมโกรกแห้งสนิท ห้ามตากแดดจัด เก็บในโหลแก้วปิดสนิท' },
      { title: '🍵 การต้ม', steps: 'นำใบแห้ง 1 ช้อนโต๊ะต้มในน้ำร้อนไม่เดือด ปิดฝาทิ้งไว้ 10 นาที ดื่มแก้ปวดตามข้อ กลั้วปากฆ่าเชื้อลดกลิ่นปาก' },
      { title: '🔨 การบด', steps: 'นำใบสดหรือแห้งบดละเอียดจนหอมฟุ้งเพื่อเอาน้ำมันหอมระเหย ใช้ใบสดพอกตามกล้ามเนื้อ ถูทั่วตัว หรือผสมน้ำพอกผิวหนังที่คันจากแมลงกัดต่อย เพื่อลดบวม' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำมัน: แช่ใบแห้งในน้ำมันมะกอก 4-6 สัปดาห์ ใช้ทาถูนวดแก้ปวดเมื่อย\nดองเหล้า: แช่ใบแห้งในแอลกอฮอล 4-6 สัปดาห์ หยดผสมน้ำจิบแก้ท้องเกร็ง ขับลม' },
    ],
    warning: '⚠ ห้ามดื่มน้ำมันสกัดโดยตรง สรรพคุณรุนแรงถึงตาย ผู้แพ้ยาปฏิชีวนะควรหลีกเลี่ยง ไม่ควรใช้พอกหรือดมใต้จมูก อาจทำให้ลมหายใจติดขัด'
  },

  // ─── NEW HERBS ──────────────────────────────────────────────
  {
    id: 'bulrush',
    nameEn: 'Common Bulrush',
    nameTh: 'กกธูป',
    icon: '🌾',
    iconBg: 'linear-gradient(135deg,#1a2800,#2a4008)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ห้ามเลือด', 'ขับปัสสาวะ', 'wound'],
    filterTags: ['herb', 'cool', 'wound'],
    description: 'พืชล้มลุกอายุหลายปี มีเหง้าใต้ดิน ลำต้นสูงได้ 1.5-3 ม. ใบแบนยาวคล้ายดาบ สีเขียวอมเทา กว้าง 2-4 ซม. ดอกมีลักษณะเป็นแท่งทรงกระบอกคล้ายหางแมว แบ่งสองส่วนชัดเจน ส่วนบนดอกตัวผู้เรียวสีเหลือง ส่วนล่างดอกตัวเมียหนาคล้ายไส้กรอก',
    properties: [
      { icon: '🩸', title: 'ห้ามเลือด', text: 'ละอองเรณูมีฤทธิ์ช่วยห้ามเลือด ลดกำเดาไหล', level: 85 },
      { icon: '💧', title: 'ขับปัสสาวะ', text: 'เหง้าและรากมีฤทธิ์เย็น ช่วยขับปัสสาวะลดบวมน้ำ แก้ปวดท้องน้อยประจำเดือน', level: 80 },
      { icon: '🔥', title: 'แผลไฟไหม้', text: 'ใช้พอกแก้แผลไฟไหม้ น้ำร้อนลวก ลดการอักเสบ', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'นำส่วนดอกละอองเรณูมาตากแดดให้แห้งสนิทบนผ้าสะอาดและเคาะละอองผงสีเหลือง เหง้าหรือรากล้างให้สะอาดปอกเปลือก เก็บใส่ภาชนะแยกดอกและราก เก็บในที่แห้งและเย็นเท่านั้น' },
      { title: '🍵 การต้ม/ชง', steps: 'ผงละอองเรณูใช้โรยแผลสดลดอาการอักเสบ เป็นหนอง หรือชงดื่ม 1 ช้อนชา ขับปัสสาวะ ลดบวมน้ำ ปวดท้องน้อย\nนำหน่อ รากและเหง้าล้างสะอาดต้มน้ำเดือด เมื่อดื่มช่วยบรรเทาอาการไข้ ขับปัสสาวะ' },
      { title: '🔨 การบด/พอก', steps: 'ล้างเหง้ารากแล้วนำมาบดจนละเอียด นำมาพอกบริเวณแผลไฟไหม้ ผิวหนังอักเสบ เหง้ากับรากใช้ทำแป้งรับประทานได้ (นำแป้งไปตากแห้งแดดจัดจึงนำมาใช้)' },
    ],
    warning: 'ไม่ควรเก็บในบริเวณน้ำขังหรือสกปรก อาจเป็นพิษ'
  },
  {
    id: 'ramHead',
    nameEn: 'Ram Head Orchid',
    nameTh: 'กล้วยไม้นารีหัวแกะ',
    icon: '🪷',
    iconBg: 'linear-gradient(135deg,#1a0a2a,#320a40)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['ระงับประสาท', 'ลดไข้', 'nerve', 'danger'],
    filterTags: ['herb', 'cool', 'nerve', 'danger'],
    description: 'พืชหายากและมีลักษณะเฉพาะสูง ดอกมีรูปร่างคล้ายกับหัวแกะ กลีบดอกล่างม้วนกลมเป็นถุงสีขาว มีลายเส้นสีแดงม่วงหรือชมพูเข้ม กลีบเลี้ยงด้านบนสีเขียวอมน้ำตาล ใบรูปรีสีเขียวอมน้ำเงิน ออกเรียงสลับกัน เป็นพืชล้มลุกสูงประมาณ 10-30 ซม. มีเหง้าอยู่ใต้ดิน',
    properties: [
      { icon: '🧠', title: 'ระงับประสาท', text: 'ตำรายาพื้นบ้านใช้แก้นอนไม่หลับและอาการกระตุกของกล้ามเนื้อ เหมาะสำหรับกระดูกร้าวเพื่อลดอาการกระตุก', level: 88 },
      { icon: '🌡', title: 'ลดไข้อักเสบ', text: 'ใช้ส่วนเหง้าระบายความร้อนในร่างกาย ลดอักเสบ แนะนำเป็นส่วนผสมของยาชา', level: 75 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'นำส่วนเหง้าและราก ล้างและหั่นเป็นชิ้นเล็กๆ ตากแดดจัดจนแห้งสนิท เก็บในโหลแก้วกระดาษไข อยู่ได้ไม่ถึง 1 ปี 6 เดือนก็เริ่มเสื่อม' },
      { title: '🍵 การต้ม', steps: 'นำรากแห้ง 1-2 กรัม ต้มในน้ำสะอาดด้วยไฟอ่อน 15 นาที ปิดฝากันระเหย เมื่อดื่มคลายเครียด แก้ปวดหัวจากความวิตกกังวล ช่วยลดไข้ สรรพคุณหลักคือการระงับประสาท' },
      { title: '🔨 การบด', steps: 'นำรากที่ตากแดดจนแห้งสนิทบดในโกร่งยาจนละเอียด ใช้ผงรากปริมาณน้อยผสมน้ำอุ่นดื่มต่อ 1 แก้ว หรือผสมน้ำสะอาดพอกบริเวณที่กล้ามเนื้อกระตุกเพื่อลดอาการ เหมาะสำหรับกรณีกระดูกร้าว' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: นำรากแห้งแช่ในแอลกอฮอลเข้มข้น 4-6 สัปดาห์ ดองในที่มืด หยดผสมน้ำเพียงเล็กน้อย เพื่อระงับประสาทอย่างอ่อน กล่อมระบบประสาทให้สงบลง' },
    ],
    warning: '⚠ ตัวนี้มีความเป็นพิษสูง เหมาะสำหรับการปรุงยาเท่านั้น บางคนอาจแพ้ทางผิวหนังเมื่อสัมผัสใบหรือลำต้น หากใช้ในปริมาณมากจะทำให้ง่วงซึมรุนแรง และทำให้รู้สึกชาจนไม่รู้สึกอะไร'
  },
  {
    id: 'chocolateDaisy',
    nameEn: 'Chocolate Daisy',
    nameTh: 'ช็อกโกแลตเดซี่',
    icon: '🌻',
    iconBg: 'linear-gradient(135deg,#2a1000,#4a2000)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น',
    tags: ['คลายเครียด', 'แก้ปวดท้อง', 'nerve', 'digestive'],
    filterTags: ['herb', 'cool', 'nerve', 'digestive'],
    description: 'โดดเด่นด้วยกลิ่นหอมช็อกโกแลตแท้ๆ กลีบดอกสีเหลืองสด ตรงกลางเป็นสีน้ำตาลเข้ม ลักษณะคล้ายดอกเดซี่ทั่วไป ใบเรียวยาวดูสะอาดตา มีกลิ่นหอมเฉพาะตัวที่คล้ายกับ "ช็อกโกแลต" อย่างชัดเจน',
    properties: [
      { icon: '😌', title: 'ลดความเครียด', text: 'กลิ่นหอมช่วยผ่อนคลาย กล่อมประสาทให้หลับสบาย ลดอาการปวดหัวจากความเครียด', level: 80 },
      { icon: '🏊', title: 'แก้ปวดท้องเกร็ง', text: 'ตำราพื้นเมืองใช้รากแก้อาการปวดเกร็งในท้อง เหมาะสำหรับกรณีจมน้ำหรือว่ายน้ำจนปวดเกร็งในท้อง', level: 75 },
      { icon: '🫁', title: 'ระบบทางเดินหายใจ', text: 'ควันจากเผาดอกแห้งสูดดมเพื่อบรรเทาอาการคัดจมูก ขยายหลอดลม', level: 70 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'ล้างดอกให้สะอาด ผึ่งในที่ร่มมีลมโกรก ไม่ตากแดดแห้งจนเกินไป เก็บใส่โหล กระดาษไข และชงดื่มผสมน้ำผึ้ง' },
      { title: '🍵 การต้ม', steps: 'นำรากหรือดอกแห้ง 1-2 กรัมต้มในน้ำสะอาดด้วยไฟอ่อน 10 นาที ปิดฝาหม้อเพื่อกันกลิ่นระเหย เมื่อดื่มช่วยอาการปวดท้องเกร็ง ขับลมในลำไส้' },
      { title: '🔨 การบดและเผา', steps: 'นำดอกแห้งมาบดละเอียดให้เป็นเนื้อนวล\nภายนอก: นำผงดอกแห้งมาเผาเป็นธูปสมุนไพร สูดดมเพื่อขยายหลอดลม นิยมทำเป็นถุงหอมคล้าย Wild Mint\nนำผงผสมน้ำสะอาดพอกผิวหนังลดอาการระคายเคือง สัตว์กัดต่อย แพ้ คัน' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: นำดอกและรากแห้งแช่ในแอลกอฮอล ทิ้งไว้ 4-6 สัปดาห์ ใช้หยดผสมน้ำดื่มปริมาณน้อย กล่อมประสาทให้หลับสบาย และลดอาการปวดหัวจากความเครียด' },
    ],
    warning: 'เมื่อสูดดมควันที่เข้มมากไปจะทำให้อันตราย อาจมีอาการแพ้เกสร รสชาติขมกว่ากลิ่นที่หอมหวาน ควรผสมน้ำผึ้ง'
  },
  {
    id: 'wildCarrot',
    nameEn: 'Wild Carrot',
    nameTh: 'แครอทป่า',
    icon: '🥕',
    iconBg: 'linear-gradient(135deg,#2a1000,#4a2808)',
    categories: ['herb', 'hot'],
    effect: 'hot',
    effectLabel: 'ฤทธิ์ร้อน',
    tags: ['ขับปัสสาวะ', 'ป้องกันนิ่ว', 'digestive'],
    filterTags: ['herb', 'hot', 'digestive'],
    description: 'ดอกออกเป็นช่อซี่ร่ม สีขาวละเอียดคล้ายผ้าลูกไม้ จุดเด่นตรงกลางช่อดอกมักมีดอกย่อยสีม่วง-แดงเข้ม 1 ดอก เมื่อดอกแก่จะหุบเข้าหากันคล้ายรังนก รากแก้วสีขาวเรียวยาว กลิ่นหอมคล้ายแครอท แต่เนื้อแข็งและเหนียวกว่า ลำต้นมีขนแข็งปกคลุม สูง 30 ซม.',
    properties: [
      { icon: '🚽', title: 'ขับปัสสาวะ', text: 'ช่วยชะล้างทางเดินปัสสาวะ ลดการสะสมของกรดยูริค และป้องกันนิ่วในไต', level: 85 },
      { icon: '💨', title: 'ย่อยอาหาร', text: 'ช่วยขับลม แก้ท้องอืดและกระตุ้นการย่อย', level: 78 },
      { icon: '🫀', title: 'ชะล้างพิษ', text: 'ช่วยชะล้างพิษ กระตุ้นการทำงานของน้ำดี', level: 72 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บช่อดอกที่มีเมล็ดแก่หรือขูดราก ล้างให้สะอาด ผึ่งในที่ร่มมีลมโกรกจนแห้งสนิท ห้ามตากแดดจัด เก็บในโหลแยกเมล็ดและรากออกจากกัน' },
      { title: '🍵 การชง', steps: 'ไม่มีการต้ม ใช้การชงเท่านั้น นำเมล็ดแห้งมาชงชาในน้ำอุ่น ดื่มช่วยขับปัสสาวะ' },
      { title: '🔨 การบด', steps: 'นำเมล็ดที่แห้งสนิทมาตำหรือบดในโกร่งบดยาจนแตก เพื่อให้ตัวยาออกมาได้ง่ายขึ้น ใช้ผงเมล็ดบดชงดื่ม กระตุ้นรอบเดือน แก้ท้องอืด กระตุ้นการย่อย\nใช้ผงพอกผิวหนังบริเวณที่บาดแผลอักเสบ' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่เมล็ดหรือรากแห้งในแอลกอฮอล ทิ้งไว้ 4-6 สัปดาห์ ใช้หยดผสมน้ำดื่ม 5-10 หยด เพื่อล้างพิษ ต้านพิษ\nดองน้ำมัน: แช่เมล็ดแห้งในน้ำมันเมล็ดองุ่นหรือน้ำมันแอพริคอต ทิ้งไว้ 2-4 สัปดาห์ ทาบริเวณผิวหน้าเพื่อลดริ้วรอย บำรุงเซลล์ผิว' },
    ],
    warning: '⚠ ระวังพืชมีพิษที่เลียนแบบชื่อว่า เฮมล็อค (Poison Hemlock): Wild Carrot จะมีขนที่ลำต้นและดอกกลางสีม่วงอ่อน ส่วน Poison Hemlock ลำต้นเรียบและมีดอกสีม่วงแดง หากทามากไปอาจทำให้ผิวไวต่อแสง'
  },
  {
    id: 'wildFeverfew',
    nameEn: 'Wild Feverfew',
    nameTh: 'ไวลด์ฟีเวอร์ฟิว',
    icon: '🌼',
    iconBg: 'linear-gradient(135deg,#1a2a00,#2a4010)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็นอ่อน',
    tags: ['กระตุ้นภูมิ', 'ลดไข้', 'immune', 'wound'],
    filterTags: ['herb', 'cool', 'immune', 'wound'],
    description: 'ดอกออกเป็นช่อกระจุกแน่นสีขาวครีมที่ปลายยอด คล้ายดอกกะหล่ำขนาดเล็ก กลีบดอกย่อยสั้นและแข็ง รากมีระบบรากแก้วที่สะสมอาหารจนอวบหนาคล้ายหัวพืช เนื้อรากมีความแข็งและมีรสขมจัด ลำต้นตั้งตรงแข็งแรงสูงประมาณ 60-120 ซม. ใบมีขนาดใหญ่ผิวสากเหมือนกระดาษทราย',
    properties: [
      { icon: '💪', title: 'กระตุ้นภูมิคุ้มกัน', text: 'มีฤทธิ์คล้ายเอนชินาซี กระตุ้นเม็ดเลือดขาวกำจัดเชื้อโรค ใช้ลดไข้สลับหนาว อุณหภูมิไม่คงที่', level: 82 },
      { icon: '🫁', title: 'ลดอักเสบทางเดินหายใจ', text: 'บรรเทาอาการอักเสบของทางเดินหายใจ ลดอาการขัดจมูก', level: 75 },
      { icon: '🫀', title: 'บำรุงหัวใจอ่อนๆ', text: 'เหมาะสำหรับใส่อาหารให้ผู้ป่วยที่เลือดลมไม่ไปหล่อเลี้ยงหัวใจ หรือทำการถ่ายเลือดอยู่', level: 65 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บส่วนรากหรือส่วนที่เหนือดิน ล้างให้สะอาดหั่นรากเป็นแว่นหนา ผึ่งในที่ร่มมีลมโกรกจนแห้งสนิท เก็บในที่มิดชิดในโหลแก้วเพื่อรักษาความขมและสารสำคัญ' },
      { title: '🍵 การต้ม', steps: 'นำรากแห้งมาต้มกับน้ำด้วยไฟอ่อน เคี่ยวประมาณ 15 นาที จิบอุ่นๆ เพื่อลดไข้ ปรับอุณหภูมิให้คงที่หรือกระตุ้นภูมิคุ้มกันเมื่อมีอาการหวัด' },
      { title: '🔨 การบด', steps: 'นำใบหรือรากที่แห้งสนิทมาบดเป็นผงละเอียด ใช้ผงรากผสมน้ำเล็กน้อยพอกบริเวณที่มีแผลพุพองหรือแผลหายช้าเพื่อช่วยสมานแผลและฆ่าเชื้อ' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: ใช้รากแห้งแช่แอลกอฮอล 4-6 สัปดาห์ ใช้เพียง 15 หยดผสมน้ำดื่มวันละ 2-3 ครั้ง เช้า กลางวัน เย็น เพื่อบำรุงร่างกายและป้องกันอาการล้มป่วยในช่วงอากาศเปลี่ยนแปลง' },
    ],
    warning: 'การสัมผัสใบสดอาจทำให้เกิดอาการผื่นคัน ต้องแต่งตัวมิดชิด ผู้แพ้พืชตระกูลทานตะวัน น้ำมันคาโมไมล์ ต้องระมัดระวัง ใช้เกินขนาดอาจทำให้ปวดท้อง'
  },
  {
    id: 'indianTobacco',
    nameEn: 'Indian Tobacco',
    nameTh: 'ยาสูบอินเดีย',
    icon: '🌿',
    iconBg: 'linear-gradient(135deg,#00102a,#001840)',
    categories: ['herb', 'hot'],
    effect: 'hot',
    effectLabel: 'ฤทธิ์ร้อน',
    tags: ['หลอดลม', 'เลิกบุหรี่', 'nerve', 'danger'],
    filterTags: ['herb', 'hot', 'nerve', 'danger'],
    description: 'สมุนไพรฤทธิ์รุนแรง ญาติสนิทกับ Cardinal Flower แต่สารสำคัญเข้มข้นกว่ามาก ได้รับฉายา "Puke Weed" หรือยาสำรอก ดอกออกเป็นช่อกระเปาะขนาดเล็ก สีฟ้าจางหรือม่วงอ่อน รูปถ้วยแยกเป็น 5 แฉก ฝักเมล็ดพองลม เป็นที่มาของชื่อ Inflata',
    properties: [
      { icon: '🫁', title: 'ขยายหลอดลมรุนแรง', text: 'สารโลเบลีนช่วยขยายหลอดลมอย่างรุนแรง รักษาหอบหืด หลอดลมอักเสบ', level: 92 },
      { icon: '🚬', title: 'ถอนพิษนิโคติน', text: 'โครงสร้างทางเคมีคล้ายนิโคตินแต่ไม่ทำให้เสพติด ใช้เป็นสมุนไพรหลักในการเลิกบุหรี่', level: 85 },
      { icon: '💆', title: 'คลายกล้ามเนื้อ', text: 'ลดอาการเกร็งของกล้ามเนื้อและอาการวิตกกังวลที่ส่งผลต่อระบบหายใจ ส่วนผสมของยาระงับประสาท', level: 80 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บส่วนใบ ดอกและฝักพองลม ล้างให้สะอาดผึ่งในที่ร่มมีลมโกรกและมืดสนิท (รักษาสารอัลคาลอยด์) เก็บในภาชนะปิดสนิท หม้อหรือไห เก็บในที่แห้งและมืด พ้นจากแสง สารอัลคาลอยด์สลายตัวง่ายเมื่อโดนแสง' },
      { title: '🍵 การต้ม', steps: 'ใช้ใบและฝักแห้งเพียงเล็กน้อย น้อยกว่า Cardinal Flower ต้มในน้ำร้อนแล้วดับไฟแช่ทิ้งไว้ 5 นาที ใช้จิบ 1-2 อึกเพื่อขับเสมหะ ใช้ในสภาวะฉุกเฉินเพื่อขยายหลอดลม 1-2 อึกเท่านั้น' },
      { title: '🔨 การบด', steps: 'นำใบ ดอกและฝักมาบดละเอียดเป็นผงผสมขี้ผึ้ง ทาบริเวณหน้าอกช่วยให้หายใจคล่องขึ้น หรือพอกบริเวณที่กล้ามเนื้ออักเสบ เหมาะสำหรับผู้ป่วยกระดูกร้าว ทาเพื่อลดปวด' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ส่วนผสมแห้งในแอลกอฮอลเข้มข้น 2-4 สัปดาห์ ใช้เพียง 1-2 หยดเพื่อแก้อาการหอบหืด ระมัดระวังเรื่องอัตราส่วน มากเกินไปอาจทำให้หยุดหายใจ\nดองน้ำส้ม: แช่ในน้ำส้มสายชูแอปเปิ้ล 2-4 สัปดาห์ ผสมน้ำผึ้งทำยาจิบแก้ไอแห้ง' },
    ],
    warning: '⚠ อันตรายสูงมาก หากใช้ในปริมาณที่มากเกินไปจะอาเจียนรุนแรง ผิวซีดเหลือง ช้ำง่าย เลือดไม่เดิน ความดันโลหิตต่ำลง เหงื่อและอุณหภูมิร่างกายเย็นขึ้น มีอาการชักและหยุดหายใจ'
  },
  {
    id: 'orleanderSage',
    nameEn: 'Orleander Sage',
    nameTh: 'เซจโอลีแอนเดอร์',
    icon: '🫧',
    iconBg: 'linear-gradient(135deg,#10102a,#181828)',
    categories: ['herb', 'neutral'],
    effect: 'neutral',
    effectLabel: 'ฤทธิ์กลาง',
    tags: ['ฆ่าเชื้อ', 'ผ่อนคลาย', 'wound', 'nerve'],
    filterTags: ['herb', 'neutral', 'wound', 'nerve', 'digestive'],
    description: 'สมุนไพรหายากกึ่งกลางระหว่างพืชตระกูล Sage และมีความคล้ายคลึงกับ Rosebay ดอกออกเป็นช่อตั้งตรงที่ปลายกิ่ง สีม่วงเข้มหรือน้ำเงินอมม่วง มีรูปทรงแบบปากเปิดตามฉบับพืชตระกูล Sage ใบมีลักษณะเรียวยาวและแข็งคล้าย Rosebay ผิวใบมีขนละเอียดสีเทาเงินปกคลุม',
    properties: [
      { icon: '🦠', title: 'ฆ่าเชื้อทางเดินหายใจ', text: 'ฆ่าเชื้อลดการอักเสบทางเดินหายใจภายในช่องปาก', level: 80 },
      { icon: '😌', title: 'ผ่อนคลายจิตใจ', text: 'กลิ่นหอมระเหยช่วยให้จิตใจสงบและผ่อนคลาย ช่วยให้สมาธิดีขึ้น มีฤทธิ์รองจาก Ram Head ต่อระบบประสาท', level: 72 },
      { icon: '🧴', title: 'ฟื้นฟูผิวหนัง', text: 'สารสกัดจากใบช่วยสมานแผลและลดอาการคันจากผื่นแพ้ ใช้ดีกว่า Yarrow สำหรับผิวหนัง', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บส่วนใบและยอดดอก ล้างสะเด็ดน้ำให้แห้ง มัดเป็นช่อเล็กๆ แขวนในที่ร่มลมโกรกดี ห้ามตากแดด จะทำให้สารระเหยหายไป เมื่อแห้งแล้วเก็บใส่ภาชนะปิดสนิทเพื่อรักษาน้ำมันหอมระเหย' },
      { title: '🍵 การชง', steps: 'นำใบแห้ง 1-2 ใบชงในน้ำร้อนไม่เดือดจัดทิ้งไว้ 35 นาที ปล่อยให้น้ำปรับอุณหภูมิและสมุนไพรคลายตัว จิบขับเสมหะ บรรเทาอาการเจ็บคอ ใช้กลั้วรักษาแผลในปาก' },
      { title: '🔨 การบด', steps: 'นำใบแห้งบดเป็นผงละเอียด ใช้ผงผสมกับแป้งข้าวจ้าวหรือแป้งข้าวโพดพอกผิวหนังลดอาการบวมแดง นำใบสดมาขยี้และพอกบริเวณแผลสดเพื่อสมานบาดแผลและลดอาการคัน (แผลตกสเก็ด)' },
      { title: '🍾 การสกัด', steps: 'ดองเหล้า: แช่ใบแห้งในแอลกอฮอลเข้มข้น 2-4 สัปดาห์ ใช้หยดผสมน้ำ 15 หยด ช่วยย่อย ลดท้องอืดหลังอาหาร\nดองน้ำมัน: แช่ใบแห้งในน้ำมันอัลมอนด์หรือมะกอก 2-4 สัปดาห์ ทาขมับลดอาการปวดศีรษะหรือนวดคลายกล้ามเนื้อ' },
    ],
    warning: 'ถึงจะมีชื่อว่า Sage และไม่มีพิษร้ายแรงเท่า Rosebay แต่ไม่ควรใช้มากเกินไป ในปริมาณมากเกินไปจะส่งผลเสียกับความดันโลหิต ร้ายแรงที่สุดทำให้หัวใจเต้นผิดจังหวะและชีพจรอาจหยุดเต้น'
  },
  {
    id: 'bloodFlower',
    nameEn: 'Blood Flower',
    nameTh: 'บลัดฟลาวเวอร์',
    icon: '🌺',
    iconBg: 'linear-gradient(135deg,#2a0000,#500010)',
    categories: ['herb', 'cool'],
    effect: 'cool',
    effectLabel: 'ฤทธิ์เย็น+กลาง',
    tags: ['หัวใจ', 'ขับพิษ', 'danger'],
    filterTags: ['herb', 'cool', 'neutral', 'danger'],
    description: 'พืชมีฤทธิ์ทางสมุนไพรรุนแรง ดอกออกเป็นช่อกระจุกซี่ร่มที่ปลายยอด กลีบดอกสีแดงสดจะพับงอด้านล่างอย่างเป็นระเบียบ มีเกสรรูปมงกุฎสีเหลืองส้มยื่นเด่น รากเป็นระบบรากแก้วแข็งแรงสีน้ำตาลอ่อน เมื่อตัดหรือหักจะมีน้ำยางขาวขุ่นไหลออกมาจำนวนมาก เป็นแหล่งสะสมสารสำคัญที่เข้มข้น',
    properties: [
      { icon: '🫀', title: 'กระตุ้นการทำงานของหัวใจ', text: 'สารไลโกลไซด์ออกฤทธิ์เพิ่มแรงบีบของกล้ามเนื้อหัวใจ ใช้รักษาโรคตัวบวมที่เกิดจากโรคหัวใจ', level: 88 },
      { icon: '🤢', title: 'ขับพยาธิและอาเจียน', text: 'รากมีฤทธิ์รุนแรงในการขับพยาธิ ใช้เป็นยาฉุกเฉินเพื่อทำให้อาเจียนในกรณีที่ได้รับสารพิษ', level: 85 },
      { icon: '✂️', title: 'ตัดตาปลาและเนื้อเยื่อ', text: 'น้ำยางมีเอนไซม์ที่ช่วยย่อยสลายโปรตีน ใช้ในการตัดตาปลาหรือทำลายเนื้อเยื่อส่วนเกิน (เนื้อเยื่อที่ตายแล้ว)', level: 80 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'เก็บรากล้างสะอาดด้วยน้ำเย็น เพื่อให้น้ำยางแข็งตัวไม่ไหลทิ้ง หั่นแว่นบางๆ ผึ่งที่ร่มแห้งสนิท เก็บในโหลทึบไม่โดนแสงแดดเพื่อรักษาสาร' },
      { title: '🍵 การต้ม', steps: 'ต้มรากแห้งกับน้ำสะอาดด้วยไฟอ่อน จนตัวยาเข้มข้น จิบเล็กน้อยเพื่อกระตุ้นอาเจียน ขับเสมหะ (ขับพิษบางส่วน)' },
      { title: '🔨 การบด', steps: 'นำรากหรือใบมาบดในโกร่งบดยาจนเป็นผงละเอียด\nใช้ผงรากผสมน้ำเชื่อมปริมาณเล็กน้อย ขับเสมหะ\nใช้ผงใบแห้งผสมน้ำผึ้ง พอกบริเวณที่เป็นฝีช่วยดูดหนอง (แนะนำเฉพาะฝี)' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำส้มสายชู: แช่รากสดในน้ำส้มสายชู ทิ้งไว้ 2-4 สัปดาห์ ใช้สำลีชุบแต้มในจุดที่เป็นหูดหรือตาปลาจนกว่าจะหลุดออก' },
    ],
    warning: '⚠ อันตรายสูงมาก ในปริมาณเกินขนาดอาจทำให้หัวใจเต้นผิดจังหวะ ตาพร่ามัวและหัวใจหยุดเต้น น้ำยางหากสัมผัสผิวหนังจะทำให้มีผื่นพอง เข้าตาทำให้ตาบอด ผู้ป่วยโรคหัวใจห้ามใช้'
  },

  // ─── NEW MUSHROOMS ──────────────────────────────────────────
  {
    id: 'bayBolete',
    nameEn: 'Bay Bolete',
    nameTh: 'เห็ดเบย์โบลีท',
    icon: '🍄',
    iconBg: 'linear-gradient(135deg,#1a0800,#301400)',
    categories: ['mushroom', 'neutral'],
    effect: 'neutral',
    effectLabel: 'ฤทธิ์กลาง',
    tags: ['บำรุงประสาท', 'เสริมภูมิคุ้มกัน', 'immune', 'nerve'],
    filterTags: ['mushroom', 'neutral', 'immune', 'nerve'],
    description: 'หมวกเห็ดมีสีน้ำตาลเข้มคล้ายช็อกโกแลต ผิวสัมผัสกำมะหยี่เมื่อยังอ่อน และมันวาวขึ้นเมื่อชื้น ใต้ดอกไม่มีครีบแต่เป็นรู สีเหลืองอมเขียว จุดเด่นคือเมื่อถูกนิ้วกดหรือขีดข่วนจะเปลี่ยนเป็นสีน้ำเงินแกมเขียวทันที ก้านค่อนข้างยาว เนื้อในแน่นขาวนวล เปลี่ยนเป็นสีฟ้าจางๆ เมื่อถูกตัด',
    properties: [
      { icon: '🧠', title: 'บำรุงระบบประสาท', text: 'มีสารแอลทิไนล์สูง ช่วยให้สมองผ่อนคลาย เพิ่มสมาธิ', level: 82 },
      { icon: '🛡', title: 'ต้านอนุมูลอิสระ', text: 'ป้องกันการเสื่อมของเซลล์', level: 75 },
      { icon: '💪', title: 'เสริมภูมิคุ้มกัน', text: 'ช่วยให้ระบบภูมิคุ้มกันทำงานได้ดีขึ้น', level: 78 },
    ],
    usage: [
      { title: '🌞 การตาก', steps: 'หั่นเห็ดเป็นแผ่นบางๆ ร้อยด้ายแขวนหรือวางในตะแกรงในที่แห้งลมโกรกจนกรอบเหมือนกระดาษ เก็บในโหลแก้วที่มิดชิด การตากแห้งจะทำให้กลิ่นของเห็ดเข้มข้นขึ้น' },
      { title: '🍵 การต้ม', steps: 'นำเห็ดสดมาปรุงอาหารได้หลากหลาย เช่น ทำซุปหรือซอสพาสต้า เนื้อเห็ดชนิดนี้ไม่เละง่ายเมื่อโดนความร้อน เหมาะกับการเคี่ยวเพื่อให้รสชาติที่นุ่มนวล' },
      { title: '🔨 การบด', steps: 'นำเห็ดแห้งสนิทมาบดเป็นผงละเอียด ใช้เป็นผงชูรสโรยในอาหารหรือผสมแป้งขนมปังเพื่อเพิ่มกลิ่นอายของป่า' },
      { title: '🍾 การสกัด', steps: 'ดองน้ำมัน: นำเห็ดผสมน้ำมันมะกอกหรือแอพริคอต แช่ไว้ 2-4 สัปดาห์ ใช้แค่น้ำมันหยดเพิ่มความหอม' },
    ],
    warning: 'การเปลี่ยนสีของเห็ดเป็นสีน้ำเงินเป็นเรื่องปกติ ระวังเห็ดพิษลักษณะใกล้เคียงกันคือ Boletus Satanas (เห็ดมาร) ซึ่งมีก้านสีแดงจัด เป็นพิษรุนแรงต่อทางเดินอาหาร'
  },
];

// Build herb list
const herbList = document.getElementById('herbList');
const detailPanel = document.getElementById('herbDetails');
const placeholder = document.getElementById('placeholder');

function buildHerbList(filtered) {
  herbList.innerHTML = '';
  let visible = 0;
  filtered.forEach(h => {
    const item = document.createElement('div');
    item.className = 'herb-list-item';
    item.dataset.id = h.id;
    const effectClass = h.effect === 'cool' ? 'effect-cool' : h.effect === 'hot' ? 'effect-hot' : 'effect-neutral';
    const effectShort = h.effect === 'cool' ? 'เย็น' : h.effect === 'hot' ? 'ร้อน' : 'กลาง';
    item.innerHTML = `
      <div class="herb-icon" style="background:${h.iconBg}">${h.icon}</div>
      <div class="herb-list-info">
        <div class="herb-list-name">${h.nameEn}</div>
        <div class="herb-list-category">${h.nameTh}</div>
      </div>
      <span class="effect-badge ${effectClass}">${effectShort}</span>
    `;
    item.addEventListener('click', () => selectHerb(h));
    herbList.appendChild(item);
    visible++;
  });
  document.getElementById('noResults').style.display = visible === 0 ? 'block' : 'none';
}

function selectHerb(herb) {
  document.querySelectorAll('.herb-list-item').forEach(el => {
    el.classList.toggle('active', el.dataset.id === herb.id);
  });
  placeholder.style.display = 'none';
  renderDetail(herb);
}

function renderDetail(h) {
  const effectClass = h.effect === 'cool' ? 'effect-cool' : h.effect === 'hot' ? 'effect-hot' : 'effect-neutral';
  const categoryLabel = h.categories.includes('mushroom') ? '🍄 เห็ด' : h.categories.includes('berry') ? '🫐 เบอร์รี่' : '🌿 สมุนไพร';

  const propsHtml = h.properties.map(p => `
    <div class="property-card">
      <div class="property-icon">${p.icon}</div>
      <div class="property-title">${p.title}</div>
      <div class="property-text">${p.text}</div>
      <div class="prop-bar-wrap">
        <div class="prop-bar"><div class="prop-bar-fill" data-level="${p.level}" style="background:var(--accent); width:0"></div></div>
      </div>
    </div>
  `).join('');

  const usageHtml = h.usage.map(u => `
    <div class="usage-method">
      <div class="usage-method-header" onclick="toggleMethod(this)">
        <span class="method-label">${u.title}</span>
        <span class="method-arrow">▼</span>
      </div>
      <div class="usage-method-body">
        <div class="method-steps">${u.steps.replace(/\n/g, '<br>')}</div>
      </div>
    </div>
  `).join('');

  detailPanel.innerHTML = `
    <div class="herb-detail active" id="detail_${h.id}">
      <div class="detail-header">
        <div class="detail-icon-wrap" style="background:${h.iconBg}">${h.icon}</div>
        <div class="detail-title-group">
          <div class="detail-name-en">${h.nameEn}</div>
          <div class="detail-name-th">${h.nameTh}</div>
          <div class="detail-meta">
            <span class="meta-tag effect-badge ${effectClass}" style="font-size:0.8rem;padding:0.3rem 0.8rem">${h.effectLabel}</span>
            <span class="meta-tag" style="border-color:var(--border2);color:var(--text2)">${categoryLabel}</span>
            ${h.tags.filter(t => !['wound','immune','digestive'].includes(t)).map(t => `<span class="meta-tag" style="border-color:var(--border);color:var(--text3)">${t}</span>`).join('')}
          </div>
        </div>
      </div>
      <div class="detail-tabs">
        <button class="detail-tab active" onclick="switchTab(this,'desc_${h.id}')">🌿 ลักษณะ</button>
        <button class="detail-tab" onclick="switchTab(this,'props_${h.id}')">💊 สรรพคุณ</button>
        <button class="detail-tab" onclick="switchTab(this,'usage_${h.id}')">🔧 การใช้งาน</button>
        <button class="detail-tab" onclick="switchTab(this,'warn_${h.id}')">⚠️ คำเตือน</button>
      </div>
      <div class="tab-content active" id="desc_${h.id}">
        <div class="herb-desc">${h.description}</div>
      </div>
      <div class="tab-content" id="props_${h.id}">
        <div class="properties-grid">${propsHtml}</div>
      </div>
      <div class="tab-content" id="usage_${h.id}">
        ${usageHtml}
      </div>
      <div class="tab-content" id="warn_${h.id}">
        <div class="warning-box"><strong>⚠ ข้อควรระวัง</strong>${h.warning}</div>
      </div>
    </div>
  `;

  // Animate bars
  setTimeout(() => {
    document.querySelectorAll('.prop-bar-fill').forEach(bar => {
      bar.style.width = bar.dataset.level + '%';
    });
  }, 100);
}

function switchTab(btn, targetId) {
  const container = btn.closest('.herb-detail');
  container.querySelectorAll('.detail-tab').forEach(t => t.classList.remove('active'));
  container.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById(targetId).classList.add('active');
}

function toggleMethod(header) {
  const method = header.closest('.usage-method');
  method.classList.toggle('open');
}

// Filtering
let activeFilter = 'all';
let searchQuery = '';

function applyFilters() {
  const filtered = herbs.filter(h => {
    const matchFilter = activeFilter === 'all' || h.filterTags.includes(activeFilter);
    const q = searchQuery.toLowerCase();
    const matchSearch = !q || h.nameEn.toLowerCase().includes(q) || h.nameTh.includes(q) || h.tags.some(t => t.toLowerCase().includes(q));
    return matchFilter && matchSearch;
  });
  buildHerbList(filtered);
}

document.getElementById('filterBar').addEventListener('click', e => {
  const chip = e.target.closest('.filter-chip');
  if (!chip) return;
  document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
  chip.classList.add('active');
  activeFilter = chip.dataset.filter;
  applyFilters();
});

document.getElementById('searchInput').addEventListener('input', e => {
  searchQuery = e.target.value;
  applyFilters();
});

// Particles
(function() {
  const canvas = document.getElementById('particles-canvas');
  const ctx = canvas.getContext('2d');
  let particles = [];
  const W = () => { canvas.width = window.innerWidth; canvas.height = window.innerHeight; };
  W();
  window.addEventListener('resize', W);

  class Particle {
    constructor() { this.reset(); }
    reset() {
      this.x = Math.random() * canvas.width;
      this.y = Math.random() * canvas.height;
      this.r = Math.random() * 1.5 + 0.3;
      this.vx = (Math.random() - 0.5) * 0.3;
      this.vy = -Math.random() * 0.4 - 0.1;
      this.life = Math.random();
      this.maxLife = Math.random() * 0.5 + 0.3;
    }
    update() {
      this.x += this.vx;
      this.y += this.vy;
      this.life += 0.003;
      if (this.life > this.maxLife || this.y < -5) this.reset();
    }
    draw() {
      const a = Math.sin(this.life / this.maxLife * Math.PI) * 0.5;
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(122,184,64,${a})`;
      ctx.fill();
    }
  }

  for (let i = 0; i < 80; i++) particles.push(new Particle());

  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    particles.forEach(p => { p.update(); p.draw(); });
    requestAnimationFrame(animate);
  }
  animate();
})();

// Init
buildHerbList(herbs);

// Auto-select first herb
setTimeout(() => selectHerb(herbs[0]), 100);
</script>
</body>
</html>
