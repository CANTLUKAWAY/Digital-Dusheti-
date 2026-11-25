# Digital-Dusheti-
<!DOCTYPE html>
<html lang="ka">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DIGITAL DUSHETI — ციფრული დუშეთი</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+Georgian:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin="" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin=""></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            civic: {
              900: '#0F254A',
              800: '#1A3668',
              700: '#274985'
            },
            sun: {
              500: '#F5C542',
              600: '#D9A927',
              100: '#FEF7E0'
            }
          },
          fontFamily: {
            sans: ['"Noto Sans Georgian"', 'Inter', 'sans-serif']
          }
        }
      }
    };
  </script>
  <style>
    body {
      font-family: 'Noto Sans Georgian', 'Inter', sans-serif;
      background-color: #FFFFFF;
      color: #0F254A;
      overflow-x: hidden;
    }
    .portal-card {
      background: #FFFFFF;
      border: 1px solid #E2E8F0;
      border-radius: 1.5rem;
      transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
    }
    .portal-card:hover {
      border-color: #CBD5E1;
      box-shadow: 0 14px 30px -14px rgba(15, 37, 74, 0.14);
      transform: translateY(-2px);
    }
    .blur-sensitive {
      filter: blur(5px);
      user-select: none;
      transition: filter 0.2s ease;
    }
    .blur-sensitive.unblurred,
    .blur-sensitive:hover {
      filter: blur(0);
    }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    .leaflet-container {
      font-family: 'Noto Sans Georgian', 'Inter', sans-serif !important;
      z-index: 10;
    }
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fade-in { animation: fadeInUp 0.24s ease-out forwards; }
  </style>
