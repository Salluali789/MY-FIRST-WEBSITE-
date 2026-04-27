<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Edu3D World - Visualize Your Learning</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
  <link href="https://cdn.jsdelivr.net/npm/@fontsource/inter@5.0.20/400.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@fontsource/inter@5.0.20/500.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@fontsource/inter@5.0.20/600.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@fontsource/inter@5.0.20/700.css" rel="stylesheet">
  <style>
    * { font-family: 'Inter', sans-serif; }
    
    .glass {
      background: rgba(255, 255, 255, 0.65);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.5);
    }
    
    .glass-dark {
      background: rgba(15, 23, 42, 0.55);
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
      border: 1px solid rgba(255, 255, 255, 0.15);
    }
    
    .glass-card {
      background: rgba(255, 255, 255, 0.72);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.6);
      transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
    }
    
    .glass-card:hover {
      transform: translateY(-6px);
      box-shadow: 0 20px 40px rgba(59, 130, 246, 0.18);
      border-color: rgba(59, 130, 246, 0.3);
      background: rgba(255, 255, 255, 0.85);
    }
    
    .gradient-bg {
      background: linear-gradient(135deg, #e0f2fe 0%, #f0f9ff 25%, #dbeafe 50%, #eff6ff 75%, #f8fafc 100%);
      background-size: 400% 400%;
      animation: gradientShift 15s ease infinite;
    }
    
    @keyframes gradientShift {
      0%, 100% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
    }
    
    .float-animation {
      animation: float 6s ease-in-out infinite;
    }
    
    @keyframes float {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-12px); }
    }
    
    .pulse-glow {
      animation: pulseGlow 3s ease-in-out infinite;
    }
    
    @keyframes pulseGlow {
      0%, 100% { box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.3); }
      50% { box-shadow: 0 0 0 12px rgba(59, 130, 246, 0); }
    }
    
    .slide-up {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.7s ease, transform 0.7s ease;
    }
    
    .slide-up.visible {
      opacity: 1;
      transform: translateY(0);
    }
    
    .category-icon {
      background: linear-gradient(135deg, #3b82f6 0%, #60a5fa 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    
    #canvas-container canvas {
      border-radius: 16px;
    }
    
    .sidebar-ad-sticky {
      position: sticky;
      top: 100px;
    }
    
    .ad-label {
      font-size: 9px;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: #94a3b8;
      font-weight: 600;
    }
    
    .btn-primary {
      background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
      transition: all 0.3s ease;
    }
    
    .btn-primary:hover {
      background: linear-gradient(135deg, #2563eb 0%, #1d4ed8 100%);
      transform: translateY(-2px);
      box-shadow: 0 8px 25px rgba(37, 99, 235, 0.35);
    }
    
    .quick-fact {
      border-left: 3px solid #3b82f6;
      transition: all 0.2s ease;
    }
    
    .quick-fact:hover {
      border-left-color: #1d4ed8;
      background: rgba(59, 130, 246, 0.06);
    }
    
    .nav-link {
      position: relative;
    }
    
    .nav-link::after {
      content: '';
      position: absolute;
      bottom: -4px;
      left: 50%;
      width: 0;
      height: 2px;
      background: #3b82f6;
      transition: all 0.3s ease;
      transform: translateX(-50%);
    }
    
    .nav-link:hover::after {
      width: 100%;
    }
    
    .modal-overlay {
      background: rgba(0, 0, 0, 0.5);
      backdrop-filter: blur(8px);
    }
    
    ::-webkit-scrollbar { width: 8px; }
    ::-webkit-scrollbar-track { background: #f1f5f9; }
    ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
  </style>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: { 50: '#eff6ff', 100: '#dbeafe', 200: '#bfdbfe', 300: '#93c5fd', 400: '#60a5fa', 500: '#3b82f6', 600: '#2563eb', 700: '#1d4ed8', 800: '#1e40af', 900: '#1e3a8a' }
          }
        }
      }
    }
  </script>
