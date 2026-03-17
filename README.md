This is a standalone HTML file designed for your GitHub Pages personal site. It includes a responsive layout, advanced 3D background, custom cursor, and smooth animations — all optimized to impress HR and recruiters. Simply save it as `index.html`, push to a repository, and enable GitHub Pages.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Maybach Nayan · digital architect</title>
    <meta name="description" content="Digital architect crafting immersive luxury experiences. Explore portfolio, expertise, and visionary work.">
    <!-- favicon (simple inline) -->
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90' fill='%23D4AF37'>✦</text></svg>">

    <!-- Tailwind (core utilities) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Fonts & libraries -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Inter:opsz,wght@14..32,300;14..32,400;14..32,600;14..32,800&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
    
    <!-- Three.js, GSAP + ScrollTrigger, TextPlugin, Lenis smooth scroll -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/TextPlugin.min.js"></script>
    <script src="https://cdn.jsdelivr.net/gh/studio-freight/lenis@1.0.27/bundled/lenis.min.js"></script>

    <style>
        * { cursor: none; }
        body {
            background: #030303;
            color: #f0f0f0;
            overflow-x: hidden;
            font-family: 'Inter', sans-serif;
        }
        /* custom cursor — advanced magnetic feel */
        .cursor-dot, .cursor-outline {
            position: fixed;
            top: 0; left: 0;
            transform: translate(-50%, -50%);
            border-radius: 50%;
            z-index: 9999;
            pointer-events: none;
            user-select: none;
            opacity: 0;
            transition: opacity 0.3s ease;
        }
        .cursor-dot {
            width: 6px; height: 6px;
            background: #D4AF37;
            box-shadow: 0 0 15px #D4AF37;
        }
        .cursor-outline {
            width: 48px; height: 48px;
            border: 1.5px solid rgba(212, 175, 55, 0.5);
            backdrop-filter: blur(2px);
            transition: width 0.2s, height 0.2s, border-color 0.2s, background 0.2s;
        }
        body:hover .cursor-outline {
            width: 70px; height: 70px;
            border-color: #D4AF37;
            background: rgba(212, 175, 55, 0.03);
        }
        a, button, .magnetic {
            cursor: none;
        }
        /* disable custom cursor on touch devices */
        @media (hover: none) and (pointer: coarse) {
            * { cursor: auto; }
            .cursor-dot, .cursor-outline { display: none !important; }
        }
        /* scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0a0a0a; }
        ::-webkit-scrollbar-thumb { background: #D4AF37; border-radius: 0; }
        
        /* glassmorphism refined */
        .glass-card {
            background: rgba(20, 20, 20, 0.4);
            backdrop-filter: blur(14px) saturate(180%);
            border: 1px solid rgba(212, 175, 55, 0.1);
            border-right-color: rgba(212, 175, 55, 0.2);
            border-bottom-color: rgba(212, 175, 55, 0.2);
            box-shadow: 0 20px 40px -12px rgba(0,0,0,0.8);
        }
        .text-gold-gradient {
            background: linear-gradient(145deg, #D4AF37 0%, #F2E2A5 45%, #D4AF37 80%);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .marquee {
            white-space: nowrap;
            overflow: hidden;
            mask-image: linear-gradient(to right, transparent, black 15%, black 85%, transparent);
        }
        .marquee span {
            display: inline-block;
            animation: marquee 25s linear infinite;
        }
        @keyframes marquee {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }
        .loader {
            position: fixed;
            inset: 0;
            background: #000;
            z-index: 10000;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        #canvas-container {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 0;
            opacity: 0.5;
            pointer-events: none;
        }
        section, header, footer { position: relative; z-index: 5; }
        input, textarea { background: transparent; border-bottom: 1px solid #333; transition: 0.3s; }
        input:focus, textarea:focus { border-bottom-color: #D4AF37; outline: none; }
        /* hide scrollbar for horizontal work section */
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        /* mobile menu (simple) */
        .mobile-nav { display: none; }
        .mobile-nav.open { display: flex; }
        @media (max-width: 768px) {
            .nav-links { display: none; }
        }
    </style>
</head>
<body class="antialiased selection:bg-maybach-gold selection:text-black">

    <!-- preloader -->
    <div class="loader" id="loader">
        <h1 class="font-display text-4xl md:text-7xl text-maybach-gold tracking-widest mb-4" style="font-family: 'Cinzel';">MAYBACH NAYAN</h1>
        <div class="w-64 h-1 bg-gray-900 rounded-full overflow-hidden">
            <div class="h-full bg-maybach-gold w-0" id="loader-bar"></div>
        </div>
    </div>

    <!-- custom cursor (only on non-touch) -->
    <div class="cursor-dot hidden md:block" id="cursorDot"></div>
    <div class="cursor-outline hidden md:block" id="cursorOutline"></div>

    <!-- navigation (mix-blend difference for hero) -->
    <nav class="fixed top-0 w-full z-50 px-6 py-8 flex justify-between items-center mix-blend-difference text-white">
        <a href="#" class="font-display text-2xl md:text-3xl font-black tracking-widest hover:opacity-70 transition">MN.</a>
        <div class="hidden md:flex space-x-12 font-sans text-xs tracking-[0.2em] uppercase nav-links">
            <a href="#about" class="nav-link relative group">About<span class="absolute -bottom-1 left-0 w-0 h-px bg-maybach-gold group-hover:w-full transition-all duration-500"></span></a>
            <a href="#work" class="nav-link relative group">Work<span class="absolute -bottom-1 left-0 w-0 h-px bg-maybach-gold group-hover:w-full transition-all duration-500"></span></a>
            <a href="#contact" class="nav-link relative group">Contact<span class="absolute -bottom-1 left-0 w-0 h-px bg-maybach-gold group-hover:w-full transition-all duration-500"></span></a>
        </div>
        <button class="md:hidden text-white text-2xl focus:outline-none" id="menuBtn">☰</button>
    </nav>
    <!-- mobile menu dropdown -->
    <div class="mobile-nav fixed top-20 left-0 w-full bg-black/95 backdrop-blur-lg z-40 flex-col items-center py-8 space-y-6 text-white font-sans text-sm tracking-widest uppercase md:hidden" id="mobileMenu">
        <a href="#about" class="hover:text-maybach-gold" onclick="toggleMenu()">About</a>
        <a href="#work" class="hover:text-maybach-gold" onclick="toggleMenu()">Work</a>
        <a href="#contact" class="hover:text-maybach-gold" onclick="toggleMenu()">Contact</a>
    </div>

    <!-- 3D background -->
    <div id="canvas-container"></div>

    <!-- hero section with split text & parallax -->
    <header class="relative min-h-screen flex flex-col justify-center items-center text-center px-4 overflow-hidden">
        <div class="z-10 max-w-6xl mx-auto">
            <p class="font-mono text-maybach-gold tracking-[0.4em] text-sm md:text-base mb-6 opacity-0 hero-reveal">— EST. 2024 —</p>
            <h1 class="font-serif text-5xl md:text-8xl lg:text-9xl leading-[0.9] font-bold">
                <span class="block overflow-hidden"><span class="inline-block hero-split">DIGITAL</span></span>
                <span class="block overflow-hidden"><span class="inline-block hero-split text-gold-gradient">LUXURY</span></span>
                <span class="block overflow-hidden"><span class="inline-block hero-split">ARCHITECT</span></span>
            </h1>
            <p class="font-sans text-gray-400 max-w-2xl mx-auto text-lg md:text-xl leading-relaxed mt-8 opacity-0 hero-reveal">
                Sculpting high‑definition experiences where code meets craftsmanship.
            </p>
            <div class="mt-12 opacity-0 hero-reveal">
                <a href="#work" class="magnetic inline-block border border-maybach-gold text-maybach-gold px-10 py-5 hover:bg-maybach-gold hover:text-black transition-all duration-500 font-sans tracking-[0.2em] text-sm uppercase relative overflow-hidden group">
                    <span class="relative z-10">enter the atelier</span>
                    <span class="absolute inset-0 bg-maybach-gold translate-y-full group-hover:translate-y-0 transition-transform duration-500"></span>
                </a>
            </div>
        </div>
        <div class="absolute bottom-10 left-1/2 -translate-x-1/2 text-gray-600 text-sm flex flex-col items-center gap-1">
            <span>scroll to discover</span>
            <svg class="w-5 h-5 animate-bounce" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path></svg>
        </div>
    </header>

    <!-- infinite marquee (prestige clients) -->
    <div class="marquee py-10 border-y border-gray-900 relative z-10 bg-black/20 backdrop-blur-sm">
        <span class="text-3xl md:text-4xl font-display uppercase tracking-[0.2em] text-gray-600">
            ◈ Rolls-Royce  ◈  Bvlgari  ◈  Aston Martin  ◈  Sotheby's  ◈  Maybach  ◈  Christies  ◈  Bentley  ◈  Louis Vuitton  ◈ 
            Rolls-Royce  ◈  Bvlgari  ◈  Aston Martin  ◈  Sotheby's  ◈  Maybach  ◈  Christies  ◈  Bentley  ◈  Louis Vuitton  ◈ 
        </span>
    </div>

    <!-- about section -->
    <section id="about" class="py-28 md:py-40 px-6 bg-transparent relative">
        <div class="max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-2 gap-20 items-center">
            <div class="about-text-reveal">
                <h2 class="font-display text-5xl md:text-6xl mb-6 text-white">The <span class="text-maybach-gold italic">Visionary</span></h2>
                <div class="h-px w-24 bg-maybach-gold mb-8"></div>
                <p class="font-sans text-gray-300 text-lg leading-loose mb-6">
                    I am Maybach Nayan — a digital architect forging immersive narratives for the elite. With 12+ years of bridging avant-garde design and robust engineering, I create digital entities that breathe.
                </p>
                <p class="font-sans text-gray-400 text-lg leading-loose">
                    Every line of code is a deliberate stroke, every transition a choreographed emotion. My work is not just seen; it's experienced.
                </p>
                <div class="flex gap-12 mt-12">
                    <div><span class="block text-4xl font-display text-maybach-gold">12+</span><span class="text-xs tracking-widest text-gray-600">YEARS</span></div>
                    <div><span class="block text-4xl font-display text-maybach-gold">∞</span><span class="text-xs tracking-widest text-gray-600">OBSSESSION</span></div>
                </div>
            </div>
            <div class="relative h-[600px] w-full about-img-reveal group">
                <div class="absolute inset-0 border border-maybach-gold/30 translate-x-6 translate-y-6 transition-transform group-hover:translate-x-8 group-hover:translate-y-8 duration-500"></div>
                <img src="https://images.unsplash.com/photo-1560250097-0b93528c311a?q=80&w=1887&auto=format&fit=crop" alt="portrait" class="w-full h-full object-cover grayscale hover:grayscale-0 transition-all duration-1000">
            </div>
        </div>
    </section>

    <!-- expertise cards -->
    <section class="py-24 relative overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-b from-maybach-dark via-transparent to-maybach-dark pointer-events-none"></div>
        <div class="max-w-7xl mx-auto px-6">
            <div class="flex flex-col md:flex-row justify-between items-end mb-20">
                <h2 class="font-serif text-5xl md:text-7xl text-white">expertise</h2>
                <p class="font-sans text-maybach-gold tracking-[0.3em] uppercase text-sm">core competencies</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div class="glass-card p-12 group hover:bg-white/5 transition-all duration-700 service-card">
                    <div class="text-maybach-gold mb-8 text-5xl">⌘</div>
                    <h3 class="font-display text-3xl text-white mb-4 group-hover:text-maybach-gold transition-colors">UI/UX</h3>
                    <p class="text-gray-400 text-sm leading-relaxed">Sculpting user journeys with obsessive clarity and sensual micro‑interactions.</p>
                </div>
                <div class="glass-card p-12 group hover:bg-white/5 transition-all duration-700 service-card">
                    <div class="text-maybach-gold mb-8 text-5xl">⚙️</div>
                    <h3 class="font-display text-3xl text-white mb-4 group-hover:text-maybach-gold transition-colors">DEV</h3>
                    <p class="text-gray-400 text-sm leading-relaxed">Full‑stack mastery — WebGL, WebGPU, and frameworks that yield buttery performance.</p>
                </div>
                <div class="glass-card p-12 group hover:bg-white/5 transition-all duration-700 service-card">
                    <div class="text-maybach-gold mb-8 text-5xl">✦</div>
                    <h3 class="font-display text-3xl text-white mb-4 group-hover:text-maybach-gold transition-colors">3D VISION</h3>
                    <p class="text-gray-400 text-sm leading-relaxed">Real‑time 3D brandscapes, configurators, and digital twins with cinematic flair.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- featured work (horizontal scroll) -->
    <section id="work" class="py-24 bg-transparent relative">
        <div class="max-w-7xl mx-auto px-6">
            <h2 class="font-serif text-5xl md:text-7xl text-white mb-20 text-center">selected <span class="text-maybach-gold">creations</span></h2>
        </div>
        <div class="flex overflow-x-auto snap-x snap-mandatory hide-scrollbar pl-6 md:pl-[calc((100%-80rem)/2)] gap-8 pb-12" style="scrollbar-width: none;">
            <div class="min-w-[85vw] md:min-w-[600px] lg:min-w-[800px] snap-start project-card">
                <div class="relative group overflow-hidden rounded-sm">
                    <div class="absolute inset-0 bg-black/60 z-10 opacity-0 group-hover:opacity-100 transition-opacity duration-500 flex items-center justify-center">
                        <span class="border border-white text-white px-8 py-4 uppercase tracking-widest text-xs backdrop-blur-sm">discover</span>
                    </div>
                    <img src="https://images.unsplash.com/photo-1545324418-cc1e3fa86c52?q=80&w=2070&auto=format&fit=crop" class="w-full h-[500px] object-cover transform group-hover:scale-110 transition-transform duration-1000">
                    <div class="absolute bottom-8 left-8 z-20">
                        <span class="text-maybach-gold text-xs tracking-[0.3em]">RESIDENCE</span>
                        <h3 class="font-display text-4xl text-white">ONYX PENTHOUSE</h3>
                    </div>
                </div>
            </div>
            <div class="min-w-[85vw] md:min-w-[600px] lg:min-w-[800px] snap-start project-card">
                <div class="relative group overflow-hidden rounded-sm">
                    <div class="absolute inset-0 bg-black/60 z-10 opacity-0 group-hover:opacity-100 transition-opacity duration-500 flex items-center justify-center">
                        <span class="border border-white text-white px-8 py-4 uppercase tracking-widest text-xs backdrop-blur-sm">discover</span>
                    </div>
                    <img src="https://images.unsplash.com/photo-1618220179428-22790b461013?q=80&w=2070&auto=format&fit=crop" class="w-full h-[500px] object-cover transform group-hover:scale-110 transition-transform duration-1000">
                    <div class="absolute bottom-8 left-8 z-20">
                        <span class="text-maybach-gold text-xs tracking-[0.3em]">AUTOMOTIVE</span>
                        <h3 class="font-display text-4xl text-white">VELOCITA GT</h3>
                    </div>
                </div>
            </div>
            <div class="min-w-[85vw] md:min-w-[600px] lg:min-w-[800px] snap-start project-card">
                <div class="relative group overflow-hidden rounded-sm">
                    <div class="absolute inset-0 bg-black/60 z-10 opacity-0 group-hover:opacity-100 transition-opacity duration-500 flex items-center justify-center">
                        <span class="border border-white text-white px-8 py-4 uppercase tracking-widest text-xs backdrop-blur-sm">discover</span>
                    </div>
                    <img src="https://images.unsplash.com/photo-1600494603989-9650cf6eee3a?q=80&w=2070&auto=format&fit=crop" class="w-full h-[500px] object-cover transform group-hover:scale-110 transition-transform duration-1000">
                    <div class="absolute bottom-8 left-8 z-20">
                        <span class="text-maybach-gold text-xs tracking-[0.3em]">WATCHES</span>
                        <h3 class="font-display text-4xl text-white">CHRONOMÉTRIE</h3>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- contact section -->
    <section id="contact" class="py-32 px-6 relative">
        <div class="max-w-3xl mx-auto text-center">
            <p class="text-maybach-gold tracking-[0.3em] uppercase mb-4">initiate dialogue</p>
            <h2 class="font-serif text-6xl md:text-8xl text-white mb-16">let’s <span class="italic text-gray-600">collide</span></h2>
            <form id="contactForm" class="space-y-10 text-left" onsubmit="event.preventDefault(); alert('Thank you. The atelier will respond within 24h.');">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="group">
                        <label class="block text-xs uppercase tracking-widest text-gray-500 mb-2 group-focus-within:text-maybach-gold">name</label>
                        <input type="text" class="w-full bg-transparent border-b border-gray-700 py-3 text-white focus:border-maybach-gold" placeholder="---">
                    </div>
                    <div class="group">
                        <label class="block text-xs uppercase tracking-widest text-gray-500 mb-2 group-focus-within:text-maybach-gold">email</label>
                        <input type="email" class="w-full bg-transparent border-b border-gray-700 py-3 text-white focus:border-maybach-gold" placeholder="---">
                    </div>
                </div>
                <div class="group">
                    <label class="block text-xs uppercase tracking-widest text-gray-500 mb-2 group-focus-within:text-maybach-gold">message</label>
                    <textarea rows="4" class="w-full bg-transparent border-b border-gray-700 py-3 text-white focus:border-maybach-gold" placeholder="your vision..."></textarea>
                </div>
                <div class="text-center pt-8">
                    <button type="submit" class="magnetic bg-maybach-gold text-black px-16 py-5 font-sans font-bold uppercase tracking-[0.2em] hover:bg-white transition-all duration-300 text-sm">
                        send inquiry
                    </button>
                </div>
            </form>
        </div>
    </section>

    <!-- footer -->
    <footer class="bg-black/80 py-8 px-6 border-t border-gray-900 relative z-10">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center text-gray-500 text-sm">
            <span class="font-display text-white text-xl tracking-widest">MN.</span>
            <span>© 2025 MAYBACH NAYAN — ALL RIGHTS RESERVED</span>
            <div class="flex gap-6 mt-4 md:mt-0">
                <a href="#" class="hover:text-maybach-gold">LI</a>
                <a href="#" class="hover:text-maybach-gold">X</a>
                <a href="#" class="hover:text-maybach-gold">IG</a>
            </div>
        </div>
    </footer>

    <script>
        (function(){
            // mobile menu toggle
            const menuBtn = document.getElementById('menuBtn');
            const mobileMenu = document.getElementById('mobileMenu');
            window.toggleMenu = function() {
                mobileMenu.classList.remove('open');
            };
            if (menuBtn) {
                menuBtn.addEventListener('click', () => {
                    mobileMenu.classList.toggle('open');
                });
            }
            // close mobile menu when clicking a link
            document.querySelectorAll('#mobileMenu a').forEach(link => {
                link.addEventListener('click', () => {
                    mobileMenu.classList.remove('open');
                });
            });

            // preloader
            window.addEventListener('load', () => {
                const bar = document.getElementById('loader-bar');
                const loader = document.getElementById('loader');
                gsap.to(bar, { width: '100%', duration: 1.8, ease: 'power2.inOut',
                    onComplete: () => {
                        gsap.to(loader, { y: '-100%', duration: 1.2, ease: 'power4.inOut', onComplete: init });
                    }
                });
            });

            function init() {
                // register GSAP plugins
                gsap.registerPlugin(ScrollTrigger, TextPlugin);

                // ---- Lenis smooth scroll ----
                const lenis = new Lenis({ lerp: 0.1, wheelMultiplier: 0.7 });
                function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
                requestAnimationFrame(raf);
                lenis.on('scroll', ScrollTrigger.update);
                gsap.ticker.add((time)=>{ lenis.raf(time * 1000); });
                gsap.ticker.lagSmoothing(0);

                // ---- advanced cursor with magnetic ----
                const cursorDot = document.getElementById('cursorDot');
                const cursorOutline = document.getElementById('cursorOutline');
                if (cursorDot && cursorOutline) {
                    document.addEventListener('mousemove', (e) => {
                        gsap.to(cursorDot, { x: e.clientX, y: e.clientY, duration: 0, overwrite: 'auto' });
                        gsap.to(cursorOutline, { x: e.clientX, y: e.clientY, duration: 0.4, ease: 'power2.out' });
                    });
                    // magnetic elements
                    document.querySelectorAll('.magnetic, a, button').forEach(el => {
                        el.addEventListener('mousemove', (e) => {
                            const rect = el.getBoundingClientRect();
                            const x = e.clientX - rect.left - rect.width/2;
                            const y = e.clientY - rect.top - rect.height/2;
                            gsap.to(el, { x: x*0.2, y: y*0.2, duration: 0.4, ease: 'power2.out' });
                            gsap.to(cursorOutline, { scale: 1.8, borderColor: '#D4AF37', background: 'rgba(212,175,55,0.1)', duration: 0.2 });
                        });
                        el.addEventListener('mouseleave', () => {
                            gsap.to(el, { x: 0, y: 0, duration: 0.5, ease: 'elastic.out(1,0.3)' });
                            gsap.to(cursorOutline, { scale: 1, borderColor: 'rgba(212,175,55,0.5)', background: 'transparent' });
                        });
                    });
                }

                // ---- Three.js advanced scene (floating orbs + gold mesh) ----
                const container = document.getElementById('canvas-container');
                if (container) {
                    const scene = new THREE.Scene();
                    scene.background = null;
                    const camera = new THREE.PerspectiveCamera(45, window.innerWidth/window.innerHeight, 0.1, 1000);
                    camera.position.set(0, 2, 18);
                    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true, powerPreference: "high-performance" });
                    renderer.setSize(window.innerWidth, window.innerHeight);
                    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
                    container.appendChild(renderer.domElement);

                    // lights
                    const ambient = new THREE.AmbientLight(0x404060);
                    const dirLight = new THREE.DirectionalLight(0xffffff, 1);
                    dirLight.position.set(2, 3, 4);
                    scene.add(ambient, dirLight);
                    const goldLight = new THREE.PointLight(0xD4AF37, 2, 20);
                    goldLight.position.set(-2, 1, 4);
                    scene.add(goldLight);

                    // main geometry — torus knot with wireframe + inner glow
                    const knotGeo = new THREE.TorusKnotGeometry(2, 0.6, 128, 16);
                    const knotMat = new THREE.MeshStandardMaterial({ color: 0xD4AF37, emissive: 0x332200, wireframe: true, transparent: true, opacity: 0.25 });
                    const knot = new THREE.Mesh(knotGeo, knotMat);
                    scene.add(knot);

                    // floating spheres
                    const sphereGroup = new THREE.Group();
                    for (let i = 0; i < 8; i++) {
                        const geo = new THREE.SphereGeometry(0.3 + Math.random()*0.2, 16);
                        const mat = new THREE.MeshStandardMaterial({ color: 0xD4AF37, emissive: 0x221100 });
                        const sphere = new THREE.Mesh(geo, mat);
                        const angle = (i / 8) * Math.PI*2;
                        sphere.position.set(Math.cos(angle)*5, Math.sin(angle*2)*1.5, Math.sin(angle)*4 - 2);
                        sphereGroup.add(sphere);
                    }
                    scene.add(sphereGroup);

                    // particles
                    const particleCount = 1200;
                    const posArray = new Float32Array(particleCount*3);
                    for (let i = 0; i < particleCount*3; i+=3) {
                        posArray[i] = (Math.random()-0.5)*30;
                        posArray[i+1] = (Math.random()-0.5)*20;
                        posArray[i+2] = (Math.random()-0.5)*30;
                    }
                    const particleGeo = new THREE.BufferGeometry();
                    particleGeo.setAttribute('position', new THREE.BufferAttribute(posArray, 3));
                    const particleMat = new THREE.PointsMaterial({ color: 0xffffff, size: 0.05, transparent: true, opacity: 0.4 });
                    const particles = new THREE.Points(particleGeo, particleMat);
                    scene.add(particles);

                    // mouse follow
                    let mouseX = 0, mouseY = 0;
                    document.addEventListener('mousemove', (e) => {
                        mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
                        mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
                    });

                    function animateThree() {
                        requestAnimationFrame(animateThree);
                        knot.rotation.x += 0.001;
                        knot.rotation.y += 0.002 + mouseX*0.003;
                        sphereGroup.rotation.y += 0.001;
                        sphereGroup.rotation.x += mouseY*0.002;
                        particles.rotation.y += 0.0002;
                        goldLight.position.x = mouseX*4;
                        goldLight.position.y = mouseY*3;
                        renderer.render(scene, camera);
                    }
                    animateThree();

                    window.addEventListener('resize', () => {
                        camera.aspect = window.innerWidth/window.innerHeight;
                        camera.updateProjectionMatrix();
                        renderer.setSize(window.innerWidth, window.innerHeight);
                    });
                }

                // ---- GSAP scroll animations ----
                // hero split & reveal
                gsap.to('.hero-reveal', { y:0, opacity:1, duration: 1.2, stagger: 0.2, delay: 0.3, ease: 'power3.out' });
                gsap.from('.hero-split', { y: 200, duration: 1.5, stagger: 0.1, ease: 'power4.out', delay: 0.2 });

                // about text/image
                gsap.from('.about-text-reveal', { scrollTrigger: '#about', x: -70, opacity: 0, duration: 1.2, ease: 'power3' });
                gsap.from('.about-img-reveal', { scrollTrigger: '#about', x: 70, opacity: 0, duration: 1.2, delay: 0.1, ease: 'power3' });

                // service cards
                gsap.from('.service-card', { scrollTrigger: '.service-card', y: 60, opacity: 0, duration: 1, stagger: 0.2, ease: 'back.out(1.2)' });

                // project cards
                gsap.from('.project-card', { scrollTrigger: '#work', x: 100, opacity: 0, duration: 1.2, stagger: 0.3, ease: 'power3' });

                // contact form
                gsap.from('#contact form > *', { scrollTrigger: '#contact', y: 40, opacity: 0, duration: 0.8, stagger: 0.15 });

                ScrollTrigger.refresh();
            }
        })();
    </script>
</body>
</html>
```