</head>
<body class="bg-white text-[#0F254A] antialiased selection:bg-[#F5C542] selection:text-[#0F254A]">

  <!-- ===================================================================== -->
  <!-- 1. INITIAL LOGIN / REGISTRATION SCREEN                                -->
  <!-- ===================================================================== -->
  <section id="auth-view" class="min-h-screen bg-white flex flex-col justify-between">
    <header class="border-b border-slate-100 px-4 sm:px-8 py-4">
      <div class="max-w-6xl mx-auto flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div id="auth-crest-top"></div>
          <div>
            <div class="font-extrabold text-lg tracking-tight text-[#0F254A]">
              DIGITAL <span class="text-[#D9A927]">DUSHETI</span>
            </div>
            <div class="text-xs text-slate-500" data-i18n="portalSub">ციფრული დუშეთი</div>
          </div>
        </div>
        <button type="button" onclick="toggleLang()" class="px-3.5 py-2 rounded-xl bg-slate-50 hover:bg-slate-100 border border-slate-200 text-xs font-extrabold text-[#0F254A] flex items-center gap-1.5 transition">
          <span class="w-2 h-2 rounded-full bg-[#F5C542]"></span>
          <span id="auth-lang-badge">GE</span>
        </button>
      </div>
    </header>

    <main class="flex-grow flex items-center justify-center px-4 py-12">
      <div class="max-w-md w-full bg-white rounded-3xl p-8 sm:p-10 border border-slate-200 shadow-xl shadow-slate-200/50 animate-fade-in">
        <div class="flex flex-col items-center text-center mb-7">
          <div id="auth-crest-center" class="mb-4"></div>
          <h1 class="text-2xl sm:text-3xl font-extrabold text-[#0F254A]" data-i18n="loginTitle">მოგესალმებით დუშეთის პორტალზე</h1>
          <p class="text-sm text-slate-500 mt-2" data-i18n="loginDesc">შესასვლელად შეიყვანეთ მხოლოდ თქვენი სახელი და გვარი</p>
        </div>

        <form id="auth-form" onsubmit="return handleLogin(event)" class="space-y-4">
          <div>
            <label for="login-first-name" class="block text-xs font-bold text-slate-700 mb-1.5" data-i18n="firstName">სახელი</label>
            <input id="login-first-name" type="text" required autocomplete="given-name" placeholder="მაგ: ნინო" data-i18n-ph="firstNamePh" class="w-full rounded-2xl border border-slate-200 bg-slate-50/70 focus:bg-white px-4 py-3 text-sm text-[#0F254A] focus:outline-none focus:border-[#0F254A] transition" />
          </div>
          <div>
            <label for="login-last-name" class="block text-xs font-bold text-slate-700 mb-1.5" data-i18n="lastName">გვარი</label>
            <input id="login-last-name" type="text" required autocomplete="family-name" placeholder="მაგ: კაპანაძე" data-i18n-ph="lastNamePh" class="w-full rounded-2xl border border-slate-200 bg-slate-50/70 focus:bg-white px-4 py-3 text-sm text-[#0F254A] focus:outline-none focus:border-[#0F254A] transition" />
          </div>
          <p id="login-error" class="hidden text-xs font-bold text-red-600"></p>
          <button id="login-btn" type="submit" class="w-full py-3.5 rounded-2xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-sm transition shadow-md flex items-center justify-center gap-2">
            <span data-i18n="loginCta">შესვლა / Log In</span>
            <svg class="w-4 h-4 text-[#F5C542]" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M13 7l5 5m0 0l-5 5m5-5H6" />
            </svg>
          </button>
        </form>
      </div>
    </main>

    <footer class="border-t border-slate-100 py-5 px-4 text-center text-xs text-slate-600 space-y-1">
      <div class="font-extrabold text-[#0F254A]">Project made by VOG</div>
      <div class="font-semibold">პროექტი შექმნილია “თაობათა ხმის” მიერ</div>
    </footer>
  </section>

  <!-- ===================================================================== -->
  <!-- 2. MAIN MUNICIPAL PORTAL APPLICATION                                  -->
  <!-- ===================================================================== -->
  <div id="app-shell" class="hidden flex-col min-h-screen bg-white">
    <header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-slate-200/80">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-20 gap-3">
          <button type="button" onclick="goPage('home')" class="flex items-center gap-3 text-left shrink-0 focus:outline-none">
            <div id="nav-crest"></div>
            <div>
              <div class="font-extrabold text-lg sm:text-xl tracking-tight leading-none text-[#0F254A]">
                DIGITAL <span class="text-[#D9A927]">DUSHETI</span>
              </div>
              <div class="text-[11px] text-slate-500 font-medium mt-1" data-i18n="portalSub">ციფრული დუშეთი</div>
            </div>
          </button>

          <nav class="hidden lg:flex items-center gap-1 overflow-x-auto no-scrollbar">
            <button type="button" onclick="goPage('home')" data-nav="home" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navHome">მთავარი</button>
            <button type="button" onclick="goPage('services')" data-nav="services" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navServices">სერვისები</button>
            <button type="button" onclick="goPage('map')" data-nav="map" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navMap">რუკა</button>
            <button type="button" onclick="goPage('help')" data-nav="help" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navHelp">დაეხმარე დუშეთს</button>
            <button type="button" onclick="goPage('petitions')" data-nav="petitions" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navPetitions">განცხადებები</button>
            <button type="button" onclick="goPage('news')" data-nav="news" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navNews">სიახლეები</button>
            <button type="button" onclick="goPage('health')" data-nav="health" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navHealth">ჯანმრთელობა</button>
            <button type="button" onclick="goPage('jobs')" data-nav="jobs" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navJobs">დასაქმება</button>
            <button type="button" onclick="goPage('edu')" data-nav="edu" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navEdu">განათლება</button>
            <button type="button" onclick="goPage('contact')" data-nav="contact" class="nav-link px-2.5 py-2 rounded-xl text-xs font-bold transition" data-i18n="navContact">კონტაქტი</button>
          </nav>

          <div class="flex items-center gap-2 shrink-0">
            <button type="button" onclick="toggleLang()" class="px-2.5 py-2 rounded-xl bg-slate-50 hover:bg-slate-100 border border-slate-200 text-xs font-extrabold text-[#0F254A] flex items-center gap-1.5 transition">
              <span class="w-2 h-2 rounded-full bg-[#F5C542]"></span>
              <span id="nav-lang-badge">GE</span>
            </button>

            <div class="hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-xl bg-slate-50 border border-slate-200">
              <span id="nav-user-name" class="text-xs font-bold text-[#0F254A] max-w-[125px] truncate"></span>
              <button type="button" onclick="logoutUser()" class="text-[11px] font-bold text-red-600 hover:underline" data-i18n="logout">გასვლა</button>
            </div>

            <button type="button" onclick="openAdminModal()" class="px-2 py-1.5 rounded-lg bg-slate-100 hover:bg-[#FEF7E0] text-slate-600 hover:text-[#0F254A] border border-slate-200 text-[10px] font-extrabold tracking-wider transition">
              ADM
            </button>

            <button type="button" onclick="toggleMobileMenu()" class="lg:hidden p-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-[#0F254A]" aria-label="Menu">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
              </svg>
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile Navigation Drawer -->
      <div id="mobile-menu" class="hidden lg:hidden bg-white border-t border-slate-200 px-4 py-4 space-y-3">
        <div class="flex items-center justify-between pb-2 border-b border-slate-100">
          <span id="mobile-user-name" class="text-xs font-bold text-[#0F254A]"></span>
          <button type="button" onclick="logoutUser()" class="text-xs font-bold text-red-600" data-i18n="logout">გასვლა</button>
        </div>
        <div class="grid grid-cols-2 gap-2">
          <button type="button" onclick="goPage('home'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navHome">მთავარი</button>
          <button type="button" onclick="goPage('services'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navServices">სერვისები</button>
          <button type="button" onclick="goPage('map'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navMap">რუკა</button>
          <button type="button" onclick="goPage('help'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-[#FEF7E0] text-xs font-bold" data-i18n="navHelp">დაეხმარე დუშეთს</button>
          <button type="button" onclick="goPage('petitions'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navPetitions">განცხადებები</button>
          <button type="button" onclick="goPage('news'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navNews">სიახლეები</button>
          <button type="button" onclick="goPage('health'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navHealth">ჯანმრთელობა</button>
          <button type="button" onclick="goPage('jobs'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navJobs">დასაქმება</button>
          <button type="button" onclick="goPage('edu'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navEdu">განათლება</button>
          <button type="button" onclick="goPage('contact'); toggleMobileMenu()" class="text-left px-3 py-2 rounded-xl bg-slate-50 text-xs font-bold" data-i18n="navContact">კონტაქტი</button>
        </div>
      </div>
    </header>

    <main id="page-container" class="flex-grow bg-white"></main>

    <footer class="bg-slate-50 border-t border-slate-200 mt-16">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
        <div class="flex flex-col sm:flex-row items-center justify-between gap-4 pb-8 border-b border-slate-200">
          <div class="flex items-center gap-3">
            <div id="footer-crest"></div>
            <div>
              <div class="font-extrabold text-[#0F254A]">DIGITAL DUSHETI</div>
              <div class="text-xs text-slate-500" data-i18n="footerSub">დუშეთის მუნიციპალიტეტის ერთიანი ციფრული პორტალი</div>
            </div>
          </div>
          <div class="flex flex-wrap items-center gap-3">
            <div class="px-4 py-2 rounded-xl bg-[#FEF7E0] border border-[#F5C542] text-[#0F254A] text-xs font-extrabold flex items-center gap-2">
              <svg class="w-4 h-4 text-[#D9A927]" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 21s-6-4.35-9-9a5.25 5.25 0 019-5.25A5.25 5.25 0 0121 12c-3 4.65-9 9-9 9z" />
              </svg>
              <span data-i18n="line111">111 — ბავშვთა დაცვის ცხელი ხაზი</span>
            </div>
            <div aria-disabled="true" class="px-4 py-2 rounded-xl bg-red-600 text-white text-xs font-extrabold select-none cursor-default" data-i18n="line112">
              112 — გადაუდებელი დახმარება
            </div>
          </div>
        </div>
        <div class="pt-6 flex flex-col sm:flex-row items-center justify-between gap-2 text-xs text-slate-600">
          <div class="font-extrabold text-[#0F254A]">Project made by VOG</div>
          <div class="font-semibold">პროექტი შექმნილია “თაობათა ხმის” მიერ</div>
        </div>
      </div>
    </footer>
  </div>

  <!-- Toast Notifications & Modal Dialog -->
  <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 max-w-sm"></div>
  <div id="modal-backdrop" class="hidden fixed inset-0 bg-[#0F254A]/50 backdrop-blur-sm z-50 flex items-center justify-center p-4 overflow-y-auto">
    <div class="bg-white rounded-3xl max-w-2xl w-full p-6 sm:p-8 border border-slate-200 shadow-2xl relative my-8 max-h-[90vh] overflow-y-auto animate-fade-in">
      <button type="button" onclick="closeModal()" aria-label="Close" class="absolute top-5 right-5 w-9 h-9 rounded-full bg-slate-100 hover:bg-slate-200 text-[#0F254A] flex items-center justify-center font-bold">✕</button>
      <div id="modal-content"></div>
    </div>
  </div>

  <script>
    // =========================================================================
    // 1. DUSHETI OFFICIAL COAT OF ARMS SVG
    // =========================================================================
    function renderDushetiCrest(sizeClass) {
      return `
        <svg viewBox="0 0 120 140" fill="none" xmlns="http://www.w3.org/2000/svg" class="${sizeClass} shrink-0">
          <path d="M24 34L20 14H29V19H35V14H44L41 34H24Z" fill="#E2E8F0" stroke="#0F254A" stroke-width="2" stroke-linejoin="round"/>
          <path d="M49 32L46 10H55V15H65V10H74L71 32H49Z" fill="#F8FAFC" stroke="#0F254A" stroke-width="2" stroke-linejoin="round"/>
          <rect x="55" y="19" width="10" height="13" rx="5" fill="#64748B"/>
          <path d="M79 34L76 14H85V19H91V14H100L96 34H79Z" fill="#E2E8F0" stroke="#0F254A" stroke-width="2" stroke-linejoin="round"/>
          <path d="M26 33C48 29 72 29 94 33L92 40C71 36 49 36 28 40L26 33Z" fill="#CBD5E1" stroke="#0F254A" stroke-width="2"/>
          <path d="M28 41H60V116C42 108 28 93 28 76V41Z" fill="#C81E1E"/>
          <path d="M60 41H92V76C92 93 78 108 60 116V41Z" fill="#111827"/>
          <path d="M28 41H92V76C92 93 78 108 60 116C42 108 28 93 28 76V41Z" stroke="#0F254A" stroke-width="2.5"/>
          <path d="M29 50L36 55L44 49L52 55L59 50" stroke="#FFFFFF" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M36 44V48M34 46H38M44 43V47M42 45H46M52 44V48M50 46H54M40 56V60M38 58H42M48 56V60M46 58H50" stroke="#FFFFFF" stroke-width="1.6"/>
          <path d="M53 60C53 54 67 54 67 60V68L81 72V83L70 81V98C65 102 55 102 50 98V81L39 83V72L53 68V60Z" fill="#F5C542" stroke="#78350F" stroke-width="1.8" stroke-linejoin="round"/>
          <rect x="56" y="58" width="8" height="10" rx="4" fill="#D9A927" stroke="#78350F" stroke-width="1.2"/>
          <rect x="38" y="83" width="44" height="3.5" rx="1.5" fill="#F8FAFC" stroke="#0F254A" stroke-width="1.2"/>
          <rect x="75" y="79" width="2.5" height="11" fill="#F5C542" stroke="#78350F" stroke-width="1"/>
          <circle cx="60" cy="85" r="9.5" fill="#FDE047" stroke="#78350F" stroke-width="1.8"/>
          <circle cx="60" cy="85" r="4.5" fill="#F5C542" stroke="#78350F" stroke-width="1.2"/>
          <path d="M18 118C38 128 82 128 102 118L97 130C78 138 42 138 23 130L18 118Z" fill="#F8FAFC" stroke="#0F254A" stroke-width="2"/>
          <text x="60" y="128" text-anchor="middle" fill="#0F254A" font-size="8.5" font-weight="800">დუშეთი</text>
        </svg>
      `;
    }

    // =========================================================================
    // 2. FULL GE / EN TRANSLATION TABLE
    // =========================================================================
    const translations = {
      ka: {
        portalSub: "ციფრული დუშეთი",
        loginTitle: "მოგესალმებით დუშეთის პორტალზე",
        loginDesc: "შესასვლელად შეიყვანეთ მხოლოდ თქვენი სახელი და გვარი",
        firstName: "სახელი",
        lastName: "გვარი",
        firstNamePh: "მაგ: ნინო",
        lastNamePh: "მაგ: კაპანაძე",
        loginCta: "შესვლა / Log In",
        logout: "გასვლა",
        navHome: "მთავარი",
        navServices: "სერვისები",
        navMap: "რუკა",
        navHelp: "დაეხმარე დუშეთს",
        navPetitions: "განცხადებები",
        navNews: "სიახლეები",
        navHealth: "ჯანმრთელობა",
        navJobs: "დასაქმება",
        navEdu: "განათლება",
        navContact: "კონტაქტი",
        heroTitle: "დუშეთი — თანამედროვე ქალაქი, საერთო პასუხისმგებლობით",
        heroSub: "წყლისა და კომუნალური გადასახადები, 18 ზუსტი ლოკაციის რუკა, სკოლების კატალოგი, დასაქმება და მოქალაქეთა ჩართულობა ერთ ციფრულ პორტალში.",
        ctaServices: "აღმოაჩინე სერვისები",
        ctaMap: "გახსენი დუშეთის რუკა",
        searchPh: "მოძებნე სერვისი, სკოლა, ვაკანსია ან ლოკაცია...",
        quickServices: "სწრაფი მუნიციპალური სერვისები",
        quickServicesSub: "აირჩიეთ სასურველი გვერდი პორტალის ფუნქციებით სარგებლობისთვის",
        readFull: "სრულად წაკითხვა",
        viewFull: "სრულად ნახვა",
        footerSub: "დუშეთის მუნიციპალიტეტის ერთიანი ციფრული პორტალი",
        line111: "111 — ბავშვთა დაცვის ცხელი ხაზი",
        line112: "112 — გადაუდებელი დახმარება"
      },
      en: {
        portalSub: "Digital Municipality Portal",
        loginTitle: "Welcome to Digital Dusheti",
        loginDesc: "Enter your first and last name to access the municipal portal",
        firstName: "First Name",
        lastName: "Last Name",
        firstNamePh: "e.g. Nino",
        lastNamePh: "e.g. Kapanadze",
        loginCta: "Log In / შესვლა",
        logout: "Log Out",
        navHome: "Home",
        navServices: "Services",
        navMap: "Smart Map",
        navHelp: "Help Dusheti",
        navPetitions: "Statements",
        navNews: "News",
        navHealth: "Health",
        navJobs: "Jobs",
        navEdu: "Education",
        navContact: "Contact",
        heroTitle: "Dusheti — A Modern City with Shared Responsibility",
        heroSub: "Water & utility payments, 18 verified Dusheti map places, schools directory, jobs, and citizen engagement in one clean portal.",
        ctaServices: "Explore Services",
        ctaMap: "Open Dusheti Map",
        searchPh: "Search services, schools, jobs, or map places...",
        quickServices: "Quick Municipal Services",
        quickServicesSub: "Choose a module to access digital municipal services",
        readFull: "Read Full Story",
        viewFull: "View Details",
        footerSub: "Unified Digital Portal of Dusheti Municipality",
        line111: "111 — Child Protection Hotline",
        line112: "112 — Emergency Assistance"
      }
    };

    function getGoogleMapsLink(lat, lng, label) {
      if (typeof lat === "number" && typeof lng === "number" && !isNaN(lat) && !isNaN(lng)) {
        return `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(lat + "," + lng)}`;
      }
      return `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent((label || "") + ", Dusheti, Georgia")}`;
    }

    // =========================================================================
    // 3. STATE & INITIAL DATA (18 VERIFIED DUSHETI LOCATIONS + MUNICIPAL DATA)
    // =========================================================================
    const state = {
      lang: "ka",
      user: null,
      adminUnlocked: false,
      page: "home",
      searchQuery: "",
      selectedService: "წყლის გადასახადი",
      blurPayments: true,
      clickPlaceMode: false,
      map: null,
      layers: {
        schools: true,
        civic: true,
        health: true,
        churches: true,
        tourism: true,
        alerts: true
      },
      syncUrl: "",
      heroImage: "https://images.unsplash.com/photo-1565008576549-57569a49371d?auto=format&fit=crop&w=1400&q=80",
      payments: [
        { id: "REC-3011", payer: "თამარ ჯავახიშვილი", service: "წყლის გადასახადი", account: "409128", amount: "14.50 ₾", date: "2026-11-24" },
        { id: "REC-3008", payer: "ლევან წიკლაური", service: "ელექტროენერგია", account: "772104", amount: "42.80 ₾", date: "2026-11-22" }
      ],
      applications: [
        { id: "APP-104", applicant: "მარიამ ბურდული", subject: "გარე განათების მოწესრიგება", location: "ქ. დუშეთი, რუსთაველის ქ. №27", status: "განხილვაშია", date: "2026-11-24" }
      ],
      problems: [
        {
          id: 1,
          author: "ნინო ჭინჭარაული",
          category: "ინფრასტრუქტურა",
          categoryEn: "Infrastructure",
          title: "საფეხმავლო ბილიკისა და ასფალტის მონაკვეთის მოწესრიგება",
          titleEn: "Walkway and Road Surface Repair",
          location: "ქ. დუშეთი, ცენტრალური უბნის მიმდებარედ",
          desc: "საჭიროა ადგილობრივი საგზაო მონაკვეთისა და ტროტუარის ფრაგმენტის განახლება ფეხით მოსიარულეთა უსაფრთხოებისთვის.",
          descEn: "Local roadway and walkway repair is needed for safe pedestrian movement.",
          photo: "https://images.unsplash.com/photo-1515162816999-a0c47dc192f7?auto=format&fit=crop&w=900&q=80",
          date: "2026-11-24",
          published: true
        },
        {
          id: 2,
          author: "გიგა მთივლიშვილი",
          category: "დასუფთავება",
          categoryEn: "Sanitation",
          title: "ნარჩენების ურნების განახლება და რეგულარული გატანა",
          titleEn: "Waste Bins Upgrade and Collection",
          location: "დუშეთის მუნიციპალიტეტი",
          desc: "დამატებითი ურნების განთავსება და სანიტარული გრაფიკის დაცვა საზოგადოებრივი სივრცის სისუფთავისთვის.",
          descEn: "Adding bins and maintaining collection schedules to keep public areas clean.",
          photo: "https://images.unsplash.com/photo-1530587191325-3db32d826c18?auto=format&fit=crop&w=900&q=80",
          date: "2026-11-21",
          published: true
        }
      ],
      petitions: [
        {
          id: 1,
          author: "სალომე მ.",
          title: "ახალგაზრდული კულტურული და საგანმანათლებლო შეხვედრების სივრცე დუშეთის ცენტრალურ პარკში",
          titleEn: "Youth Cultural & Educational Space in Dusheti Central Park",
          desc: "ინიციატივა დუშეთის ცენტრალურ პარკში (3MMX+X2V) ახალგაზრდული ღონისძიებებისა და შეხვედრების ორგანიზების შესახებ.",
          descEn: "Proposal to host youth cultural and educational events in Dusheti Central Park.",
          votes: 162,
          supported: false,
          date: "2026-11-23"
        },
        {
          id: 2,
          author: "დავით გ.",
          title: "უსაფრთხო საფეხმავლო და სარეკრეაციო ბილიკი ბაზალეთის ტბის მიმართულებით",
          titleEn: "Safe Pedestrian & Recreation Path Toward Bazaleti Lake",
          desc: "ბაზალეთის ტბის (42.0378, 44.6793) მიმდებარედ უსაფრთხო გადაადგილებისა და დასვენების პირობების გაუმჯობესება.",
          descEn: "Improving safe walking and recreational access near Bazaleti Lake.",
          votes: 214,
          supported: false,
          date: "2026-11-19"
        }
      ],
      news: [
        {
          id: 1,
          published: true,
          author: "პრესსამსახური",
          date: "2026-11-25",
          tag: "ინფრასტრუქტურა",
          tagEn: "Infrastructure",
          title: "დუშეთის ცენტრალური პარკისა და საზოგადოებრივი სივრცეების კეთილმოწყობა",
          titleEn: "Rehabilitation of Dusheti Central Park & Public Spaces",
          photo: "https://images.unsplash.com/photo-1585320806297-9794b3e4eeae?auto=format&fit=crop&w=1000&q=80",
          excerpt: "განახლდა საფეხმავლო ბილიკები, განათება და მოქალაქეთა დასვენების ზონები.",
          excerptEn: "Walkways, lighting, and public resting zones have been upgraded.",
          body: "დუშეთის ცენტრალური პარკისა და მიმდებარე საზოგადოებრივი სივრცეების განახლების ფარგლებში მოეწყო მოწესრიგებული ბილიკები, განათება და მწვანე ზონები. პროექტის მიზანია ადგილობრივი მოსახლეობისა და სტუმრებისთვის უსაფრთხო და კომფორტული გარემოს შექმნა.",
          bodyEn: "As part of upgrading Dusheti Central Park and nearby public areas, neat walkways, lighting, and green zones were developed to create a safe and welcoming environment for residents and visitors."
        },
        {
          id: 2,
          published: true,
          author: "განათლების მიმართულება",
          date: "2026-11-22",
          tag: "განათლება",
          tagEn: "Education",
          title: "ვაჟა-ფშაველას სახელობის დუშეთის №1 და №2 საჯარო სკოლებში სასწავლო გარემოს გაძლიერება",
          titleEn: "Strengthening Learning Spaces at Dusheti Public Schools #1 and #2",
          photo: "https://images.unsplash.com/photo-1509062522246-3755977927d7?auto=format&fit=crop&w=1000&q=80",
          excerpt: "სკოლებში გაუმჯობესდა ციფრული სასწავლო რესურსები, ბიბლიოთეკები და მოსწავლეთა პროექტების მხარდაჭერა.",
          excerptEn: "Digital learning resources, libraries, and student project support have expanded.",
          body: "ვაჟა-ფშაველას სახელობის დუშეთის №1 საჯარო სკოლასა და დუშეთის №2 საჯარო სკოლაში აქტიურად მიმდინარეობს სასწავლო რესურსების განახლება, მოსწავლეთა ციფრული უნარების განვითარება და საგანმანათლებლო ინიციატივების მხარდაჭერა.",
          bodyEn: "Vazha-Pshavela Dusheti Public School #1 and Dusheti Public School #2 continue expanding learning resources, digital skills, and student academic programs."
        },
        {
          id: 3,
          published: true,
          author: "უსაფრთხოების მონიტორინგი",
          date: "2026-11-19",
          tag: "უსაფრთხოება",
          tagEn: "Safety",
          title: "მთიან მარშრუტებსა და ჟინვალი-ანანურის მიმართულებით მონიტორინგი გაძლიერდა",
          titleEn: "Enhanced Monitoring Along Mountain Routes & Zhinvali–Ananuri Corridor",
          photo: "https://images.unsplash.com/photo-1483664852095-d6cc6870702d?auto=format&fit=crop&w=1000&q=80",
          excerpt: "საგზაო და ამინდის მდგომარეობის მუდმივი მონიტორინგი მიმდინარეობს.",
          excerptEn: "Continuous road and weather monitoring is underway across mountain routes.",
          body: "ჟინვალის წყალსაცავის, ანანურისა და მაღალმთიანი დასახლებების მიმართულებით მიმდინარეობს საგზაო მდგომარეობის, შესაძლო მეწყრული უბნებისა და ამინდის პირობების მუდმივი მონიტორინგი.",
          bodyEn: "Road conditions, potential landslide spots, and weather conditions along Zhinvali Reservoir, Ananuri, and highland routes are monitored continuously."
        }
      ],
      jobs: [
        { id: 1, title: "Smart City სისტემების არქიტექტორი", titleEn: "Smart City Systems Architect", company: "დუშეთის ციფრული განვითარების ცენტრი", salary: "3,600 ₾ / თვეში", schedule: "სრული განაკვეთი", location: "ქ. დუშეთი", firstJob: false },
        { id: 2, title: "Product & UI/UX დიზაინერი", titleEn: "Product & UI/UX Designer", company: "Aragvi Digital Studio", salary: "3,100 ₾ / თვეში", schedule: "დისტანციური / ჰიბრიდული", location: "ქ. დუშეთი", firstJob: false },
        { id: 3, title: "Full-Stack ვებ-დეველოპერი", titleEn: "Full-Stack Web Developer", company: "მცხეთა-მთიანეთის ციფრული ჰაბი", salary: "3,400 ₾ / თვეში", schedule: "დისტანციური", location: "ქ. დუშეთი", firstJob: false },
        { id: 4, title: "ტურიზმისა და ციფრული მარკეტინგის მენეჯერი", titleEn: "Tourism & Digital Marketing Manager", company: "Bazaleti Hospitality Group", salary: "2,600 ₾ / თვეში", schedule: "ჰიბრიდული", location: "ბაზალეთი / დუშეთი", firstJob: false },
        { id: 5, title: "პირველი სამსახური: უმცროსი Frontend დეველოპერი (სტაჟიორი)", titleEn: "First Job: Junior Frontend Developer Intern", company: "დუშეთის ინოვაციების ლაბორატორია", salary: "1,400 ₾ / თვეში", schedule: "სტაჟირება", location: "ქ. დუშეთი", firstJob: true }
      ],
      schools: [
        { id: 1, name: "ვაჟა-ფშაველას სახელობის დუშეთის №1 საჯარო სკოლა", nameEn: "Vazha-Pshavela Dusheti Public School #1", settlement: "ქ. დუშეთი", address: "Plus Code: 3MPV+M5H, დუშეთი", students: "485 მოსწავლე", lat: 42.0867, lng: 44.7054, desc: "ვაჟა-ფშაველას სახელობის დუშეთის №1 საჯარო სკოლა სრული ზოგადი განათლების პროგრამითა და განახლებული სასწავლო გარემოთი." },
        { id: 2, name: "დუშეთის №2 საჯარო სკოლა", nameEn: "Dusheti Public School #2", settlement: "ქ. დუშეთი", address: "42.08682, 44.69014, დუშეთი", students: "561 მოსწავლე", lat: 42.08682, lng: 44.69014, desc: "ქალაქ დუშეთის №2 საჯარო სკოლა თანამედროვე საკლასო ოთახებით, ბიბლიოთეკითა და საგანმანათლებლო პროგრამებით." },
        { id: 3, name: "დაბა ჟინვალის საჯარო სკოლა", nameEn: "Daba Zhinvali Public School", settlement: "დაბა ჟინვალი", address: "დაბა ჟინვალი, დუშეთის მუნიციპალიტეტი", students: "340 მოსწავლე", lat: 42.168406, lng: 44.765189, desc: "ჟინვალის საჯარო სკოლა მოსწავლეთა აკადემიური და შემოქმედებითი განვითარების პროგრამებით." },
        { id: 4, name: "დაბა ფასანაურის საჯარო სკოლა", nameEn: "Daba Pasanauri Public School", settlement: "დაბა ფასანაური", address: "დაბა ფასანაური, დუშეთის მუნიციპალიტეტი", students: "295 მოსწავლე", lat: 42.3536, lng: 44.6889, desc: "ფასანაურის საჯარო სკოლა სრული ზოგადი განათლებისა და კულტურული პროექტებით." },
        { id: 5, name: "სოფელ ჭოპორტის საჯარო სკოლა", nameEn: "Village Choporti Public School", settlement: "სოფ. ჭოპორტი", address: "სოფ. ჭოპორტი, დუშეთის მუნიციპალიტეტი", students: "130 მოსწავლე", lat: 41.9885, lng: 44.7521, desc: "ჭოპორტის საჯარო სკოლა ადგილობრივი მოსწავლეების განათლებისა და სპორტული აქტივობებისთვის." },
        { id: 6, name: "სოფელ მჭადიჯვრის საჯარო სკოლა", nameEn: "Village Mchadijvari Public School", settlement: "სოფ. მჭადიჯვარი", address: "სოფ. მჭადიჯვარი, დუშეთის მუნიციპალიტეტი", students: "145 მოსწავლე", lat: 42.0281, lng: 44.5784, desc: "მჭადიჯვრის საჯარო სკოლა ისტორიულ და საგანმანათლებლო სივრცეში." },
        { id: 7, name: "სოფელ ბაზალეთის საჯარო სკოლა", nameEn: "Village Bazaleti Public School", settlement: "სოფ. ბაზალეთი", address: "სოფ. ბაზალეთი, დუშეთის მუნიციპალიტეტი", students: "155 მოსწავლე", lat: 42.0378, lng: 44.6793, desc: "ბაზალეთის საჯარო სკოლა გარემოსდაცვითი და აკადემიური პროგრამებით." },
        { id: 8, name: "სოფელ ბარისახოს საჯარო სკოლა", nameEn: "Village Barisakho Public School", settlement: "სოფ. ბარისახო", address: "სოფ. ბარისახო, დუშეთის მუნიციპალიტეტი", students: "68 მოსწავლე", lat: 42.4442, lng: 44.8725, desc: "პირაქეთა ხევსურეთის საგანმანათლებლო კერა ბარისახოში." }
      ],
      mapMarkers: [
        { id: 1, published: true, layer: "schools", name: "ვაჟა-ფშაველას სახელობის დუშეთის №1 საჯარო სკოლა", lat: 42.0867, lng: 44.7054, address: "Plus Code: 3MPV+M5H, დუშეთი", info: "ვაჟა-ფშაველას სახელობის დუშეთის №1 საჯარო სკოლა" },
        { id: 2, published: true, layer: "schools", name: "დუშეთის №2 საჯარო სკოლა", lat: 42.08682, lng: 44.69014, address: "42.08682, 44.69014, დუშეთი", info: "დუშეთის №2 საჯარო სკოლა" },
        { id: 3, published: true, layer: "civic", name: "დუშეთის მუნიციპალიტეტის მერია", lat: 42.0846, lng: 44.6968, address: "რუსთაველის ქუჩა №27, დუშეთი", info: "დუშეთის მუნიციპალიტეტის მერია, რუსთაველის ქუჩა №27" },
        { id: 4, published: true, layer: "health", name: "დუშეთის სამედიცინო ცენტრი (Geohospital)", lat: 42.0835, lng: 44.6935, address: "ქ. დუშეთი", info: "დუშეთის სამედიცინო ცენტრი (Geohospital)" },
        { id: 5, published: true, layer: "civic", name: "დუშეთის პოლიციის ადმინისტრაციული შენობა", lat: 42.0852, lng: 44.6948, address: "ქ. დუშეთი", info: "დუშეთის პოლიციის შესაბამისი ადმინისტრაციული შენობა" },
        { id: 6, published: true, layer: "civic", name: "დუშეთის მაგისტრატი სასამართლო", lat: 42.0842, lng: 44.6952, address: "ქ. დუშეთი", info: "დუშეთის მაგისტრატი სასამართლო" },
        { id: 7, published: true, layer: "civic", name: "დუშეთის სოციალური მომსახურების ცენტრი", lat: 42.0839, lng: 44.6960, address: "ქ. დუშეთი", info: "დუშეთის სოციალური მომსახურების ცენტრი" },
        { id: 8, published: true, layer: "civic", name: "სახელმწიფო სერვისების განვითარების სააგენტოს დუშეთის ფილიალი", lat: 42.0849, lng: 44.6959, address: "ქ. დუშეთი", info: "სახელმწიფო სერვისების განვითარების სააგენტოს დუშეთის ფილიალი" },
        { id: 9, published: true, layer: "churches", name: "წმ. ნიკოლოზის სახელობის ეკლესია", lat: 42.08478, lng: 44.69761, address: "42.08478, 44.69761, დუშეთი", info: "წმ. ნიკოლოზის სახელობის ეკლესია" },
        { id: 10, published: true, layer: "tourism", name: "ვაჟა-ფშაველას მონუმენტი", lat: 42.0847, lng: 44.6965, address: "42.0847, 44.6965, დუშეთი", info: "ვაჟა-ფშაველას მონუმენტი" },
        { id: 11, published: true, layer: "churches", name: "წმ. ბარბარეს სახელობის ეკლესია", lat: 42.0809, lng: 44.6974, address: "Plus Code: 3MJW+9XG, დუშეთი", info: "წმ. ბარბარეს სახელობის ეკლესია" },
        { id: 12, published: true, layer: "churches", name: "წმ. გრიგოლის სახელობის ეკლესია", lat: 42.08616, lng: 44.70136, address: "42.08616, 44.70136, დუშეთი", info: "წმ. გრიგოლის სახელობის ეკლესია" },
        { id: 13, published: true, layer: "tourism", name: "ჭილაშვილების ციხე-დარბაზი", lat: 42.08809, lng: 44.67950, address: "42.08809, 44.67950, დუშეთი", info: "ჭილაშვილების ციხე-დარბაზი" },
        { id: 14, published: true, layer: "tourism", name: "მოხელეთა დარბაზი ისტორიული შენობა", lat: 42.0856, lng: 44.6972, address: "65 Nazi Shamanauri St, დუშეთი", info: "მოხელეთა დარბაზი ისტორიული შენობა (65 Nazi Shamanauri St.)" },
        { id: 15, published: true, layer: "tourism", name: "ჟინვალის წყალსაცავი", lat: 42.168406, lng: 44.765189, address: "42.168406, 44.765189", info: "ჟინვალის წყალსაცავი" },
        { id: 16, published: true, layer: "tourism", name: "ანანურის ციხე-კომპლექსი", lat: 42.163768, lng: 44.703293, address: "42.163768, 44.703293", info: "ანანურის ციხე-კომპლექსი" },
        { id: 17, published: true, layer: "tourism", name: "ბაზალეთის ტბა", lat: 42.0378, lng: 44.6793, address: "42.0378, 44.6793", info: "ბაზალეთის ტბა" },
        { id: 18, published: true, layer: "civic", name: "დუშეთის ცენტრალური პარკი", lat: 42.0849, lng: 44.6976, address: "Plus Code: 3MMX+X2V, დუშეთი", info: "დუშეთის ცენტრალური პარკი" },
        { id: 19, published: true, layer: "alerts", name: "მეწყრული მონაკვეთის მონიტორინგი — მთიანი მარშრუტი", lat: 42.1920, lng: 44.7310, address: "ჟინვალი-ფასანაურის მიმართულება", info: "სიახლის/მეწყრის მონიტორინგის წერტილი — მიმდინარე დაკვირვება" }
      ]
    };

    // =========================================================================
    // 4. PERSISTENCE (LOCAL + OPTIONAL GOOGLE APPS SCRIPT WEB APP SYNC)
    // =========================================================================
    function saveLocalState() {
      try {
        localStorage.setItem("dd_portal_state_v1", JSON.stringify({
          payments: state.payments,
          applications: state.applications,
          problems: state.problems,
          petitions: state.petitions,
          news: state.news,
          jobs: state.jobs,
          schools: state.schools,
          mapMarkers: state.mapMarkers,
          heroImage: state.heroImage,
          blurPayments: state.blurPayments,
          syncUrl: state.syncUrl
        }));
      } catch (err) {}
    }

    async function persistState() {
      saveLocalState();
      if (state.syncUrl && state.syncUrl.startsWith("https://script.google.com/")) {
        try {
          await fetch(state.syncUrl, {
            method: "POST",
            mode: "no-cors",
            headers: { "Content-Type": "text/plain;charset=utf-8" },
            body: JSON.stringify({
              payments: state.payments,
              applications: state.applications,
              problems: state.problems,
              petitions: state.petitions,
              news: state.news,
              jobs: state.jobs,
              schools: state.schools,
              mapMarkers: state.mapMarkers,
              heroImage: state.heroImage
            })
          });
        } catch (err) {}
      }
    }

    async function loadSavedState() {
      try {
        const raw = localStorage.getItem("dd_portal_state_v1");
        if (raw) {
          const parsed = JSON.parse(raw);
          if (Array.isArray(parsed.payments)) state.payments = parsed.payments;
          if (Array.isArray(parsed.applications)) state.applications = parsed.applications;
          if (Array.isArray(parsed.problems)) state.problems = parsed.problems;
          if (Array.isArray(parsed.petitions)) state.petitions = parsed.petitions;
          if (Array.isArray(parsed.news)) state.news = parsed.news;
          if (Array.isArray(parsed.jobs)) state.jobs = parsed.jobs;
          if (Array.isArray(parsed.schools)) state.schools = parsed.schools;
          if (Array.isArray(parsed.mapMarkers)) state.mapMarkers = parsed.mapMarkers;
          if (typeof parsed.heroImage === "string" && parsed.heroImage) state.heroImage = parsed.heroImage;
          if (typeof parsed.blurPayments === "boolean") state.blurPayments = parsed.blurPayments;
          if (typeof parsed.syncUrl === "string") state.syncUrl = parsed.syncUrl;
        }
      } catch (err) {}

      if (state.syncUrl && state.syncUrl.startsWith("https://script.google.com/")) {
        try {
          const res = await fetch(state.syncUrl);
          if (res.ok) {
            const remote = await res.json();
            if (remote && typeof remote === "object") {
              if (Array.isArray(remote.payments)) state.payments = remote.payments;
              if (Array.isArray(remote.applications)) state.applications = remote.applications;
              if (Array.isArray(remote.problems)) state.problems = remote.problems;
              if (Array.isArray(remote.petitions)) state.petitions = remote.petitions;
              if (Array.isArray(remote.news)) state.news = remote.news;
              if (Array.isArray(remote.jobs)) state.jobs = remote.jobs;
              if (Array.isArray(remote.schools)) state.schools = remote.schools;
              if (Array.isArray(remote.mapMarkers)) state.mapMarkers = remote.mapMarkers;
              if (typeof remote.heroImage === "string" && remote.heroImage) state.heroImage = remote.heroImage;
              saveLocalState();
              if (state.user) renderPage();
            }
          }
        } catch (err) {}
      }
    }

    // =========================================================================
    // 5. AUTHENTICATION, LANGUAGE & NAVIGATION
    // =========================================================================
    function handleLogin(event) {
      if (event) event.preventDefault();
      const fnInput = document.getElementById("login-first-name");
      const lnInput = document.getElementById("login-last-name");
      const errEl = document.getElementById("login-error");

      const firstName = fnInput ? fnInput.value.trim() : "";
      const lastName = lnInput ? lnInput.value.trim() : "";

      if (!firstName || !lastName) {
        if (errEl) {
          errEl.textContent = state.lang === "ka" ? "გთხოვთ შეიყვანოთ სახელი და გვარი" : "Please enter first and last name";
          errEl.classList.remove("hidden");
        }
        return false;
      }

      if (errEl) errEl.classList.add("hidden");
      state.user = { firstName, lastName };
      try {
        localStorage.setItem("dd_active_user", JSON.stringify(state.user));
      } catch (err) {}

      showPortal();
      notify(state.lang === "ka" ? `მოგესალმებით, ${firstName} ${lastName}!` : `Welcome, ${firstName} ${lastName}!`);
      return false;
    }

    function showPortal() {
      const authView = document.getElementById("auth-view");
      const appShell = document.getElementById("app-shell");
      if (authView) authView.classList.add("hidden");
      if (appShell) {
        appShell.classList.remove("hidden");
        appShell.classList.add("flex");
      }
      const fullName = `${state.user.firstName} ${state.user.lastName}`;
      const navName = document.getElementById("nav-user-name");
      const mobName = document.getElementById("mobile-user-name");
      if (navName) navName.textContent = fullName;
      if (mobName) mobName.textContent = fullName;
      goPage("home");
    }

    function logoutUser() {
      state.user = null;
      state.adminUnlocked = false;
      try {
        localStorage.removeItem("dd_active_user");
      } catch (err) {}
      const appShell = document.getElementById("app-shell");
      const authView = document.getElementById("auth-view");
      if (appShell) {
        appShell.classList.add("hidden");
        appShell.classList.remove("flex");
      }
      if (authView) authView.classList.remove("hidden");
    }

    function toggleLang() {
      state.lang = state.lang === "ka" ? "en" : "ka";
      const badge = state.lang.toUpperCase();
      const b1 = document.getElementById("auth-lang-badge");
      const b2 = document.getElementById("nav-lang-badge");
      if (b1) b1.textContent = badge;
      if (b2) b2.textContent = badge;
      applyTranslations();
      if (state.user) renderPage();
    }

    function applyTranslations() {
      const dict = translations[state.lang];
      document.querySelectorAll("[data-i18n]").forEach(el => {
        const key = el.getAttribute("data-i18n");
        if (dict[key]) el.textContent = dict[key];
      });
      document.querySelectorAll("[data-i18n-ph]").forEach(el => {
        const key = el.getAttribute("data-i18n-ph");
        if (dict[key]) el.placeholder = dict[key];
      });
    }

    function goPage(pageName) {
      state.page = pageName;
      window.scrollTo({ top: 0, behavior: "smooth" });
      document.querySelectorAll(".nav-link").forEach(btn => {
        if (btn.getAttribute("data-nav") === pageName) {
          btn.classList.add("bg-[#0F254A]", "text-white");
          btn.classList.remove("text-[#0F254A]", "hover:bg-slate-100");
        } else {
          btn.classList.remove("bg-[#0F254A]", "text-white");
          btn.classList.add("text-[#0F254A]", "hover:bg-slate-100");
        }
      });
      renderPage();
    }

    function toggleMobileMenu() {
      const menu = document.getElementById("mobile-menu");
      if (menu) menu.classList.toggle("hidden");
    }

    function notify(msg, isError = false) {
      const box = document.getElementById("toast-container");
      if (!box) return;
      const item = document.createElement("div");
      item.className = `${isError ? "bg-red-600" : "bg-[#0F254A]"} text-white px-4 py-3 rounded-2xl shadow-xl text-xs font-bold flex items-center gap-2.5 animate-fade-in`;
      item.innerHTML = `<span class="w-2.5 h-2.5 rounded-full ${isError ? "bg-white" : "bg-[#F5C542]"}"></span><span>${escapeText(msg)}</span>`;
      box.appendChild(item);
      setTimeout(() => item.remove(), 3400);
    }

    function openModal(html) {
      const body = document.getElementById("modal-content");
      const backdrop = document.getElementById("modal-backdrop");
      if (body && backdrop) {
        body.innerHTML = html;
        backdrop.classList.remove("hidden");
      }
    }

    function closeModal() {
      const backdrop = document.getElementById("modal-backdrop");
      if (backdrop) backdrop.classList.add("hidden");
    }

    function escapeText(val) {
      return String(val || "")
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;");
    }

    // =========================================================================
    // 6. PAGES
    // =========================================================================

    function renderHome() {
      const d = translations[state.lang];
      const en = state.lang === "en";
      const q = state.searchQuery.trim().toLowerCase();

      const modules = [
        { page: "services", title: en ? "Municipal & Utility Payments" : "მუნიციპალური და კომუნალური გადახდები", desc: en ? "Water tax, electricity, gas, cleaning, receipts & applications." : "წყლის გადასახადი, დენი, გაზი, დასუფთავება, ქვითრები და განცხადებები." },
        { page: "map", title: en ? "Smart Map (18 Places)" : "ინტერაქტიული რუკა", desc: en ? "18 Dusheti places, Google Maps links, alerts & pin tools." : "18 ლოკაცია, Google Maps ბმულები, საფრთხეების/სიახლეების წერტილები." },
        { page: "help", title: en ? "Help Dusheti" : "დაეხმარე დუშეთს", desc: en ? "Report issues with photos for ADM review and public release." : "ატვირთეთ ფოტო და აღწერეთ პრობლემა ADM მოდერაციისთვის." },
        { page: "petitions", title: en ? "Statements & Initiatives" : "განცხადებები", desc: en ? "Submit public statements and support citizen proposals." : "გამოაქვეყნეთ განცხადება და მხარი დაუჭირეთ ინიციატივებს." },
        { page: "health", title: en ? "Health & Safety" : "ჯანმრთელობა და უსაფრთხოება", desc: en ? "Geohospital medical center, 111 child helpline & 112 badge." : "Geohospital სამედიცინო ცენტრი, 111 ბავშვთა ხაზი და 112." },
        { page: "jobs", title: en ? "Jobs & First Job" : "დასაქმება", desc: en ? "Modern monthly salary roles + First Job youth internships." : "მაღალანაზღაურებადი თვიური ვაკანსიები და „პირველი სამსახური“." },
        { page: "edu", title: en ? "Schools Directory" : "განათლება და სკოლები", desc: en ? "Dusheti municipality schools with details and map view." : "დუშეთის მუნიციპალიტეტის საჯარო სკოლები და რუკაზე ჩვენება." },
        { page: "contact", title: en ? "Contact" : "კონტაქტი", desc: en ? "Rustaveli St #27, Dusheti • thevoiceofgeneratio05@gmail.com" : "რუსთაველის ქ. №27, დუშეთი • thevoiceofgeneratio05@gmail.com" }
      ];

      const filteredModules = q
        ? modules.filter(m => m.title.toLowerCase().includes(q) || m.desc.toLowerCase().includes(q))
        : modules;

      const publishedNews = state.news.filter(n => n.published).slice(0, 3);

      return `
        <section class="bg-white border-b border-slate-100">
          <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 sm:py-16">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-10 items-center">
              <div class="lg:col-span-7 animate-fade-in">
                <h1 class="text-3xl sm:text-5xl font-extrabold tracking-tight leading-[1.18] text-[#0F254A] mb-5">
                  ${d.heroTitle}
                </h1>
                <p class="text-base sm:text-lg text-slate-600 leading-relaxed mb-7 max-w-2xl">
                  ${d.heroSub}
                </p>

                <div class="mb-7 max-w-xl">
                  <input type="text" value="${escapeText(state.searchQuery)}" oninput="state.searchQuery=this.value; renderPage()" placeholder="${d.searchPh}" class="w-full rounded-2xl border border-slate-200 bg-slate-50/80 focus:bg-white px-4 py-3 text-sm text-[#0F254A] focus:outline-none focus:border-[#0F254A] transition" />
                </div>

                <div class="flex flex-wrap items-center gap-3.5">
                  <button type="button" onclick="goPage('services')" class="px-6 py-3.5 rounded-2xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-sm transition shadow-sm">
                    ${d.ctaServices}
                  </button>
                  <button type="button" onclick="goPage('map')" class="px-6 py-3.5 rounded-2xl bg-[#F5C542] hover:bg-[#D9A927] text-[#0F254A] font-extrabold text-sm transition">
                    ${d.ctaMap}
                  </button>
                </div>
              </div>

              <div class="lg:col-span-5">
                <div class="rounded-3xl overflow-hidden border border-slate-200 shadow-lg bg-white">
                  <img src="${escapeText(state.heroImage)}" alt="Dusheti Panorama" class="w-full h-72 sm:h-80 object-cover" />
                  <div class="p-4 bg-white flex items-center justify-between text-xs text-slate-600">
                    <span class="font-bold text-[#0F254A]">${en ? "Dusheti Panoramic View" : "დუშეთის ხედი"}</span>
                    <a href="${getGoogleMapsLink(42.0846, 44.6968, 'Dusheti Municipality')}" target="_blank" rel="noopener noreferrer" class="font-extrabold text-[#D9A927] hover:underline">Google Maps →</a>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14">
          <div class="mb-8">
            <h2 class="text-2xl sm:text-3xl font-extrabold text-[#0F254A]">${d.quickServices}</h2>
            <p class="text-slate-500 text-sm sm:text-base mt-1">${d.quickServicesSub}</p>
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
            ${filteredModules.map((m, i) => `
              <button type="button" onclick="goPage('${m.page}')" class="portal-card text-left p-6 flex flex-col justify-between group">
                <div>
                  <div class="w-11 h-11 rounded-2xl bg-[#FEF7E0] text-[#0F254A] group-hover:bg-[#0F254A] group-hover:text-[#F5C542] font-extrabold text-sm flex items-center justify-center mb-4 transition">
                    0${i + 1}
                  </div>
                  <h3 class="text-lg font-extrabold text-[#0F254A] mb-2">${m.title}</h3>
                  <p class="text-sm text-slate-600 leading-relaxed">${m.desc}</p>
                </div>
                <div class="mt-5 pt-3 border-t border-slate-100 flex items-center justify-between text-xs font-extrabold text-[#0F254A]">
                  <span>${d.viewFull}</span>
                  <span class="text-[#D9A927]">→</span>
                </div>
              </button>
            `).join("")}
          </div>
        </section>

        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pb-10">
          <div class="flex items-center justify-between mb-6">
            <h2 class="text-2xl font-extrabold text-[#0F254A]">${en ? "Latest News" : "უახლესი სიახლეები"}</h2>
            <button type="button" onclick="goPage('news')" class="text-xs font-extrabold text-[#0F254A] hover:underline">${en ? "All News →" : "ყველა სიახლე →"}</button>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            ${publishedNews.map(item => `
              <article class="portal-card overflow-hidden flex flex-col justify-between">
                <div>
                  <img src="${escapeText(item.photo)}" alt="${escapeText(item.title)}" class="w-full h-48 object-cover bg-slate-100" />
                  <div class="p-6">
                    <div class="flex items-center justify-between text-xs text-slate-500 mb-2">
                      <span class="px-2.5 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] font-bold">${en ? item.tagEn : item.tag}</span>
                      <span>${item.date}</span>
                    </div>
                    <h3 class="text-base font-extrabold text-[#0F254A] mb-2">${en ? item.titleEn : item.title}</h3>
                    <p class="text-xs text-slate-600 leading-relaxed">${en ? item.excerptEn : item.excerpt}</p>
                  </div>
                </div>
                <div class="px-6 pb-6">
                  <button type="button" onclick="showNewsModal(${item.id})" class="w-full py-2.5 rounded-xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-xs transition">${d.readFull}</button>
                </div>
              </article>
            `).join("")}
          </div>
        </section>
      `;
    }

    function renderServices() {
      const en = state.lang === "en";
      const billPanels = [
        { ka: "წყლის გადასახადი", en: "Water Tax", box: "bg-sky-50 border-sky-200", icon: `<svg class="w-6 h-6 text-sky-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3C12 3 6 10.2 6 14.5a6 6 0 0012 0C18 10.2 12 3 12 3z"/></svg>` },
        { ka: "ელექტროენერგია", en: "Electricity", box: "bg-amber-50 border-amber-200", icon: `<svg class="w-6 h-6 text-amber-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>` },
        { ka: "ბუნებრივი აირი", en: "Gas", box: "bg-orange-50 border-orange-200", icon: `<svg class="w-6 h-6 text-orange-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M12 2c1.5 3.5 5 6 5 10a5 5 0 01-10 0c0-2.2 1.2-4.2 2.5-5.7.3 1.5 1.2 2.2 2.5 2.2V2z"/></svg>` },
        { ka: "დასუფთავება", en: "Cleaning", box: "bg-emerald-50 border-emerald-200", icon: `<svg class="w-6 h-6 text-emerald-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M4 7h16M9 7V4h6v3m-8 0l1 13h8l1-13"/></svg>` },
        { ka: "ინტერნეტი", en: "Internet", box: "bg-indigo-50 border-indigo-200", icon: `<svg class="w-6 h-6 text-indigo-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M5 12.55a11 11 0 0114.08 0M8.53 16.11a6 6 0 016.95 0M12 20h.01"/></svg>` },
        { ka: "მობილური", en: "Mobile", box: "bg-purple-50 border-purple-200", icon: `<svg class="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><rect x="7" y="2" width="10" height="20" rx="2"/><path d="M12 18h.01"/></svg>` },
        { ka: "ტრანსპორტი", en: "Transport", box: "bg-teal-50 border-teal-200", icon: `<svg class="w-6 h-6 text-teal-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><rect x="4" y="4" width="16" height="13" rx="2"/><path d="M4 11h16M8 17v2m8-2v2"/></svg>` },
        { ka: "განათლება", en: "Education", box: "bg-rose-50 border-rose-200", icon: `<svg class="w-6 h-6 text-rose-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3L2 9l10 6 10-6-10-6zm-6 9v5c0 2 3 3 6 3s6-1 6-3v-5"/></svg>` }
      ];

      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A] mb-2">${en ? "Municipal & Utility Payments" : "მუნიციპალური და კომუნალური გადახდები"}</h1>
          <p class="text-sm text-slate-600 mb-8">${en ? "Select Water Tax or any utility panel to check debt, pay online, and get a receipt." : "აირჩიეთ წყლის გადასახადი ან სხვა პანელი დავალიანების შესამოწმებლად და გადასახდელად."}</p>

          <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 mb-10">
            ${billPanels.map(p => `
              <button type="button" onclick="selectBillCategory('${p.ka}')" class="${p.box} border rounded-3xl p-5 flex flex-col items-center text-center hover:shadow-md transition">
                <div class="w-12 h-12 rounded-2xl bg-white shadow-sm flex items-center justify-center mb-3">${p.icon}</div>
                <div class="text-sm font-extrabold text-[#0F254A]">${en ? p.en : p.ka}</div>
              </button>
            `).join("")}
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            <div class="lg:col-span-7 portal-card p-6 sm:p-8">
              <h2 class="text-xl font-extrabold text-[#0F254A] mb-4">${en ? "Debt Check & Payment" : "დავალიანების შემოწმება და გადახდა"}</h2>
              <form onsubmit="submitBillPayment(event)" class="space-y-4">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">${en ? "Category" : "გადასახადის სახეობა"}</label>
                    <select id="bill-cat" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm font-bold text-[#0F254A]">
                      ${billPanels.map(p => `<option value="${p.ka}" ${state.selectedService === p.ka ? "selected" : ""}>${en ? p.en : p.ka}</option>`).join("")}
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">${en ? "Subscriber Number" : "აბონენტის ნომერი"}</label>
                    <input type="text" id="bill-acc" required placeholder="მაგ: 409128" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
                  </div>
                </div>
                <div class="flex flex-wrap items-end gap-3">
                  <div class="flex-1 min-w-[160px]">
                    <label class="block text-xs font-bold text-slate-700 mb-1">${en ? "Amount (GEL)" : "თანხა (₾)"}</label>
                    <input type="text" id="bill-amt" required value="16.00 ₾" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm font-extrabold text-[#0F254A]" />
                  </div>
                  <button type="button" onclick="checkBillDebt()" class="px-4 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-[#0F254A] font-bold text-xs transition">${en ? "Check Debt" : "დავალიანების შემოწმება"}</button>
                </div>
                <button type="submit" class="px-6 py-3 rounded-xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-sm transition">${en ? "Pay & Get Receipt" : "გადახდა და ქვითრის მიღება"}</button>
              </form>
            </div>

            <div class="lg:col-span-5 portal-card p-6 sm:p-8">
              <h2 class="text-xl font-extrabold text-[#0F254A] mb-4">${en ? "Submit Municipal Application" : "მუნიციპალური განაცხადი"}</h2>
              <form onsubmit="submitServiceApp(event)" class="space-y-3.5">
                <input type="text" id="srv-app-subj" required placeholder="${en ? "Subject" : "განაცხადის თემა"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
                <input type="text" id="srv-app-loc" required placeholder="${en ? "Location / Address" : "მისამართი / ლოკაცია"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
                <textarea id="srv-app-desc" rows="3" required placeholder="${en ? "Description..." : "აღწერა..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
                <button type="submit" class="w-full py-3 rounded-xl bg-[#F5C542] hover:bg-[#D9A927] text-[#0F254A] font-extrabold text-sm transition">${en ? "Send Application" : "განაცხადის გაგზავნა"}</button>
              </form>
            </div>
          </div>
        </div>
      `;
    }

    function selectBillCategory(nameKa) {
      state.selectedService = nameKa;
      const el = document.getElementById("bill-cat");
      if (el) el.value = nameKa;
      notify(nameKa);
    }

    function checkBillDebt() {
      const acc = (document.getElementById("bill-acc").value || "").trim();
      if (!acc) {
        notify(state.lang === "ka" ? "შეიყვანეთ აბონენტის ნომერი" : "Enter subscriber number", true);
        return;
      }
      const amtInput = document.getElementById("bill-amt");
      if (amtInput) amtInput.value = "18.40 ₾";
      notify(state.lang === "ka" ? "დავალიანება შემოწმდა: 18.40 ₾" : "Debt checked: 18.40 ₾");
    }

    function submitBillPayment(e) {
      e.preventDefault();
      const service = document.getElementById("bill-cat").value;
      const account = document.getElementById("bill-acc").value.trim();
      const amount = document.getElementById("bill-amt").value.trim();
      const recId = "REC-" + Math.floor(1000 + Math.random() * 9000);
      const payer = `${state.user.firstName} ${state.user.lastName}`;
      const date = "2026-11-25";

      state.payments.unshift({ id: recId, payer, service, account, amount, date });
      persistState();

      openModal(`
        <div class="text-center space-y-3">
          <div class="w-14 h-14 rounded-2xl bg-emerald-100 text-emerald-700 flex items-center justify-center mx-auto font-extrabold text-xl">✓</div>
          <h3 class="text-xl font-extrabold text-[#0F254A]">ელექტრონული ქვითარი</h3>
          <div class="bg-slate-50 rounded-2xl p-4 border border-slate-200 text-left text-xs space-y-2">
            <div class="flex justify-between"><span>ქვითრის №:</span><strong>${recId}</strong></div>
            <div class="flex justify-between"><span>გადამხდელი:</span><strong>${escapeText(payer)}</strong></div>
            <div class="flex justify-between"><span>სერვისი:</span><strong>${escapeText(service)}</strong></div>
            <div class="flex justify-between"><span>აბონენტი:</span><strong>${escapeText(account)}</strong></div>
            <div class="flex justify-between"><span>თანხა:</span><strong class="text-emerald-700">${escapeText(amount)}</strong></div>
            <div class="flex justify-between"><span>თარიღი:</span><strong>${date}</strong></div>
          </div>
        </div>
      `);
    }

    function submitServiceApp(e) {
      e.preventDefault();
      state.applications.unshift({
        id: "APP-" + Math.floor(110 + Math.random() * 880),
        applicant: `${state.user.firstName} ${state.user.lastName}`,
        subject: document.getElementById("srv-app-subj").value.trim(),
        location: document.getElementById("srv-app-loc").value.trim(),
        status: "მიღებულია",
        date: "2026-11-25"
      });
      persistState();
      notify(state.lang === "ka" ? "განაცხადი გაიგზავნა!" : "Application sent!");
      goPage("petitions");
    }

    function renderMap() {
      const en = state.lang === "en";
      const layerDefs = [
        { key: "schools", ka: "სკოლები", en: "Schools", color: "#2563EB" },
        { key: "civic", ka: "მერია და უწყებები", en: "Civic & Gov", color: "#0F254A" },
        { key: "health", ka: "სამედიცინო", en: "Medical", color: "#059669" },
        { key: "churches", ka: "ეკლესიები", en: "Churches", color: "#7C3AED" },
        { key: "tourism", ka: "ძეგლები და ტბები", en: "Landmarks & Lakes", color: "#D9A927" },
        { key: "alerts", ka: "სიახლეები / მეწყერი", en: "News / Landslide", color: "#DC2626" }
      ];

      const visibleMarkers = state.mapMarkers.filter(m => m.published && state.layers[m.layer]);

      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 animate-fade-in">
          <div class="flex flex-wrap items-center justify-between gap-4 mb-5">
            <div>
              <h1 class="text-2xl sm:text-3xl font-extrabold text-[#0F254A]">${en ? "Dusheti Smart Map" : "დუშეთის ინტერაქტიული რუკა"}</h1>
              <p class="text-xs sm:text-sm text-slate-500 mt-1">${en ? "18 verified Dusheti locations, news/landslide points, Google Maps links & click-to-add pins." : "18 შემოწმებული ლოკაცია, სიახლეების/მეწყრის წერტილები, Google Maps ბმულები და რუკაზე დაჭერით წერტილის დასმა."}</p>
            </div>
            <div class="flex flex-wrap gap-2">
              <button type="button" onclick="toggleClickPinMode()" class="px-4 py-2.5 rounded-xl ${state.clickPlaceMode ? "bg-[#F5C542] text-[#0F254A]" : "bg-slate-100 text-[#0F254A]"} border border-slate-200 font-extrabold text-xs transition">
                ${state.clickPlaceMode ? (en ? "Click Map: ON" : "რუკაზე დაჭერა: ჩართულია") : (en ? "Click Map to Place" : "რუკაზე დაჭერით დასმა")}
              </button>
              <button type="button" onclick="openAddMapPointModal()" class="px-4 py-2.5 rounded-xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-xs transition">
                ${en ? "+ Add Point" : "+ წერტილის დამატება"}
              </button>
            </div>
          </div>

          <div class="bg-slate-50 rounded-2xl p-3 border border-slate-200 mb-5 flex flex-wrap gap-2">
            ${layerDefs.map(l => `
              <button type="button" onclick="toggleMapLayer('${l.key}')" class="px-3.5 py-1.5 rounded-full text-xs font-bold flex items-center gap-2 border transition ${state.layers[l.key] ? "bg-[#0F254A] text-white border-[#0F254A]" : "bg-white text-slate-600 border-slate-200"}">
                <span class="w-2.5 h-2.5 rounded-full" style="background:${l.color}"></span>
                <span>${en ? l.en : l.ka}</span>
              </button>
            `).join("")}
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <div class="lg:col-span-8 bg-white rounded-3xl border border-slate-200 overflow-hidden shadow-sm">
              <div id="dusheti-leaflet-map" class="w-full h-[460px] sm:h-[540px]"></div>
            </div>
            <div class="lg:col-span-4 space-y-4">
              <div id="map-detail-box" class="bg-[#0F254A] text-white rounded-3xl p-6">
                <h3 class="text-base font-extrabold">${en ? "Select a Point" : "აირჩიეთ წერტილი რუკაზე"}</h3>
                <p class="text-xs text-slate-300 mt-1">${en ? "Tap any marker or place in the list to view details and open Google Maps." : "დააჭირეთ წერტილს დეტალების სანახავად და Google Maps-ში გასახსნელად."}</p>
              </div>
              <div class="portal-card p-4 max-h-[360px] overflow-y-auto space-y-2">
                ${visibleMarkers.map((m, idx) => `
                  <div class="p-3 rounded-2xl bg-slate-50 hover:bg-[#FEF7E0] border border-slate-200/70 transition flex items-start justify-between gap-2">
                    <button type="button" onclick="focusMapPoint(${m.id})" class="text-left flex-1">
                      <div class="text-xs font-extrabold text-[#0F254A]">${idx + 1}. ${escapeText(m.name)}</div>
                      <div class="text-[11px] text-slate-500 mt-0.5">${escapeText(m.address)}</div>
                    </button>
                    <a href="${getGoogleMapsLink(m.lat, m.lng, m.name)}" target="_blank" rel="noopener noreferrer" class="text-[11px] font-extrabold text-[#0F254A] underline shrink-0">Maps</a>
                  </div>
                `).join("")}
              </div>
            </div>
          </div>
        </div>
      `;
    }

    function initLeafletMap() {
      const container = document.getElementById("dusheti-leaflet-map");
      if (!container || typeof L === "undefined") return;
      if (state.map) {
        state.map.remove();
        state.map = null;
      }
      state.map = L.map("dusheti-leaflet-map").setView([42.0852, 44.6968], 13);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: "&copy; OpenStreetMap | DIGITAL DUSHETI"
      }).addTo(state.map);

      const colors = {
        schools: "#2563EB",
        civic: "#0F254A",
        health: "#059669",
        churches: "#7C3AED",
        tourism: "#D9A927",
        alerts: "#DC2626"
      };

      state.mapMarkers.forEach(m => {
        if (!m.published || !state.layers[m.layer]) return;
        const c = colors[m.layer] || "#0F254A";
        const markerIcon = L.divIcon({
          className: "",
          html: `<div style="background:${c};width:22px;height:22px;border-radius:9999px;border:3px solid #FFFFFF;box-shadow:0 3px 8px rgba(0,0,0,0.3);"></div>`,
          iconSize: [22, 22],
          iconAnchor: [11, 11]
        });
        const marker = L.marker([m.lat, m.lng], { icon: markerIcon }).addTo(state.map);
        marker.bindPopup(`
          <strong>${escapeText(m.name)}</strong><br/>
          <span style="font-size:11px">${escapeText(m.address)}</span><br/>
          <a href="${getGoogleMapsLink(m.lat, m.lng, m.name)}" target="_blank" rel="noopener noreferrer" style="font-size:11px;font-weight:700;color:#0F254A;">Google Maps →</a>
        `);
        marker.on("click", () => {
          const box = document.getElementById("map-detail-box");
          if (box) {
            box.innerHTML = `
              <div class="text-[11px] font-bold text-[#F5C542] mb-1">${escapeText(m.address)} (${Number(m.lat).toFixed(5)}, ${Number(m.lng).toFixed(5)})</div>
              <h3 class="text-base font-extrabold">${escapeText(m.name)}</h3>
              <p class="text-xs text-slate-200 mt-2 leading-relaxed">${escapeText(m.info)}</p>
              <a href="${getGoogleMapsLink(m.lat, m.lng, m.name)}" target="_blank" rel="noopener noreferrer" class="inline-block mt-3 px-3.5 py-1.5 rounded-xl bg-[#F5C542] text-[#0F254A] text-xs font-extrabold">Google Maps</a>
            `;
          }
        });
      });

      state.map.on("click", function (ev) {
        if (!state.clickPlaceMode) return;
        openAddMapPointModal(ev.latlng.lat.toFixed(5), ev.latlng.lng.toFixed(5));
      });
    }

    function toggleClickPinMode() {
      state.clickPlaceMode = !state.clickPlaceMode;
      notify(state.clickPlaceMode ? "დააჭირეთ რუკაზე სასურველ ადგილს" : "რუკაზე დაჭერის რეჟიმი გაითიშა");
      renderPage();
    }

    function toggleMapLayer(key) {
      state.layers[key] = !state.layers[key];
      renderPage();
    }

    function focusMapPoint(id) {
      const m = state.mapMarkers.find(x => x.id === id);
      if (!m || !state.map) return;
      state.map.setView([m.lat, m.lng], 15);
      L.popup().setLatLng([m.lat, m.lng]).setContent(`<strong>${escapeText(m.name)}</strong><br/>${escapeText(m.address)}`).openOn(state.map);
    }

    function openAddMapPointModal(latVal = "42.0852", lngVal = "44.6968") {
      const en = state.lang === "en";
      openModal(`
        <h3 class="text-xl font-extrabold text-[#0F254A] mb-3">${en ? "Add Map Location" : "წერტილის დამატება რუკაზე"}</h3>
        <form onsubmit="addMapPointFromModal(event)" class="space-y-3">
          <input type="text" id="mp-name" required placeholder="${en ? "Place / News / Landslide Name" : "დასახელება (ადგილი / სიახლე / მეწყერი)"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <select id="mp-layer" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm">
            <option value="civic">მერია და უწყებები</option>
            <option value="schools">სკოლა</option>
            <option value="health">სამედიცინო</option>
            <option value="churches">ეკლესია</option>
            <option value="tourism">ძეგლი / ტბა</option>
            <option value="alerts">სიახლეები / მეწყერი</option>
          </select>
          <input type="text" id="mp-addr" required placeholder="${en ? "Address / Plus Code" : "მისამართი / Plus Code"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <div class="grid grid-cols-2 gap-3">
            <input type="number" step="any" id="mp-lat" value="${latVal}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
            <input type="number" step="any" id="mp-lng" value="${lngVal}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          </div>
          <textarea id="mp-info" rows="2" required placeholder="${en ? "Description..." : "აღწერა..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Save Point" : "რუკაზე დამატება"}</button>
        </form>
      `);
    }

    function addMapPointFromModal(e) {
      e.preventDefault();
      state.mapMarkers.unshift({
        id: Date.now(),
        published: true,
        layer: document.getElementById("mp-layer").value,
        name: document.getElementById("mp-name").value.trim(),
        address: document.getElementById("mp-addr").value.trim(),
        lat: parseFloat(document.getElementById("mp-lat").value),
        lng: parseFloat(document.getElementById("mp-lng").value),
        info: document.getElementById("mp-info").value.trim()
      });
      persistState();
      closeModal();
      notify(state.lang === "ka" ? "წერტილი დაემატა რუკაზე!" : "Point added!");
      renderPage();
    }

    function renderHelp() {
      const en = state.lang === "en";
      const items = state.problems.filter(p => p.published);
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-8">
            <div>
              <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A]">${en ? "Help Dusheti" : "დაეხმარე დუშეთს"}</h1>
              <p class="text-sm text-slate-600 mt-1">${en ? "Submit photos and problem details. Items go to ADM and appear publicly after approval." : "ატვირთეთ ფოტო და აღწერეთ პრობლემა — ჩანაწერი გადადის ADM პანელში და საჯაროდ ქვეყნდება დადასტურების შემდეგ."}</p>
            </div>
            <button type="button" onclick="openHelpModal()" class="px-5 py-3 rounded-2xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-sm shrink-0 self-start">
              ${en ? "+ Upload Issue & Photo" : "+ პრობლემის და ფოტოს ატვირთვა"}
            </button>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            ${items.map(p => `
              <div class="portal-card overflow-hidden flex flex-col justify-between">
                <div>
                  <img src="${escapeText(p.photo)}" alt="${escapeText(p.title)}" class="w-full h-52 object-cover bg-slate-100" />
                  <div class="p-6">
                    <span class="px-2.5 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] text-xs font-bold">${en ? p.categoryEn : p.category}</span>
                    <h3 class="text-lg font-extrabold text-[#0F254A] mt-3 mb-1.5">${en ? p.titleEn : p.title}</h3>
                    <div class="text-xs font-semibold text-slate-500 mb-2">${escapeText(p.location)}</div>
                    <p class="text-sm text-slate-600 leading-relaxed">${en ? p.descEn : p.desc}</p>
                  </div>
                </div>
                <div class="px-6 py-3.5 bg-slate-50 border-t border-slate-100 flex justify-between text-xs text-slate-500">
                  <span>${escapeText(p.author)}</span>
                  <span>${p.date}</span>
                </div>
              </div>
            `).join("")}
          </div>
        </div>
      `;
    }

    function openHelpModal() {
      const en = state.lang === "en";
      openModal(`
        <h3 class="text-xl font-extrabold text-[#0F254A] mb-3">${en ? "Report Issue with Photo" : "პრობლემის და ფოტოს ატვირთვა"}</h3>
        <form onsubmit="submitHelpItem(event)" class="space-y-3">
          <input type="text" id="hp-title" required placeholder="${en ? "Issue Title" : "პრობლემის სათაური"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="hp-cat" required placeholder="${en ? "Category" : "კატეგორია"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="hp-loc" required placeholder="${en ? "Location in Dusheti" : "მდებარეობა დუშეთში"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="file" id="hp-file" accept="image/*" class="w-full text-xs text-slate-600 border border-slate-200 rounded-xl p-2" />
          <textarea id="hp-desc" rows="3" required placeholder="${en ? "Describe the problem..." : "აღწერეთ პრობლემა..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Send to ADM" : "გაგზავნა ADM პანელში"}</button>
        </form>
      `);
    }

    function submitHelpItem(e) {
      e.preventDefault();
      const title = document.getElementById("hp-title").value.trim();
      const category = document.getElementById("hp-cat").value.trim();
      const location = document.getElementById("hp-loc").value.trim();
      const desc = document.getElementById("hp-desc").value.trim();
      const fileEl = document.getElementById("hp-file");

      const saveItem = (photoSrc) => {
        state.problems.unshift({
          id: Date.now(),
          author: `${state.user.firstName} ${state.user.lastName}`,
          category,
          categoryEn: category,
          title,
          titleEn: title,
          location,
          desc,
          descEn: desc,
          photo: photoSrc || "https://images.unsplash.com/photo-1515162816999-a0c47dc192f7?auto=format&fit=crop&w=900&q=80",
          date: "2026-11-25",
          published: false
        });
        persistState();
        closeModal();
        notify(state.lang === "ka" ? "პრობლემა გაიგზავნა ADM-ში დასამტკიცებლად" : "Sent to ADM for approval");
      };

      if (fileEl && fileEl.files && fileEl.files[0]) {
        const reader = new FileReader();
        reader.onload = ev => saveItem(ev.target.result);
        reader.readAsDataURL(fileEl.files[0]);
      } else {
        saveItem(null);
      }
    }

    function renderPetitions() {
      const en = state.lang === "en";
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-8">
            <div>
              <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A]">${en ? "Statements & Applications" : "განცხადებები"}</h1>
              <p class="text-sm text-slate-600 mt-1">${en ? "Publish civic statements, support initiatives, and track municipal applications." : "გამოაქვეყნეთ განცხადებები, მხარი დაუჭირეთ ინიციატივებს და შეამოწმეთ სტატუსი."}</p>
            </div>
            <button type="button" onclick="openStatementModal()" class="px-5 py-3 rounded-2xl bg-[#F5C542] hover:bg-[#D9A927] text-[#0F254A] font-extrabold text-sm shrink-0 self-start">
              ${en ? "+ New Statement" : "+ ახალი განცხადება"}
            </button>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-10">
            ${state.petitions.map(pt => `
              <div class="portal-card p-6 flex flex-col justify-between">
                <div>
                  <div class="flex justify-between text-xs text-slate-500 mb-2">
                    <span>${escapeText(pt.author)}</span>
                    <span>${pt.date}</span>
                  </div>
                  <h3 class="text-lg font-extrabold text-[#0F254A] mb-2">${en ? pt.titleEn : pt.title}</h3>
                  <p class="text-sm text-slate-600 leading-relaxed">${en ? pt.descEn : pt.desc}</p>
                </div>
                <div class="mt-6 pt-4 border-t border-slate-100 flex items-center justify-between">
                  <span class="text-xs font-extrabold text-[#0F254A]">${pt.votes} ${en ? "Supporters" : "მხარდამჭერი"}</span>
                  <button type="button" onclick="voteStatement(${pt.id})" class="px-4 py-2 rounded-xl ${pt.supported ? "bg-emerald-600 text-white" : "bg-[#0F254A] text-white"} text-xs font-bold transition">
                    ${pt.supported ? (en ? "Supported ✓" : "მხარდაჭერილია ✓") : (en ? "Support" : "მხარდაჭერა")}
                  </button>
                </div>
              </div>
            `).join("")}
          </div>

          <div class="portal-card p-6">
            <h2 class="text-base font-extrabold text-[#0F254A] mb-4">${en ? "Municipal Applications Status" : "მუნიციპალური განაცხადების სტატუსი"}</h2>
            <div class="space-y-2.5">
              ${state.applications.map(a => `
                <div class="p-3.5 rounded-2xl bg-slate-50 border border-slate-200/70 flex flex-wrap items-center justify-between gap-2 text-xs">
                  <div>
                    <span class="font-extrabold text-[#0F254A]">${a.id}</span> — <span class="font-bold">${escapeText(a.subject)}</span> (${escapeText(a.location)})
                  </div>
                  <span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] font-bold">${escapeText(a.status)}</span>
                </div>
              `).join("")}
            </div>
          </div>
        </div>
      `;
    }

    function voteStatement(id) {
      const pt = state.petitions.find(x => x.id === id);
      if (!pt) return;
      pt.supported = !pt.supported;
      pt.votes += pt.supported ? 1 : -1;
      persistState();
      renderPage();
    }

    function openStatementModal() {
      const en = state.lang === "en";
      openModal(`
        <h3 class="text-xl font-extrabold text-[#0F254A] mb-3">${en ? "Publish Statement" : "განცხადების გამოქვეყნება"}</h3>
        <form onsubmit="addStatementItem(event)" class="space-y-3">
          <input type="text" id="st-title" required placeholder="${en ? "Title" : "სათაური"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <textarea id="st-desc" rows="3" required placeholder="${en ? "Details..." : "განცხადების ტექსტი..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Publish" : "გამოქვეყნება"}</button>
        </form>
      `);
    }

    function addStatementItem(e) {
      e.preventDefault();
      const title = document.getElementById("st-title").value.trim();
      const desc = document.getElementById("st-desc").value.trim();
      state.petitions.unshift({
        id: Date.now(),
        author: `${state.user.firstName} ${state.user.lastName}`,
        title, titleEn: title, desc, descEn: desc,
        votes: 1, supported: true, date: "2026-11-25"
      });
      persistState();
      closeModal();
      notify(state.lang === "ka" ? "განცხადება გამოქვეყნდა!" : "Statement published!");
      renderPage();
    }

    function renderNews() {
      const d = translations[state.lang];
      const en = state.lang === "en";
      const items = state.news.filter(n => n.published);
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-8">
            <div>
              <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A]">${en ? "Municipal News" : "სიახლეები"}</h1>
              <p class="text-sm text-slate-600 mt-1">${en ? "Submitted news goes to ADM first so you decide whether it stays hidden or public." : "გამოგზავნილი სიახლე ჯერ მიდის ADM პანელში, სადაც თქვენ წყვეტთ საჯარო იქნება თუ დამალული."}</p>
            </div>
            <button type="button" onclick="openSendNewsModal()" class="px-5 py-3 rounded-2xl bg-[#0F254A] text-white font-bold text-xs sm:text-sm shrink-0 self-start">
              ${en ? "+ Send News to ADM" : "+ სიახლის გაგზავნა ADM-ში"}
            </button>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            ${items.map(n => `
              <article class="portal-card overflow-hidden flex flex-col justify-between">
                <div>
                  <img src="${escapeText(n.photo)}" alt="${escapeText(n.title)}" class="w-full h-52 object-cover" />
                  <div class="p-6">
                    <div class="flex justify-between text-xs text-slate-500 mb-2">
                      <span class="px-2.5 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] font-bold">${en ? n.tagEn : n.tag}</span>
                      <span>${n.date}</span>
                    </div>
                    <h3 class="text-lg font-extrabold text-[#0F254A] mb-2">${en ? n.titleEn : n.title}</h3>
                    <p class="text-sm text-slate-600 leading-relaxed">${en ? n.excerptEn : n.excerpt}</p>
                  </div>
                </div>
                <div class="px-6 pb-6">
                  <button type="button" onclick="showNewsModal(${n.id})" class="w-full py-2.5 rounded-xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-xs transition">${d.readFull}</button>
                </div>
              </article>
            `).join("")}
          </div>
        </div>
      `;
    }

    function showNewsModal(id) {
      const n = state.news.find(x => x.id === id);
      if (!n) return;
      const en = state.lang === "en";
      openModal(`
        <img src="${escapeText(n.photo)}" alt="${escapeText(n.title)}" class="w-full h-64 object-cover rounded-2xl mb-5 border border-slate-200" />
        <div class="flex items-center gap-3 text-xs text-slate-500 mb-2">
          <span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] font-bold">${en ? n.tagEn : n.tag}</span>
          <span>${n.date}</span>
        </div>
        <h2 class="text-2xl font-extrabold text-[#0F254A] mb-4">${en ? n.titleEn : n.title}</h2>
        <p class="text-sm sm:text-base text-slate-700 leading-relaxed">${en ? n.bodyEn : n.body}</p>
      `);
    }

    function openSendNewsModal() {
      const en = state.lang === "en";
      openModal(`
        <h3 class="text-xl font-extrabold text-[#0F254A] mb-2">${en ? "Send News to ADM" : "სიახლის გაგზავნა ADM პანელში"}</h3>
        <p class="text-xs text-slate-500 mb-4">${en ? "Saved as hidden in ADM until you publish it publicly." : "სიახლე ინახება ADM პანელში დამალულად, სანამ საჯაროდ გამოაქვეყნებთ."}</p>
        <form onsubmit="submitNewsForAdm(event)" class="space-y-3">
          <input type="text" id="nw-title" required placeholder="${en ? "Headline" : "სათაური"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="nw-tag" required placeholder="${en ? "Category" : "კატეგორია"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="nw-photo" placeholder="${en ? "Photo URL (optional)" : "ფოტოს ბმული (არასავალდებულო)"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <textarea id="nw-body" rows="4" required placeholder="${en ? "Full story..." : "სრული ტექსტი..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Send to ADM" : "გაგზავნა"}</button>
        </form>
      `);
    }

    function submitNewsForAdm(e) {
      e.preventDefault();
      const title = document.getElementById("nw-title").value.trim();
      const tag = document.getElementById("nw-tag").value.trim();
      const photoUrl = document.getElementById("nw-photo").value.trim() || "https://images.unsplash.com/photo-1517048676732-d65bc937f952?auto=format&fit=crop&w=1000&q=80";
      const body = document.getElementById("nw-body").value.trim();
      state.news.unshift({
        id: Date.now(),
        published: false,
        author: `${state.user.firstName} ${state.user.lastName}`,
        date: "2026-11-25",
        tag, tagEn: tag, title, titleEn: title,
        photo: photoUrl,
        excerpt: body.slice(0, 120) + "...",
        excerptEn: body.slice(0, 120) + "...",
        body, bodyEn: body
      });
      persistState();
      closeModal();
      notify(state.lang === "ka" ? "სიახლე გაიგზავნა ADM პანელში (დამალული სტატუსით)" : "News sent to ADM (hidden)");
    }

    function renderHealth() {
      const en = state.lang === "en";
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A] mb-6">${en ? "Health & Safety" : "ჯანმრთელობა და უსაფრთხოება"}</h1>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-10">
            <div class="rounded-3xl p-6 bg-[#FEF7E0] border border-[#F5C542] flex items-center gap-4">
              <div class="w-14 h-14 rounded-2xl bg-[#F5C542] text-[#0F254A] flex items-center justify-center shrink-0">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M12 21s-6-4.35-9-9a5.25 5.25 0 019-5.25A5.25 5.25 0 0121 12c-3 4.65-9 9-9 9z" />
                </svg>
              </div>
              <div>
                <div class="text-2xl font-extrabold text-[#0F254A]">111</div>
                <div class="text-sm font-bold text-[#0F254A]">${en ? "Child Protection Hotline" : "ბავშვთა დაცვის ცხელი ხაზი"}</div>
                <div class="text-xs text-slate-700 mt-0.5">${en ? "Child protection & support line" : "ბავშვთა დაცვისა და მხარდაჭერის ცხელი ხაზი"}</div>
              </div>
            </div>

            <div aria-disabled="true" class="rounded-3xl p-6 bg-red-600 text-white flex items-center justify-between gap-4 select-none cursor-default">
              <div>
                <div class="text-2xl font-extrabold">112 — ${en ? "Emergency Assistance" : "გადაუდებელი დახმარება"}</div>
                <div class="text-xs text-red-100 mt-1">${en ? "Non-interactive indicator badge" : "არაინტერაქტიული საინფორმაციო ღილაკი"}</div>
              </div>
              <span class="px-3 py-1 rounded-xl bg-white/15 text-xs font-bold">112</span>
            </div>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="portal-card p-6">
              <h3 class="text-lg font-extrabold text-[#0F254A] mb-2">დუშეთის სამედიცინო ცენტრი (Geohospital)</h3>
              <p class="text-xs text-slate-500 mb-3">ქ. დუშეთი • კლინიკა და გადაუდებელი სერვისები</p>
              <p class="text-sm text-slate-600 mb-4">სამედიცინო მომსახურება, ამბულატორია და დიაგნოსტიკა დუშეთის მოსახლეობისთვის.</p>
              <a href="${getGoogleMapsLink(42.0835, 44.6935, 'Dusheti Medical Center Geohospital')}" target="_blank" rel="noopener noreferrer" class="text-xs font-extrabold text-[#0F254A] underline">Google Maps →</a>
            </div>
            <div class="portal-card p-6">
              <h3 class="text-lg font-extrabold text-[#0F254A] mb-2">დუშეთის სოციალური მომსახურების ცენტრი</h3>
              <p class="text-xs text-slate-500 mb-3">ქ. დუშეთი • აფთიაქები და სოციალური სერვისები</p>
              <p class="text-sm text-slate-600 mb-4">მოქალაქეთა მხარდაჭერა, სოციალური პროგრამები და ახლომდებარე აფთიაქების ლოკაციები.</p>
              <a href="${getGoogleMapsLink(42.0839, 44.6960, 'Dusheti Social Service Center')}" target="_blank" rel="noopener noreferrer" class="text-xs font-extrabold text-[#0F254A] underline">Google Maps →</a>
            </div>
          </div>
        </div>
      `;
    }

    function renderJobs() {
      const en = state.lang === "en";
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A] mb-2">${en ? "Jobs & First Job" : "დასაქმება და ვაკანსიები"}</h1>
          <p class="text-sm text-slate-600 mb-8">${en ? "Modern high monthly salary positions and First Job youth internships." : "თანამედროვე პოზიციები მაღალი თვიური ანაზღაურებით და „პირველი სამსახური“."}</p>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            ${state.jobs.map(j => `
              <div class="portal-card p-6 flex flex-col justify-between">
                <div>
                  <div class="flex items-center justify-between gap-2 mb-3">
                    <span class="px-3 py-1 rounded-full bg-slate-100 text-[#0F254A] text-xs font-bold">${escapeText(j.schedule)}</span>
                    ${j.firstJob ? `<span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] text-xs font-extrabold">პირველი სამსახური</span>` : ""}
                  </div>
                  <h3 class="text-lg font-extrabold text-[#0F254A]">${en && j.titleEn ? j.titleEn : j.title}</h3>
                  <div class="text-sm text-slate-600 mt-1">${escapeText(j.company)}</div>
                  <div class="mt-4 flex items-center justify-between text-xs">
                    <span class="text-slate-500">${escapeText(j.location)}</span>
                    <span class="font-extrabold text-emerald-700 text-sm">${escapeText(j.salary)}</span>
                  </div>
                </div>
                <button type="button" onclick="openJobModal(${j.id})" class="mt-5 w-full py-2.5 rounded-xl bg-[#0F254A] hover:bg-[#1A3668] text-white font-bold text-xs transition">${en ? "Upload CV & Apply" : "რეზიუმეს ატვირთვა და გაგზავნა"}</button>
              </div>
            `).join("")}
          </div>
        </div>
      `;
    }

    function openJobModal(id) {
      const job = state.jobs.find(x => x.id === id);
      if (!job) return;
      const en = state.lang === "en";
      openModal(`
        <h3 class="text-xl font-extrabold text-[#0F254A] mb-1">${escapeText(job.title)}</h3>
        <p class="text-xs text-slate-500 mb-4">${escapeText(job.company)} • ${escapeText(job.salary)}</p>
        <form onsubmit="submitJobApplication(event)" class="space-y-3">
          <input type="text" value="${escapeText(state.user.firstName + " " + state.user.lastName)}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm font-bold" />
          <input type="file" required accept=".pdf,.doc,.docx" class="w-full text-xs border border-slate-200 rounded-xl p-2.5" />
          <textarea rows="3" placeholder="${en ? "Cover note..." : "სამოტივაციო წერილი..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Submit CV" : "გაგზავნა"}</button>
        </form>
      `);
    }

    function submitJobApplication(e) {
      e.preventDefault();
      closeModal();
      notify(state.lang === "ka" ? "განაცხადი და რეზიუმე გაიგზავნა!" : "Application & CV sent!");
    }

    function renderEdu() {
      const d = translations[state.lang];
      const en = state.lang === "en";
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A] mb-2">${en ? "Dusheti Municipality Schools" : "დუშეთის მუნიციპალიტეტის სკოლები"}</h1>
          <p class="text-sm text-slate-600 mb-8">${en ? "Public schools across Dusheti town and municipality." : "საჯარო სკოლები ქალაქ დუშეთსა და დუშეთის მუნიციპალიტეტში."}</p>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            ${state.schools.map(s => `
              <div class="portal-card p-6 flex flex-col justify-between">
                <div>
                  <div class="flex justify-between items-center text-xs mb-3">
                    <span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] font-bold">${escapeText(s.settlement)}</span>
                    <span class="text-slate-500 font-semibold">${escapeText(s.students)}</span>
                  </div>
                  <h3 class="text-lg font-extrabold text-[#0F254A] mb-1.5">${en ? s.nameEn : s.name}</h3>
                  <div class="text-xs text-slate-500 mb-3">${escapeText(s.address)}</div>
                  <p class="text-sm text-slate-600 leading-relaxed">${escapeText(s.desc)}</p>
                </div>
                <div class="mt-5 pt-4 border-t border-slate-100 flex gap-2">
                  <button type="button" onclick="showSchoolModal(${s.id})" class="flex-1 py-2.5 rounded-xl bg-[#0F254A] text-white font-bold text-xs">${d.viewFull}</button>
                  <button type="button" onclick="locateOnMap(${s.lat}, ${s.lng}, '${escapeText(s.name)}')" class="px-3.5 py-2.5 rounded-xl bg-slate-100 hover:bg-[#FEF7E0] text-[#0F254A] font-bold text-xs">${en ? "Map" : "რუკა"}</button>
                  <a href="${getGoogleMapsLink(s.lat, s.lng, s.name)}" target="_blank" rel="noopener noreferrer" class="px-3.5 py-2.5 rounded-xl bg-slate-100 hover:bg-[#FEF7E0] text-[#0F254A] font-bold text-xs flex items-center">G-Maps</a>
                </div>
              </div>
            `).join("")}
          </div>
        </div>
      `;
    }

    function showSchoolModal(id) {
      const s = state.schools.find(x => x.id === id);
      if (!s) return;
      const en = state.lang === "en";
      openModal(`
        <span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] text-xs font-bold">${escapeText(s.settlement)}</span>
        <h2 class="text-xl font-extrabold text-[#0F254A] mt-3 mb-2">${en ? s.nameEn : s.name}</h2>
        <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200 text-xs space-y-1.5 my-4">
          <div><strong>მისამართი:</strong> ${escapeText(s.address)}</div>
          <div><strong>მოსწავლეები:</strong> ${escapeText(s.students)}</div>
          <div><strong>კოორდინატები:</strong> ${s.lat}, ${s.lng}</div>
        </div>
        <p class="text-sm text-slate-700 leading-relaxed mb-5">${escapeText(s.desc)}</p>
        <div class="flex gap-3">
          <button type="button" onclick="closeModal(); locateOnMap(${s.lat}, ${s.lng}, '${escapeText(s.name)}')" class="flex-1 py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">რუკაზე ჩვენება</button>
          <a href="${getGoogleMapsLink(s.lat, s.lng, s.name)}" target="_blank" rel="noopener noreferrer" class="px-5 py-3 rounded-xl bg-[#F5C542] text-[#0F254A] font-extrabold text-sm flex items-center">Google Maps</a>
        </div>
      `);
    }

    function locateOnMap(lat, lng, title) {
      goPage("map");
      setTimeout(() => {
        if (state.map) {
          state.map.setView([lat, lng], 15);
          L.popup().setLatLng([lat, lng]).setContent(`<strong>${title}</strong>`).openOn(state.map);
        }
      }, 200);
    }

    function renderContact() {
      const en = state.lang === "en";
      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <h1 class="text-2xl sm:text-4xl font-extrabold text-[#0F254A] mb-6">${en ? "Contact" : "კონტაქტი"}</h1>
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <div class="portal-card p-7 space-y-4 text-sm">
              <div>
                <div class="text-xs text-slate-500 font-bold">${en ? "Address" : "მისამართი"}</div>
                <div class="font-extrabold text-[#0F254A] mt-0.5">ქ. დუშეთი, რუსთაველის ქუჩა №27</div>
              </div>
              <div>
                <div class="text-xs text-slate-500 font-bold">${en ? "Email" : "ელ-ფოსტა"}</div>
                <a href="mailto:thevoiceofgeneratio05@gmail.com" class="font-extrabold text-[#0F254A] hover:text-[#D9A927]">thevoiceofgeneratio05@gmail.com</a>
              </div>
              <div>
                <a href="${getGoogleMapsLink(42.0846, 44.6968, 'Rustaveli St 27, Dusheti')}" target="_blank" rel="noopener noreferrer" class="inline-block px-4 py-2.5 rounded-xl bg-[#0F254A] text-white text-xs font-bold">Google Maps</a>
              </div>
            </div>

            <div class="portal-card p-7">
              <h2 class="text-lg font-extrabold text-[#0F254A] mb-4">${en ? "Send Message" : "შეტყობინების გაგზავნა"}</h2>
              <form onsubmit="submitContactMsg(event)" class="space-y-3">
                <input type="text" required placeholder="${en ? "Subject" : "თემა"}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
                <textarea rows="3" required placeholder="${en ? "Your message..." : "თქვენი შეტყობინება..."}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm"></textarea>
                <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">${en ? "Send" : "გაგზავნა"}</button>
              </form>
            </div>
          </div>
        </div>
      `;
    }

    function submitContactMsg(e) {
      e.preventDefault();
      notify(state.lang === "ka" ? "შეტყობინება გაიგზავნა!" : "Message sent!");
      e.target.reset();
    }

    // =========================================================================
    // 7. ADM PANEL (PASSWORD: 2026.26.26)
    // =========================================================================
    function openAdminModal() {
      if (state.adminUnlocked) {
        goPage("admin");
        return;
      }
      openModal(`
        <div class="max-w-sm mx-auto text-center">
          <div class="w-12 h-12 rounded-2xl bg-[#0F254A] text-[#F5C542] flex items-center justify-center mx-auto mb-3 font-extrabold text-sm">ADM</div>
          <h3 class="text-xl font-extrabold text-[#0F254A]">ADM</h3>
          <p class="text-xs text-slate-500 mt-1 mb-4">პაროლი</p>
          <form onsubmit="unlockAdmin(event)" class="space-y-3">
            <input type="password" id="adm-pass-input" required placeholder="••••••••••" class="w-full rounded-xl border border-slate-300 px-4 py-3 text-center font-mono text-sm" />
            <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">შესვლა</button>
          </form>
        </div>
      `);
    }

    function unlockAdmin(e) {
      e.preventDefault();
      const val = document.getElementById("adm-pass-input").value.trim();
      if (val === "2026.26.26") {
        state.adminUnlocked = true;
        closeModal();
        goPage("admin");
        notify("ADM გააქტიურდა");
      } else {
        notify("არასწორი პაროლი", true);
      }
    }

    function renderAdmin() {
      if (!state.adminUnlocked) return renderHome();
      const blurClass = state.blurPayments ? "blur-sensitive" : "blur-sensitive unblurred";

      return `
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 animate-fade-in">
          <div class="flex flex-wrap items-center justify-between gap-4 pb-6 mb-8 border-b border-slate-200">
            <div>
              <span class="px-3 py-1 rounded-full bg-[#FEF7E0] text-[#0F254A] text-xs font-extrabold">ADM</span>
              <h1 class="text-2xl sm:text-3xl font-extrabold text-[#0F254A] mt-2">ადმინისტრატორის პანელი</h1>
            </div>
            <button type="button" onclick="state.adminUnlocked=false; goPage('home')" class="px-4 py-2 rounded-xl border border-slate-300 text-xs font-bold">გასვლა</button>
          </div>

          <!-- Banner Photo & Google Apps Script Sync URL -->
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
            <div class="portal-card p-6">
              <h2 class="text-base font-extrabold text-[#0F254A] mb-3">მთავარი გვერდის ფოტოს შეცვლა</h2>
              <form onsubmit="admUpdateHeroPhoto(event)" class="flex flex-col sm:flex-row gap-2">
                <input type="text" id="adm-hero-url" value="${escapeText(state.heroImage)}" required class="flex-1 rounded-xl border border-slate-200 px-3.5 py-2 text-xs" />
                <button type="submit" class="px-4 py-2 rounded-xl bg-[#0F254A] text-white font-bold text-xs">შენახვა</button>
              </form>
            </div>

            <div class="portal-card p-6">
              <h2 class="text-base font-extrabold text-[#0F254A] mb-3">Google Apps Script Web App URL (საერთო სინქრონიზაცია)</h2>
              <form onsubmit="admSaveSyncUrl(event)" class="flex flex-col sm:flex-row gap-2">
                <input type="url" id="adm-sync-url" value="${escapeText(state.syncUrl)}" placeholder="https://script.google.com/macros/s/.../exec" class="flex-1 rounded-xl border border-slate-200 px-3.5 py-2 text-xs" />
                <button type="submit" class="px-4 py-2 rounded-xl bg-[#F5C542] text-[#0F254A] font-extrabold text-xs">დაკავშირება</button>
              </form>
            </div>
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <!-- Problems & Photos -->
            <div class="portal-card p-6">
              <h2 class="text-lg font-extrabold text-[#0F254A] mb-4">დაეხმარე დუშეთს — პრობლემები და ფოტოები (${state.problems.length})</h2>
              <div class="space-y-3 max-h-96 overflow-y-auto">
                ${state.problems.map(p => `
                  <div class="p-4 rounded-2xl bg-slate-50 border border-slate-200 text-xs space-y-2">
                    <div class="flex gap-3">
                      <img src="${escapeText(p.photo)}" alt="" class="w-16 h-16 rounded-xl object-cover shrink-0" />
                      <div>
                        <div class="font-extrabold text-[#0F254A]">${escapeText(p.title)}</div>
                        <div class="text-slate-500">${escapeText(p.author)} • ${escapeText(p.location)}</div>
                        <span class="inline-block mt-1 px-2 py-0.5 rounded bg-white border text-[11px] font-bold">${p.published ? "საჯაროა" : "დამალულია"}</span>
                      </div>
                    </div>
                    <div class="flex flex-wrap gap-2">
                      <button type="button" onclick="admTogglePublished('problems', ${p.id})" class="px-3 py-1 rounded-lg bg-[#0F254A] text-white font-bold">${p.published ? "დამალვა" : "გამოქვეყნება"}</button>
                      <button type="button" onclick="admEditRecord('problems', ${p.id})" class="px-3 py-1 rounded-lg bg-[#F5C542] text-[#0F254A] font-bold">რედაქტირება</button>
                      <button type="button" onclick="admDeleteRecord('problems', ${p.id})" class="px-3 py-1 rounded-lg bg-red-600 text-white font-bold">წაშლა</button>
                    </div>
                  </div>
                `).join("")}
              </div>
            </div>

            <!-- Map Markers -->
            <div class="portal-card p-6">
              <h2 class="text-lg font-extrabold text-[#0F254A] mb-4">რუკის წერტილები (${state.mapMarkers.length})</h2>
              <form onsubmit="admAddMarker(event)" class="grid grid-cols-2 gap-2 pb-4 mb-4 border-b border-slate-200">
                <input type="text" id="adm-mk-name" required placeholder="დასახელება" class="col-span-2 rounded-xl border border-slate-200 px-3 py-2 text-xs" />
                <select id="adm-mk-layer" class="rounded-xl border border-slate-200 px-3 py-2 text-xs">
                  <option value="civic">მერია და უწყებები</option>
                  <option value="schools">სკოლა</option>
                  <option value="health">სამედიცინო</option>
                  <option value="churches">ეკლესია</option>
                  <option value="tourism">ძეგლი / ტბა</option>
                  <option value="alerts">სიახლეები / მეწყერი</option>
                </select>
                <input type="text" id="adm-mk-addr" required placeholder="მისამართი" class="rounded-xl border border-slate-200 px-3 py-2 text-xs" />
                <input type="number" step="any" id="adm-mk-lat" value="42.0852" required class="rounded-xl border border-slate-200 px-3 py-2 text-xs" />
                <input type="number" step="any" id="adm-mk-lng" value="44.6968" required class="rounded-xl border border-slate-200 px-3 py-2 text-xs" />
                <input type="text" id="adm-mk-info" required placeholder="აღწერა" class="col-span-2 rounded-xl border border-slate-200 px-3 py-2 text-xs" />
                <button type="submit" class="col-span-2 py-2.5 rounded-xl bg-[#F5C542] text-[#0F254A] font-extrabold text-xs">დამატება</button>
              </form>
              <div class="space-y-2 max-h-60 overflow-y-auto">
                ${state.mapMarkers.map(m => `
                  <div class="p-3 rounded-xl bg-slate-50 border border-slate-200 text-xs flex items-center justify-between gap-2">
                    <div>
                      <div class="font-bold text-[#0F254A]">${escapeText(m.name)}</div>
                      <div class="text-slate-500">${escapeText(m.address)} (${m.published ? "საჯაროა" : "დამალულია"})</div>
                    </div>
                    <div class="flex gap-1 shrink-0">
                      <button type="button" onclick="admTogglePublished('mapMarkers', ${m.id})" class="px-2 py-1 rounded bg-[#0F254A] text-white font-bold">${m.published ? "დამალვა" : "გამოქვ."}</button>
                      <button type="button" onclick="admEditMarker(${m.id})" class="px-2 py-1 rounded bg-[#F5C542] text-[#0F254A] font-bold">შეცვლა</button>
                      <button type="button" onclick="admDeleteRecord('mapMarkers', ${m.id})" class="px-2 py-1 rounded bg-red-600 text-white font-bold">წაშლა</button>
                    </div>
                  </div>
                `).join("")}
              </div>
            </div>

            <!-- News Moderation -->
            <div class="portal-card p-6">
              <h2 class="text-lg font-extrabold text-[#0F254A] mb-4">სიახლეები — საჯარო / დამალული (${state.news.length})</h2>
              <div class="space-y-2.5 max-h-80 overflow-y-auto">
                ${state.news.map(n => `
                  <div class="p-3 rounded-xl bg-slate-50 border border-slate-200 text-xs flex items-center justify-between gap-2">
                    <div>
                      <div class="font-bold text-[#0F254A]">${escapeText(n.title)}</div>
                      <div class="text-slate-500">${escapeText(n.author)} • ${n.published ? "საჯაროა" : "დამალულია"}</div>
                    </div>
                    <div class="flex gap-1 shrink-0">
                      <button type="button" onclick="admTogglePublished('news', ${n.id})" class="px-2.5 py-1 rounded bg-[#0F254A] text-white font-bold">${n.published ? "დამალვა" : "გამოქვეყნება"}</button>
                      <button type="button" onclick="admEditRecord('news', ${n.id})" class="px-2.5 py-1 rounded bg-[#F5C542] text-[#0F254A] font-bold">შეცვლა</button>
                      <button type="button" onclick="admDeleteRecord('news', ${n.id})" class="px-2.5 py-1 rounded bg-red-600 text-white font-bold">წაშლა</button>
                    </div>
                  </div>
                `).join("")}
              </div>
            </div>

            <!-- Payments (Blurred) & Deletions -->
            <div class="portal-card p-6 space-y-6">
              <div>
                <div class="flex items-center justify-between mb-3">
                  <h2 class="text-lg font-extrabold text-[#0F254A]">გადახდები</h2>
                  <button type="button" onclick="state.blurPayments=!state.blurPayments; persistState(); renderPage()" class="px-2.5 py-1 rounded-lg bg-slate-100 text-[11px] font-bold">${state.blurPayments ? "Blur ჩართულია" : "Blur გამორთულია"}</button>
                </div>
                <div class="space-y-2 max-h-44 overflow-y-auto">
                  ${state.payments.map(pay => `
                    <div class="p-3 rounded-xl bg-slate-50 border border-slate-200 text-xs flex items-center justify-between">
                      <div>
                        <span class="font-bold ${blurClass}">${escapeText(pay.payer)}</span> — ${escapeText(pay.service)}
                      </div>
                      <div class="flex items-center gap-2">
                        <span class="font-extrabold text-emerald-700 ${blurClass}">${escapeText(pay.amount)}</span>
                        <button type="button" onclick="admDeleteRecord('payments', '${pay.id}')" class="px-2 py-0.5 rounded bg-red-600 text-white font-bold">წაშლა</button>
                      </div>
                    </div>
                  `).join("")}
                </div>
              </div>

              <div>
                <h2 class="text-base font-extrabold text-[#0F254A] mb-2">განცხადებები და ვაკანსიები</h2>
                <div class="space-y-2 max-h-44 overflow-y-auto">
                  ${state.petitions.map(pt => `
                    <div class="p-2.5 rounded-xl bg-slate-50 border border-slate-200 text-xs flex justify-between items-center">
                      <span class="truncate pr-2">${escapeText(pt.title)}</span>
                      <button type="button" onclick="admDeleteRecord('petitions', ${pt.id})" class="px-2 py-0.5 rounded bg-red-600 text-white font-bold shrink-0">წაშლა</button>
                    </div>
                  `).join("")}
                  ${state.jobs.map(jb => `
                    <div class="p-2.5 rounded-xl bg-slate-50 border border-slate-200 text-xs flex justify-between items-center">
                      <span class="truncate pr-2">${escapeText(jb.title)} (${escapeText(jb.salary)})</span>
                      <button type="button" onclick="admDeleteRecord('jobs', ${jb.id})" class="px-2 py-0.5 rounded bg-red-600 text-white font-bold shrink-0">წაშლა</button>
                    </div>
                  `).join("")}
                </div>
              </div>
            </div>
          </div>
        </div>
      `;
    }

    function admUpdateHeroPhoto(e) {
      e.preventDefault();
      state.heroImage = document.getElementById("adm-hero-url").value.trim();
      persistState();
      notify("მთავარი ფოტო განახლდა");
      renderPage();
    }

    function admSaveSyncUrl(e) {
      e.preventDefault();
      state.syncUrl = document.getElementById("adm-sync-url").value.trim();
      persistState();
      notify("სინქრონიზაციის ბმული შენახულია");
    }

    function admTogglePublished(key, id) {
      const rec = state[key].find(x => x.id === id);
      if (rec) {
        rec.published = !rec.published;
        persistState();
      }
      renderPage();
    }

    function admDeleteRecord(key, id) {
      state[key] = state[key].filter(x => x.id !== id);
      persistState();
      notify("ჩანაწერი წაიშალა");
      renderPage();
    }

    function admEditRecord(key, id) {
      const rec = state[key].find(x => x.id === id);
      if (!rec) return;
      openModal(`
        <h3 class="text-lg font-extrabold text-[#0F254A] mb-3">რედაქტირება</h3>
        <form onsubmit="saveAdmRecord(event, '${key}', ${id})" class="space-y-3">
          <input type="text" id="ed-r-title" value="${escapeText(rec.title)}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="ed-r-author" value="${escapeText(rec.author || "")}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="ed-r-photo" value="${escapeText(rec.photo || "")}" class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <textarea id="ed-r-body" rows="4" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm">${escapeText(rec.body || rec.desc || "")}</textarea>
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">შენახვა</button>
        </form>
      `);
    }

    function saveAdmRecord(e, key, id) {
      e.preventDefault();
      const rec = state[key].find(x => x.id === id);
      if (!rec) return;
      rec.title = document.getElementById("ed-r-title").value.trim();
      rec.titleEn = rec.title;
      const authVal = document.getElementById("ed-r-author").value.trim();
      if (authVal) rec.author = authVal;
      const phVal = document.getElementById("ed-r-photo").value.trim();
      if (phVal) rec.photo = phVal;
      const txtVal = document.getElementById("ed-r-body").value.trim();
      if ("body" in rec) {
        rec.body = txtVal;
        rec.bodyEn = txtVal;
      } else {
        rec.desc = txtVal;
        rec.descEn = txtVal;
      }
      persistState();
      closeModal();
      notify("ცვლილება შეინახა");
      renderPage();
    }

    function admEditMarker(id) {
      const m = state.mapMarkers.find(x => x.id === id);
      if (!m) return;
      openModal(`
        <h3 class="text-lg font-extrabold text-[#0F254A] mb-3">წერტილის რედაქტირება</h3>
        <form onsubmit="saveAdmMarker(event, ${id})" class="space-y-3">
          <input type="text" id="ed-mk-name" value="${escapeText(m.name)}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <input type="text" id="ed-mk-addr" value="${escapeText(m.address)}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <div class="grid grid-cols-2 gap-3">
            <input type="number" step="any" id="ed-mk-lat" value="${m.lat}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
            <input type="number" step="any" id="ed-mk-lng" value="${m.lng}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          </div>
          <input type="text" id="ed-mk-info" value="${escapeText(m.info)}" required class="w-full rounded-xl border border-slate-200 px-3.5 py-2.5 text-sm" />
          <button type="submit" class="w-full py-3 rounded-xl bg-[#0F254A] text-white font-bold text-sm">შენახვა</button>
        </form>
      `);
    }

    function saveAdmMarker(e, id) {
      e.preventDefault();
      const m = state.mapMarkers.find(x => x.id === id);
      if (!m) return;
      m.name = document.getElementById("ed-mk-name").value.trim();
      m.address = document.getElementById("ed-mk-addr").value.trim();
      m.lat = parseFloat(document.getElementById("ed-mk-lat").value);
      m.lng = parseFloat(document.getElementById("ed-mk-lng").value);
      m.info = document.getElementById("ed-mk-info").value.trim();
      persistState();
      closeModal();
      notify("წერტილი განახლდა");
      renderPage();
    }

    function admAddMarker(e) {
      e.preventDefault();
      state.mapMarkers.unshift({
        id: Date.now(),
        published: true,
        layer: document.getElementById("adm-mk-layer").value,
        name: document.getElementById("adm-mk-name").value.trim(),
        address: document.getElementById("adm-mk-addr").value.trim(),
        lat: parseFloat(document.getElementById("adm-mk-lat").value),
        lng: parseFloat(document.getElementById("adm-mk-lng").value),
        info: document.getElementById("adm-mk-info").value.trim()
      });
      persistState();
      notify("წერტილი დაემატა");
      renderPage();
    }

    // =========================================================================
    // 8. ROUTER & BOOTSTRAP
    // =========================================================================
    function renderPage() {
      const container = document.getElementById("page-container");
      if (!container) return;

      switch (state.page) {
        case "home": container.innerHTML = renderHome(); break;
        case "services": container.innerHTML = renderServices(); break;
        case "map": container.innerHTML = renderMap(); break;
        case "help": container.innerHTML = renderHelp(); break;
        case "petitions": container.innerHTML = renderPetitions(); break;
        case "news": container.innerHTML = renderNews(); break;
        case "health": container.innerHTML = renderHealth(); break;
        case "jobs": container.innerHTML = renderJobs(); break;
        case "edu": container.innerHTML = renderEdu(); break;
        case "contact": container.innerHTML = renderContact(); break;
        case "admin": container.innerHTML = renderAdmin(); break;
        default: container.innerHTML = renderHome();
      }

      if (state.page === "map") {
        setTimeout(initLeafletMap, 50);
      }
    }

    document.addEventListener("DOMContentLoaded", async function () {
      document.getElementById("auth-crest-top").innerHTML = renderDushetiCrest("w-10 h-11");
      document.getElementById("auth-crest-center").innerHTML = renderDushetiCrest("w-16 h-20");
      document.getElementById("nav-crest").innerHTML = renderDushetiCrest("w-10 h-11");
      document.getElementById("footer-crest").innerHTML = renderDushetiCrest("w-10 h-11");

      await loadSavedState();

      try {
        const savedUser = localStorage.getItem("dd_active_user");
        if (savedUser) {
          const parsedUser = JSON.parse(savedUser);
          if (parsedUser && parsedUser.firstName && parsedUser.lastName) {
            state.user = parsedUser;
            showPortal();
          }
        }
      } catch (err) {}
    });
  </script>
</body>
</html>