</head>
<body class="gradient-bg min-h-screen text-slate-800 antialiased">

  <!-- TOP BANNER AD -->
  <div id="top-ad" class="w-full bg-white/80 backdrop-blur-sm border-b border-slate-200/60 py-2 text-center relative overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 flex items-center justify-between">
      <span class="ad-label mr-3">Ad</span>
      <div class="flex-1 flex items-center justify-center gap-2">
        <svg class="w-4 h-4 text-primary-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
        <span class="text-sm text-slate-600 font-medium">🎓 Boost your grades with our Premium 3D Learning Plans — <a href="#" class="text-primary-600 hover:underline font-semibold">Get 50% off today!</a></span>
      </div>
      <button onclick="closeTopAd()" class="ml-3 text-slate-400 hover:text-slate-600 transition-colors p-1">
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
      </button>
    </div>
  </div>

  <!-- NAVBAR -->
  <nav class="glass sticky top-0 z-50 border-b border-white/40">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <a href="#" class="flex items-center gap-2">
          <div class="w-10 h-10 bg-gradient-to-br from-primary-500 to-primary-700 rounded-xl flex items-center justify-center shadow-lg shadow-primary-500/25">
            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"/></svg>
          </div>
          <span class="text-xl font-bold bg-gradient-to-r from-primary-600 to-primary-800 bg-clip-text text-transparent">Edu3D World</span>
        </a>
        <div class="hidden md:flex items-center gap-8">
          <a href="#home" class="nav-link text-sm font-medium text-slate-600 hover:text-primary-600 transition-colors">Home</a>
          <a href="#categories" class="nav-link text-sm font-medium text-slate-600 hover:text-primary-600 transition-colors">Subjects</a>
          <a href="#viewer" class="nav-link text-sm font-medium text-slate-600 hover:text-primary-600 transition-colors">3D Viewer</a>
          <a href="#" class="nav-link text-sm font-medium text-slate-600 hover:text-primary-600 transition-colors">Pricing</a>
        </div>
        <div class="flex items-center gap-3">
          <button class="hidden sm:inline-flex items-center px-4 py-2 text-sm font-medium text-primary-600 hover:text-primary-700 transition-colors">Log In</button>
          <button class="btn-primary text-white text-sm font-semibold px-5 py-2.5 rounded-xl shadow-lg">Sign Up Free</button>
          <button id="mobile-menu-btn" class="md:hidden p-2 rounded-lg hover:bg-white/50 transition-colors" onclick="toggleMobileMenu()">
            <svg class="w-6 h-6 text-slate-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/></svg>
          </button>
        </div>
      </div>
    </div>
    <!-- Mobile Menu -->
    <div id="mobile-menu" class="md:hidden hidden border-t border-white/40 bg-white/60 backdrop-blur-lg">
      <div class="px-4 py-4 space-y-3">
        <a href="#home" class="block py-2 text-sm font-medium text-slate-600 hover:text-primary-600">Home</a>
        <a href="#categories" class="block py-2 text-sm font-medium text-slate-600 hover:text-primary-600">Subjects</a>
        <a href="#viewer" class="block py-2 text-sm font-medium text-slate-600 hover:text-primary-600">3D Viewer</a>
        <a href="#" class="block py-2 text-sm font-medium text-slate-600 hover:text-primary-600">Pricing</a>
        <div class="pt-2 border-t border-slate-200/60 flex gap-3">
          <button class="flex-1 py-2.5 text-sm font-medium text-primary-600 border border-primary-200 rounded-xl hover:bg-primary-50 transition-colors">Log In</button>
          <button class="flex-1 py-2.5 text-sm font-semibold text-white btn-primary rounded-xl">Sign Up Free</button>
        </div>
      </div>
    </div>
  </nav>

  <!-- HERO SECTION -->
  <section id="home" class="relative overflow-hidden pt-16 pb-24 lg:pt-24 lg:pb-32">
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute -top-40 -right-40 w-96 h-96 bg-primary-200/30 rounded-full blur-3xl"></div>
      <div class="absolute top-20 -left-32 w-72 h-72 bg-sky-200/40 rounded-full blur-3xl"></div>
      <div class="absolute bottom-0 left-1/2 -translate-x-1/2 w-full h-32 bg-gradient-to-t from-white/60 to-transparent"></div>
    </div>
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
      <div class="grid lg:grid-cols-2 gap-12 items-center">
        <div class="text-center lg:text-left">
          <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-primary-100/60 border border-primary-200/50 text-primary-700 text-sm font-medium mb-6">
            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M11.3 1.046A1 1 0 0112 2v5h4a1 1 0 01.82 1.573l-7 10A1 1 0 018 18v-5H4a1 1 0 01-.82-1.573l7-10a1 1 0 011.12-.38z" clip-rule="evenodd"/></svg>
            AI-Powered 3D Learning
          </div>
          <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold leading-tight mb-6">
            <span class="bg-gradient-to-r from-slate-900 to-slate-700 bg-clip-text text-transparent">Visualize Your</span>
            <br>
            <span class="bg-gradient-to-r from-primary-600 to-sky-500 bg-clip-text text-transparent">Learning Journey</span>
          </h1>
          <p class="text-lg sm:text-xl text-slate-600 leading-relaxed mb-8 max-w-xl mx-auto lg:mx-0">
            Explore Science, Math & Arts through stunning 3D interactive models and AI-generated visuals. Make abstract concepts crystal clear.
          </p>
          <div class="flex flex-col sm:flex-row gap-4 justify-center lg:justify-start">
            <a href="#viewer" class="btn-primary text-white font-semibold px-8 py-4 rounded-2xl text-lg shadow-xl shadow-primary-500/25 flex items-center justify-center gap-2 pulse-glow">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
              Explore 3D Library
            </a>
            <button onclick="openRewardModal()" class="glass-card text-slate-700 font-semibold px-8 py-4 rounded-2xl text-lg flex items-center justify-center gap-2">
              <svg class="w-5 h-5 text-amber-500" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 000-2c4.418 0 6-1.582 6-6a1 1 0 10-2 0c0 3.418-.582 4-4 4a1 1 0 000 2z" clip-rule="evenodd"/></svg>
              Watch Demo
            </button>
          </div>
          <div class="mt-10 flex items-center gap-6 justify-center lg:justify-start">
            <div class="flex -space-x-3">
              <div class="w-10 h-10 rounded-full bg-gradient-to-br from-violet-400 to-violet-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">A</div>
              <div class="w-10 h-10 rounded-full bg-gradient-to-br from-pink-400 to-pink-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">R</div>
              <div class="w-10 h-10 rounded-full bg-gradient-to-br from-sky-400 to-sky-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">M</div>
              <div class="w-10 h-10 rounded-full bg-gradient-to-br from-emerald-400 to-emerald-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">+5k</div>
            </div>
            <div class="text-sm text-slate-600">
              <span class="font-semibold text-slate-800">5,000+</span> students learning daily
            </div>
          </div>
        </div>
        <div class="relative flex justify-center">
          <div class="relative w-full max-w-lg float-animation">
            <div class="glass-card rounded-3xl p-6 shadow-2xl shadow-primary-500/10">
              <div class="bg-gradient-to-br from-slate-100 to-slate-50 rounded-2xl p-4 mb-4">
                <div id="hero-canvas-container" class="w-full h-64 rounded-xl overflow-hidden bg-gradient-to-br from-primary-50 to-sky-50"></div>
              </div>
              <div class="flex items-center justify-between">
                <div>
                  <h3 class="font-semibold text-slate-800">3D Atom Model</h3>
                  <p class="text-sm text-slate-500">Physics • Interactive</p>
                </div>
                <span class="px-3 py-1 bg-green-100 text-green-700 text-xs font-semibold rounded-full">Free</span>
              </div>
            </div>
            <div class="absolute -top-4 -right-4 glass-card rounded-2xl p-3 shadow-lg animate-bounce" style="animation-duration: 3s;">
              <div class="flex items-center gap-2">
                <div class="w-8 h-8 bg-amber-100 rounded-lg flex items-center justify-center">
                  <svg class="w-4 h-4 text-amber-600" fill="currentColor" viewBox="0 0 20 20"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/></svg>
                </div>
                <div>
                  <p class="text-xs font-semibold text-slate-700">4.9/5 Rating</p>
                  <p class="text-xs text-slate-400">2,340 reviews</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- SUBJECT CATEGORIES -->
  <section id="categories" class="py-16 lg:py-24">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-14 slide-up">
        <span class="text-sm font-semibold text-primary-600 uppercase tracking-wider">Explore Subjects</span>
        <h2 class="text-3xl sm:text-4xl font-bold mt-3 bg-gradient-to-r from-slate-900 to-slate-700 bg-clip-text text-transparent">Choose Your Learning Path</h2>
        <p class="mt-4 text-slate-600 max-w-2xl mx-auto">Dive into interactive 3D models curated for each subject. From atoms to galaxies, from equations to masterpieces.</p>
      </div>

      <div class="grid sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 slide-up" id="category-grid">
        <!-- Physics Card -->
        <div class="glass-card rounded-2xl p-6 group cursor-pointer" onclick="selectCategory('physics')">
          <div class="w-14 h-14 bg-gradient-to-br from-violet-500 to-purple-700 rounded-2xl flex items-center justify-center mb-5 shadow-lg shadow-violet-500/25 group-hover:scale-110 transition-transform">
            <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"/></svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Physics</h3>
          <p class="text-sm text-slate-500 mb-4">3D Atoms, Forces, Waves & Quantum Models</p>
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-primary-600 bg-primary-50 px-2.5 py-1 rounded-full">42 Models</span>
            <svg class="w-5 h-5 text-slate-400 group-hover:text-primary-500 group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
          </div>
        </div>

        <!-- Biology Card -->
        <div class="glass-card rounded-2xl p-6 group cursor-pointer" onclick="selectCategory('biology')">
          <div class="w-14 h-14 bg-gradient-to-br from-emerald-500 to-green-700 rounded-2xl flex items-center justify-center mb-5 shadow-lg shadow-emerald-500/25 group-hover:scale-110 transition-transform">
            <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/></svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Biology</h3>
          <p class="text-sm text-slate-500 mb-4">3D Heart, DNA, Cells & Ecosystems</p>
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-emerald-600 bg-emerald-50 px-2.5 py-1 rounded-full">38 Models</span>
            <svg class="w-5 h-5 text-slate-400 group-hover:text-emerald-500 group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
          </div>
        </div>

        <!-- Math Card -->
        <div class="glass-card rounded-2xl p-6 group cursor-pointer" onclick="selectCategory('math')">
          <div class="w-14 h-14 bg-gradient-to-br from-blue-500 to-indigo-700 rounded-2xl flex items-center justify-center mb-5 shadow-lg shadow-blue-500/25 group-hover:scale-110 transition-transform">
            <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"/></svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Mathematics</h3>
          <p class="text-sm text-slate-500 mb-4">Geometric Shapes, Graphs & Fractals</p>
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-blue-600 bg-blue-50 px-2.5 py-1 rounded-full">56 Models</span>
            <svg class="w-5 h-5 text-slate-400 group-hover:text-blue-500 group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
          </div>
        </div>

        <!-- NATIVE AD CARD (Looks like a recommendation) -->
        <div class="glass-card rounded-2xl p-6 group cursor-pointer bg-gradient-to-br from-amber-50/60 to-orange-50/60 border-amber-200/50">
          <div class="flex items-center gap-1 mb-3">
            <span class="ad-label">Sponsored</span>
            <svg class="w-3 h-3 text-slate-400" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd"/></svg>
          </div>
          <div class="w-14 h-14 bg-gradient-to-br from-amber-400 to-orange-600 rounded-2xl flex items-center justify-center mb-5 shadow-lg shadow-amber-500/25">
            <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/></svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Khan Academy+</h3>
          <p class="text-sm text-slate-500 mb-4">Try the #1 learning companion app. Free 3D exercises for all grades.</p>
          <div class="inline-flex items-center gap-1 text-sm font-semibold text-amber-600 group-hover:text-amber-700">
            Learn More
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
          </div>
        </div>

        <!-- Arts Card -->
        <div class="glass-card rounded-2xl p-6 group cursor-pointer" onclick="selectCategory('arts')">
          <div class="w-14 h-14 bg-gradient-to-br from-pink-500 to-rose-700 rounded-2xl flex items-center justify-center mb-5 shadow-lg shadow-pink-500/25 group-hover:scale-110 transition-transform">
            <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01"/></svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Arts</h3>
          <p class="text-sm text-slate-500 mb-4">3D Sculptures, Architecture & Color Theory</p>
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-pink-600 bg-pink-50 px-2.5 py-1 rounded-full">31 Models</span>
            <svg class="w-5 h-5 text-slate-400 group-hover:text-pink-500 group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- 3D VIEWER AREA -->
  <section id="viewer" class="py-16 lg:py-24 bg-white/30">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12 slide-up">
        <span class="text-sm font-semibold text-primary-600 uppercase tracking-wider">Interactive Learning</span>
        <h2 class="text-3xl sm:text-4xl font-bold mt-3 bg-gradient-to-r from-slate-900 to-slate-700 bg-clip-text text-transparent">3D Model Viewer</h2>
        <p class="mt-4 text-slate-600 max-w-2xl mx-auto">Rotate, zoom, and explore 3D models. Click any subject above to load a model.</p>
      </div>

      <div class="grid lg:grid-cols-3 gap-8 slide-up">
        <!-- Main 3D Viewer -->
        <div class="lg:col-span-2">
          <div class="glass-card rounded-3xl p-6 shadow-xl">
            <div class="flex items-center justify-between mb-4">
              <div>
                <h3 id="viewer-title" class="text-xl font-bold text-slate-800">Select a Model to View</h3>
                <p id="viewer-subtitle" class="text-sm text-slate-500 mt-1">Click on a subject category above to load a 3D model</p>
              </div>
              <div class="flex gap-2">
                <button id="reset-camera-btn" onclick="resetCamera()" class="p-2.5 rounded-xl bg-white/60 hover:bg-white transition-colors border border-slate-200/60 text-slate-600" title="Reset View">
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/></svg>
                </button>
                <button id="fullscreen-btn" onclick="toggleFullscreen()" class="p-2.5 rounded-xl bg-white/60 hover:bg-white transition-colors border border-slate-200/60 text-slate-600" title="Fullscreen">
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 8V4m0 0h4M4 16v4m0 0h4m8-16v4m0 0h4m-4 8v4m0 0h4m-8 0h4"/></svg>
                </button>
              </div>
            </div>
            
            <!-- 3D Canvas Area -->
            <div class="relative rounded-2xl overflow-hidden bg-gradient-to-br from-slate-50 to-slate-100 border border-slate-200/60">
              <div id="canvas-container" class="w-full h-[400px] sm:h-[480px]">
                <!-- Three.js canvas will be injected here -->
              </div>
              
              <!-- Loading Overlay -->
              <div id="viewer-loading" class="absolute inset-0 bg-white/80 backdrop-blur-sm flex items-center justify-center">
                <div class="text-center">
                  <div class="w-12 h-12 border-4 border-primary-200 border-t-primary-600 rounded-full animate-spin mx-auto mb-4"></div>
                  <p class="text-sm text-slate-600 font-medium">Loading 3D Model...</p>
                </div>
              </div>

              <!-- Placeholder for Sketchfab Embed -->
              <!-- <iframe id="sketchfab-embed" class="w-full h-[400px] sm:h-[480px] border-0 rounded-2xl" src="https://sketchfab.com/models/YOUR_MODEL_ID/embed" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" style="display:none;"></iframe> -->
            </div>

            <!-- Controls -->
            <div class="flex items-center gap-3 mt-4 flex-wrap">
              <button onclick="rotateModel()" class="px-4 py-2 bg-white/60 hover:bg-white border border-slate-200/60 rounded-xl text-sm font-medium text-slate-700 transition-colors flex items-center gap-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/></svg>
                View Details
              </button>
              <button onclick="toggleWireframe()" class="px-4 py-2 bg-white/60 hover:bg-white border border-slate-200/60 rounded-xl text-sm font-medium text-slate-700 transition-colors flex items-center gap-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"/></svg>
                Wireframe
              </button>
              <button onclick="toggleAutoRotate()" class="px-4 py-2 bg-white/60 hover:bg-white border border-slate-200/60 rounded-xl text-sm font-medium text-slate-700 transition-colors flex items-center gap-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
                <span id="autorotate-label">Auto-Rotate</span>
              </button>
            </div>
          </div>
        </div>

        <!-- Sidebar -->
        <div class="space-y-6">
          <!-- Description Card -->
          <div class="glass-card rounded-2xl p-6">
            <h4 class="font-bold text-slate-800 mb-3 flex items-center gap-2">
              <svg class="w-5 h-5 text-primary-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
              Description
            </h4>
            <p id="model-description" class="text-sm text-slate-600 leading-relaxed">
              Select a category from above to load a 3D model. Each model comes with detailed descriptions, quick facts, and interactive exploration.
            </p>
          </div>

          <!-- Quick Facts -->
          <div class="glass-card rounded-2xl p-6">
            <h4 class="font-bold text-slate-800 mb-4 flex items-center gap-2">
              <svg class="w-5 h-5 text-amber-500" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"/></svg>
              Quick Facts
            </h4>
            <div id="quick-facts" class="space-y-3">
              <div class="quick-fact pl-4 py-2 bg-slate-50/50 rounded-r-lg">
                <p class="text-xs font-medium text-slate-500">Select a model</p>
              </div>
              <div class="quick-fact pl-4 py-2 bg-slate-50/50 rounded-r-lg">
                <p class="text-xs font-medium text-slate-500">to view quick facts</p>
              </div>
            </div>
          </div>

          <!-- SIDEBAR AD (Non-blocking) -->
          <div class="sidebar-ad-sticky">
            <div class="glass-card rounded-2xl p-5 bg-gradient-to-br from-indigo-50/60 to-blue-50/60 border-indigo-200/40">
              <span class="ad-label mb-2 block">Ad • Recommended</span>
              <div class="flex items-center gap-3 mb-3">
                <div class="w-12 h-12 bg-gradient-to-br from-indigo-500 to-blue-600 rounded-xl flex items-center justify-center">
                  <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/></svg>
                </div>
                <div>
                  <h5 class="font-semibold text-sm text-slate-800">3D Learning Pro</h5>
                  <p class="text-xs text-slate-500">All models unlocked</p>
                </div>
              </div>
              <p class="text-xs text-slate-600 mb-4">Access 500+ premium 3D models with AR support for immersive learning.</p>
              <button class="w-full py-2.5 bg-gradient-to-r from-indigo-600 to-blue-600 text-white text-sm font-semibold rounded-xl hover:shadow-lg hover:shadow-indigo-500/25 transition-all">
                Try Free for 7 Days
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- REWARD-BASED MODAL -->
  <div id="reward-modal" class="fixed inset-0 z-[100] hidden">
    <div class="modal-overlay absolute inset-0" onclick="closeRewardModal()"></div>
    <div class="relative z-10 flex items-center justify-center min-h-full p-4">
      <div class="glass bg-white/95 backdrop-blur-xl rounded-3xl p-8 max-w-md w-full shadow-2xl border border-white/60">
        <div class="text-center">
          <div class="w-16 h-16 bg-gradient-to-br from-amber-400 to-orange-500 rounded-2xl flex items-center justify-center mx-auto mb-5 shadow-lg shadow-amber-500/30">
            <svg class="w-8 h-8 text-white" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 100-2c4.418 0 6-1.582 6-6a1 1 0 10-2 0c0 3.418-.582 4-4 4a1 1 0 000 2z" clip-rule="evenodd"/></svg>
          </div>
          <h3 class="text-xl font-bold text-slate-800 mb-2">Watch a Short Video</h3>
          <p class="text-sm text-slate-600 mb-6">Watch a 15-second video to unlock this premium 3D model completely free. This helps us keep Edu3D World running for everyone!</p>
          <div class="space-y-3">
            <button onclick="startRewardAd()" class="w-full btn-primary text-white font-semibold py-3.5 rounded-2xl text-lg shadow-xl">
              🎬 Watch Video & Unlock
            </button>
            <button onclick="closeRewardModal()" class="w-full py-3 text-sm font-medium text-slate-500 hover:text-slate-700 transition-colors">
              Maybe Later
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- FOOTER -->
  <footer class="bg-slate-900/90 text-white pt-16 pb-8">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-10 mb-12">
        <div>
          <div class="flex items-center gap-2 mb-4">
            <div class="w-10 h-10 bg-gradient-to-br from-primary-500 to-primary-700 rounded-xl flex items-center justify-center">
              <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"/></svg>
            </div>
            <span class="text-xl font-bold">Edu3D World</span>
          </div>
          <p class="text-sm text-slate-400 leading-relaxed">Making education visual, interactive, and accessible for students worldwide through 3D technology.</p>
        </div>
        <div>
          <h4 class="font-semibold mb-4 text-white">Subjects</h4>
          <ul class="space-y-2.5">
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Physics 3D Models</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Biology 3D Models</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Math Visualizations</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Arts & Design</a></li>
          </ul>
        </div>
        <div>
          <h4 class="font-semibold mb-4 text-white">Resources</h4>
          <ul class="space-y-2.5">
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Teacher Dashboard</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">API Documentation</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Help Center</a></li>
            <li><a href="#" class="text-sm text-slate-400 hover:text-white transition-colors">Community Forum</a></li>
          </ul>
        </div>
        <div>
          <h4 class="font-semibold mb-4 text-white">Stay Updated</h4>
          <p class="text-sm text-slate-400 mb-4">Get new 3D models delivered weekly.</p>
          <div class="flex gap-2">
            <input type="email" placeholder="your@email.com" class="flex-1 px-4 py-2.5 bg-white/10 border border-white/15 rounded-xl text-sm text-white placeholder-slate-500 focus:outline-none focus:border-primary-500 transition-colors">
            <button class="px-5 py-2.5 bg-primary-600 hover:bg-primary-700 rounded-xl text-sm font-semibold transition-colors">Subscribe</button>
          </div>
        </div>
      </div>
      <div class="border-t border-white/10 pt-8 flex flex-col sm:flex-row items-center justify-between gap-4">
        <p class="text-sm text-slate-500">© 2026 Edu3D World. All rights reserved.</p>
        <div class="flex items-center gap-6">
          <a href="#" class="text-sm text-slate-500 hover:text-white transition-colors">Privacy</a>
          <a href="#" class="text-sm text-slate-500 hover:text-white transition-colors">Terms</a>
          <div class="flex items-center gap-3">
            <a href="#" class="text-slate-500 hover:text-white transition-colors"><svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"/></svg></a>
            <a href="#" class="text-slate-500 hover:text-white transition-colors"><svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0C5.374 0 0 5.373 0 12c0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 2.95 1.143.856-.237 1.78-.356 2.698-.36 0 0 .092.716.092 1.437 0 .995-.118 1.822-.118 1.822s2.026-.075 4.015-2.433c1.089-1.542 1.468-2.342 1.468-2.342s.38.757.38 1.977c0 .895-.025 1.612-.025 1.833 0 .182.122.397.395.333C17.495 17.484 20.5 14.487 20.5 10.485 20.5 6.095 16.91 2.54 12 2.54z"/></svg></a>
          </div>
        </div>
      </div>
    </div>
  </footer>

  <script>
    // Mobile menu toggle
    function toggleMobileMenu() {
      const menu = document.getElementById('mobile-menu');
      menu.classList.toggle('hidden');
    }

    // Close top ad
    function closeTopAd() {
      const ad = document.getElementById('top-ad');
      ad.style.transform = 'translateY(-100%)';
      ad.style.opacity = '0';
      ad.style.transition = 'all 0.3s ease';
      setTimeout(() => ad.style.display = 'none', 300);
    }

    // Modal functions
    function openRewardModal() {
      document.getElementById('reward-modal').classList.remove('hidden');
    }
    function closeRewardModal() {
      document.getElementById('reward-modal').classList.add('hidden');
    }
    function startRewardAd() {
      closeRewardModal();
      alert('🎬 In production, a 15-second video ad would play here. After watching, the premium 3D model unlocks!');
    }

    // Scroll animations
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
      });
    }, { threshold: 0.1 });

    document.querySelectorAll('.slide-up').forEach(el => observer.observe(el));

    // Three.js 3D Viewer Setup
    let scene, camera, renderer, currentMesh, wireframeMode = false, autoRotate = false;

    function initThreeJS() {
      const container = document.getElementById('canvas-container');
      const width = container.clientWidth;
      const height = container.clientHeight;

      scene = new THREE.Scene();
      scene.background = new THREE.Color(0xf0f9ff);

      camera = new THREE.PerspectiveCamera(50, width / height, 0.1, 1000);
      camera.position.z = 5;

      renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      renderer.setSize(width, height);
      renderer.setPixelRatio(window.devicePixelRatio);
      container.appendChild(renderer.domElement);

      // Lights
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
      scene.add(ambientLight);

      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
      directionalLight.position.set(5, 5, 5);
      scene.add(directionalLight);

      const pointLight = new THREE.PointLight(0x3b82f6, 0.5);
      pointLight.position.set(-3, 3, 3);
      scene.add(pointLight);

      // Default model: atom
      loadModel('physics');

      // Handle resize
      window.addEventListener('resize', onResize);
      animate();
    }

    function onResize() {
      const container = document.getElementById('canvas-container');
      const width = container.clientWidth;
      const height = container.clientHeight;
      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    }

    function animate() {
      requestAnimationFrame(animate);
      if (autoRotate && currentMesh) {
        currentMesh.rotation.y += 0.005;
        currentMesh.rotation.x += 0.002;
      }
      renderer.render(scene, camera);
    }

    function loadModel(type) {
      // Clear scene
      while(scene.children.length > 0) { 
        scene.remove(scene.children[0]); 
      }

      // Re-add lights
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
      scene.add(ambientLight);
      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
      directionalLight.position.set(5, 5, 5);
      scene.add(directionalLight);
      const pointLight = new THREE.PointLight(0x3b82f6, 0.5);
      pointLight.position.set(-3, 3, 3);
      scene.add(pointLight);

      let geometry, material;
      const titleEl = document.getElementById('viewer-title');
      const subtitleEl = document.getElementById('viewer-subtitle');
      const descEl = document.getElementById('model-description');
      const factsEl = document.getElementById('quick-facts');

      switch(type) {
        case 'physics':
          // Atom model: nucleus + electron orbits
          const group = new THREE.Group();
          
          // Nucleus
          const nucleusGeo = new THREE.SphereGeometry(0.4, 32, 32);
          const nucleusMat = new THREE.MeshPhysicalMaterial({ color: 0x8b5cf6, metalness: 0.3, roughness: 0.2 });
          const nucleus = new THREE.Mesh(nucleusGeo, nucleusMat);
          group.add(nucleus);

          // Protons and neutrons
          for(let i = 0; i < 3; i++) {
            const smallSphere = new THREE.SphereGeometry(0.2, 16, 16);
            const mat = new THREE.MeshPhysicalMaterial({ color: i < 2 ? 0x3b82f6 : 0x64748b, metalness: 0.2, roughness: 0.3 });
            const mesh = new THREE.Mesh(smallSphere, mat);
            mesh.position.set(Math.cos(i * 2.1) * 0.25, Math.sin(i * 2.1) * 0.25, Math.cos(i * 1.5) * 0.25);
            group.add(mesh);
          }

          // Electron orbits
          for(let i = 0; i < 3; i++) {
            const radius = 1.2 + i * 0.6;
            const orbitGeo = new THREE.TorusGeometry(radius, 0.015, 8, 100);
            const orbitMat = new THREE.MeshBasicMaterial({ color: 0x93c5fd, transparent: true, opacity: 0.4 });
            const orbit = new THREE.Mesh(orbitGeo, orbitMat);
            orbit.rotation.x = Math.PI / 2 + i * 0.4;
            orbit.rotation.y = i * 0.6;
            group.add(orbit);

            // Electron
            const electronGeo = new THREE.SphereGeometry(0.08, 16, 16);
            const electronMat = new THREE.MeshPhysicalMaterial({ color: 0x60a5fa, emissive: 0x3b82f6, emissiveIntensity: 0.5 });
            const electron = new THREE.Mesh(electronGeo, electronMat);
            electron.position.set(radius, 0, 0);
            group.add(electron);
          }

          currentMesh = group;
          scene.add(group);
          titleEl.textContent = '3D Atom Model';
          subtitleEl.textContent = 'Physics • Atomic Structure';
          descEl.textContent = 'Explore the atomic structure with protons, neutrons, and electrons in their orbital paths. Rotate the model to see electron shells from different angles.';
          factsEl.innerHTML = `
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">⚛️ Nucleus</p>
              <p class="text-xs text-slate-500 mt-0.5">Contains protons (+) and neutrons</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🔵 Electrons</p>
              <p class="text-xs text-slate-500 mt-0.5">Orbit in energy shells around nucleus</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">📐 Atomic Number</p>
              <p class="text-xs text-slate-500 mt-0.5">Equals the number of protons</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">⚖️ Mass Number</p>
              <p class="text-xs text-slate-500 mt-0.5">Protons + Neutrons in nucleus</p>
            </div>
          `;
          break;

        case 'biology':
          // Heart-like model
          const heartGroup = new THREE.Group();
          
          // Main heart shape using combined geometries
          const heartGeo = new THREE.SphereGeometry(1, 32, 32);
          const heartMat = new THREE.MeshPhysicalMaterial({ color: 0xe11d48, metalness: 0.1, roughness: 0.4 });
          const heart = new THREE.Mesh(heartGeo, heartMat);
          heart.scale.set(1, 1.1, 0.7);
          heartGroup.add(heart);

          // Aorta
          const aortaGeo = new THREE.CylinderGeometry(0.15, 0.2, 1, 16);
          const aortaMat = new THREE.MeshPhysicalMaterial({ color: 0xdc2626, metalness: 0.1, roughness: 0.5 });
          const aorta = new THREE.Mesh(aortaGeo, aortaMat);
          aorta.position.set(0, 1.2, 0);
          heartGroup.add(aorta);

          // Veins
          for(let i = 0; i < 3; i++) {
            const veinGeo = new THREE.CylinderGeometry(0.08, 0.06, 0.8, 12);
            const veinMat = new THREE.MeshPhysicalMaterial({ color: 0x2563eb, metalness: 0.1, roughness: 0.5 });
            const vein = new THREE.Mesh(veinGeo, veinMat);
            vein.position.set((i - 1) * 0.5, -1.1, 0.2);
            vein.rotation.z = (i - 1) * 0.3;
            heartGroup.add(vein);
          }

          // Chambers
          const chamberGeo = new THREE.SphereGeometry(0.4, 16, 16);
          const chamberMat = new THREE.MeshPhysicalMaterial({ color: 0xf43f5e, metalness: 0.05, roughness: 0.6 });
          for(let i = 0; i < 2; i++) {
            const chamber = new THREE.Mesh(chamberGeo, chamberMat);
            chamber.position.set((i - 0.5) * 0.8, 0.3, 0.4);
            chamber.scale.set(0.8, 1, 0.5);
            heartGroup.add(chamber);
          }

          currentMesh = heartGroup;
          scene.add(heartGroup);
          titleEl.textContent = '3D Heart Model';
          subtitleEl.textContent = 'Biology • Human Anatomy';
          descEl.textContent = 'A detailed 3D model of the human heart showing chambers, aorta, and major blood vessels. Use this to understand blood flow and cardiac anatomy.';
          factsEl.innerHTML = `
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">❤️ Size</p>
              <p class="text-xs text-slate-500 mt-0.5">About the size of your fist</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🔴 4 Chambers</p>
              <p class="text-xs text-slate-500 mt-0.5">2 atria (top) and 2 ventricles (bottom)</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">💓 Beats/Min</p>
              <p class="text-xs text-slate-500 mt-0.5">Average 72 beats per minute at rest</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🩸 Blood Flow</p>
              <p class="text-xs text-slate-500 mt-0.5">Pumps ~5 liters per minute</p>
            </div>
          `;
          break;

        case 'math':
          // Geometric shapes: icosahedron
          const mathGroup = new THREE.Group();
          
          // Main icosahedron
          const icoGeo = new THREE.IcosahedronGeometry(1.2, 1);
          const icoMat = new THREE.MeshPhysicalMaterial({ 
            color: 0x3b82f6, 
            metalness: 0.4, 
            roughness: 0.2,
            transparent: true,
            opacity: 0.85,
            side: THREE.DoubleSide
          });
          const ico = new THREE.Mesh(icoGeo, icoMat);
          mathGroup.add(ico);

          // Wireframe overlay
          const wireGeo = new THREE.IcosahedronGeometry(1.22, 1);
          const wireMat = new THREE.MeshBasicMaterial({ color: 0x60a5fa, wireframe: true, transparent: true, opacity: 0.3 });
          const wire = new THREE.Mesh(wireGeo, wireMat);
          mathGroup.add(wire);

          // Smaller shapes inside
          const smallOctGeo = new THREE.OctahedronGeometry(0.5, 0);
          const smallOctMat = new THREE.MeshPhysicalMaterial({ color: 0x8b5cf6, metalness: 0.5, roughness: 0.15 });
          const smallOct = new THREE.Mesh(smallOctGeo, smallOctMat);
          mathGroup.add(smallOct);

          // Vertices highlights
          const vertexGeo = new THREE.SphereGeometry(0.05, 8, 8);
          const vertexMat = new THREE.MeshBasicMaterial({ color: 0xfbbf24 });
          const positions = icoGeo.attributes.position;
          for(let i = 0; i < positions.count; i += 3) {
            const vertex = new THREE.Mesh(vertexGeo, vertexMat);
            vertex.position.set(positions.getX(i), positions.getY(i), positions.getZ(i));
            mathGroup.add(vertex);
          }

          currentMesh = mathGroup;
          scene.add(mathGroup);
          titleEl.textContent = 'Geometric Shapes';
          subtitleEl.textContent = 'Math • Platonic Solids';
          descEl.textContent = 'Explore an icosahedron with 20 faces, 30 edges, and 12 vertices. Platonic solids are regular, convex polyhedra with identical faces.';
          factsEl.innerHTML = `
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🔷 Faces</p>
              <p class="text-xs text-slate-500 mt-0.5">20 equilateral triangle faces</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">📏 Edges</p>
              <p class="text-xs text-slate-500 mt-0.5">30 edges connecting all vertices</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🔘 Vertices</p>
              <p class="text-xs text-slate-500 mt-0.5">12 vertices where edges meet</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🎲 Symmetry</p>
              <p class="text-xs text-slate-500 mt-0.5">5-fold rotational symmetry axes</p>
            </div>
          `;
          break;

        case 'arts':
          // Sculpture-like abstract art
          const artGroup = new THREE.Group();
          
          // Torus knot as abstract sculpture
          const knotGeo = new THREE.TorusKnotGeometry(1, 0.35, 128, 32, 2, 3);
          const knotMat = new THREE.MeshPhysicalMaterial({ 
            color: 0xec4899, 
            metalness: 0.6, 
            roughness: 0.15,
            clearcoat: 1.0,
            clearcoatRoughness: 0.1
          });
          const knot = new THREE.Mesh(knotGeo, knotMat);
          artGroup.add(knot);

          // Decorative spheres
          for(let i = 0; i < 8; i++) {
            const angle = (i / 8) * Math.PI * 2;
            const sphereGeo = new THREE.SphereGeometry(0.15, 16, 16);
            const sphereMat = new THREE.MeshPhysicalMaterial({ 
              color: new THREE.Color().setHSL(i / 8, 0.7, 0.6), 
              metalness: 0.3, 
              roughness: 0.2 
            });
            const sphere = new THREE.Mesh(sphereGeo, sphereMat);
            sphere.position.set(Math.cos(angle) * 2, Math.sin(angle * 2) * 0.5, Math.sin(angle) * 2);
            artGroup.add(sphere);
          }

          // Base platform
          const baseGeo = new THREE.CylinderGeometry(1.5, 1.6, 0.15, 32);
          const baseMat = new THREE.MeshPhysicalMaterial({ color: 0x1e293b, metalness: 0.8, roughness: 0.2 });
          const base = new THREE.Mesh(baseGeo, baseMat);
          base.position.y = -1.5;
          artGroup.add(base);

          currentMesh = artGroup;
          scene.add(artGroup);
          titleEl.textContent = '3D Abstract Sculpture';
          subtitleEl.textContent = 'Arts • Digital Sculpture';
          descEl.textContent = 'An abstract torus knot sculpture with colorful accent spheres. Explore how mathematical curves create beautiful artistic forms.';
          factsEl.innerHTML = `
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🎨 Torus Knot</p>
              <p class="text-xs text-slate-500 mt-0.5">A curve that winds around a torus</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🌀 Parameters</p>
              <p class="text-xs text-slate-500 mt-0.5">p=2, q=3 creates this shape</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🌈 Colors</p>
              <p class="text-xs text-slate-500 mt-0.5">HSL color wheel on accent spheres</p>
            </div>
            <div class="quick-fact pl-4 py-2.5 bg-slate-50/50 rounded-r-lg">
              <p class="text-xs font-semibold text-slate-700">🏛️ Art + Math</p>
              <p class="text-xs text-slate-500 mt-0.5">Where geometry meets creativity</p>
            </div>
          `;
          break;
      }

      // Reset wireframe
      wireframeMode = false;
      
      // Hide loading
      document.getElementById('viewer-loading').style.opacity = '0';
      document.getElementById('viewer-loading').style.pointerEvents = 'none';
    }

    function selectCategory(type) {
      const loadingEl = document.getElementById('viewer-loading');
      loadingEl.style.opacity = '1';
      loadingEl.style.pointerEvents = 'auto';
      
      setTimeout(() => loadModel(type), 400);
      
      // Scroll to viewer
      document.getElementById('viewer').scrollIntoView({ behavior: 'smooth' });
    }

    function resetCamera() {
      camera.position.set(0, 0, 5);
      camera.lookAt(0, 0, 0);
    }

    function toggleFullscreen() {
      const container = document.getElementById('canvas-container');
      if (!document.fullscreenElement) {
        container.requestFullscreen().catch(() => {});
      } else {
        document.exitFullscreen();
      }
    }

    function rotateModel() {
      if (currentMesh) {
        currentMesh.rotation.y += Math.PI / 2;
      }
    }

    function toggleWireframe() {
      if (!currentMesh) return;
      wireframeMode = !wireframeMode;
      currentMesh.traverse((child) => {
        if (child.isMesh && child.material) {
          child.material.wireframe = wireframeMode;
        }
      });
    }

    function toggleAutoRotate() {
      autoRotate = !autoRotate;
      const label = document.getElementById('autorotate-label');
      label.textContent = autoRotate ? 'Stop Rotation' : 'Auto-Rotate';
    }

    // Initialize
    document.addEventListener('DOMContentLoaded', () => {
      initThreeJS();
    });
  </script>
</body>
</html>

