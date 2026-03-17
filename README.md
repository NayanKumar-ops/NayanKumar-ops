<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 320" width="800" height="320">
  <defs>
    <filter id="glow" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="6"/><feMerge><feMergeNode in="coloredBlur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
    <linearGradient id="cyanWave" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#a0f0ff"><animate attributeName="stop-color" values="#a0f0ff;#40c0ff;#a0f0ff" dur="6s" repeatCount="indefinite"/></stop>
      <stop offset="50%" stop-color="#40c0ff"><animate attributeName="stop-color" values="#40c0ff;#80e0ff;#40c0ff" dur="5s" repeatCount="indefinite"/></stop>
      <stop offset="100%" stop-color="#a0f0ff"><animate attributeName="stop-color" values="#a0f0ff;#40c0ff;#a0f0ff" dur="6s" repeatCount="indefinite"/></stop>
    </linearGradient>
    <radialGradient id="nebula"><stop offset="10%" stop-color="#1a5a7a" stop-opacity="0.3"/><stop offset="80%" stop-color="#03050a"/></radialGradient>
    <path id="trail" d="M 70,240 C 100,100 240,30 380,110 C 460,155 430,220 360,240 C 280,265 200,210 170,270 C 150,310 240,290 380,295 C 500,298 640,270 710,180 C 760,120 730,60 640,50 C 540,38 500,110 540,175 C 580,240 680,280 730,250 C 770,225 780,170 760,130"/>
  </defs>
  <rect width="800" height="320" fill="#03050a"/>
  <circle cx="200" cy="50" r="180" fill="url(#nebula)"><animate attributeName="r" values="170;200;170" dur="18s" repeatCount="indefinite"/></circle>
  <circle cx="650" cy="270" r="220" fill="url(#nebula)"><animate attributeName="r" values="210;240;210" dur="22s" repeatCount="indefinite"/></circle>
  <text x="400" y="168" text-anchor="middle" fill="url(#cyanWave)" font-size="60" font-weight="bold" letter-spacing="8" filter="url(#glow)">NAYAN KUMAR</text>
  <line x1="260" y1="185" x2="540" y2="185" stroke="#40c0ff" stroke-width="1" opacity="0.7"><animate attributeName="opacity" values="0.6;0.9;0.6" dur="3s" repeatCount="indefinite"/></line>
  <text x="400" y="212" text-anchor="middle" fill="#b0f0ff" font-family="monospace" font-size="14" letter-spacing="4">CLOUD SECURITY</text>
  <text x="400" y="240" text-anchor="middle" fill="#6aa0b0" font-size="12.5" font-style="italic">Sculpting secure cloud experiences</text>
  <!-- Comet trail (30 layers with SMIL) – abbreviated here; full 30-layer version in final code -->
  <circle r="6" fill="#40c0ff" filter="url(#glow)"><animateMotion dur="10s" repeatCount="indefinite" begin="0s"><mpath href="#trail"/></animateMotion></circle>
  <!-- (additional trail circles omitted for brevity) -->
</svg>
