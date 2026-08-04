<template>
  <!-- ============================================================ -->
  <!--  لایه Mouse Trail (ستاره‌های دنبال‌کننده موس)                -->
  <!-- ============================================================ -->
  <div
    class="app"
    :class="[theme, { 'lang-fa': isFa, 'lang-en': !isFa }]"
    :dir="isFa ? 'rtl' : 'ltr'"
    :lang="isFa ? 'fa' : 'en'"
  >
    <div class="mouse-trail-layer">
      <span
        v-for="star in stars"
        :key="star.id"
        class="trail-star"
        :style="{
          left: `${star.x}px`,
          top: `${star.y}px`,
          width: `${star.size}px`,
          height: `${star.size}px`,
          opacity: star.opacity,
          transform: `translate(-50%, -50%) scale(${star.scale}) rotate(${star.rotate}deg)`,
          background: star.color,
          boxShadow: `0 0 6px ${star.glow}, 0 0 14px ${star.glow}, 0 0 24px ${star.glow}`,
        }"
      ></span>
    </div>

    <!-- ============================================================ -->
    <!--  پس‌زمینه‌های تزیینی                                        -->
    <!-- ============================================================ -->
    <div class="bg-base"></div>
    <div class="bg-mesh"></div>
    <div class="bg-grid"></div>
    <div class="bg-noise"></div>
    <div class="aurora aurora-1"></div>
    <div class="aurora aurora-2"></div>
    <div class="aurora aurora-3"></div>

    <!-- ============================================================ -->
    <!--  نوار بالای سایت (App Bar / Navbar)                         -->
    <!-- ============================================================ -->
    <header
      class="navbar"
      :dir="isFa ? 'rtl' : 'ltr'"
      :style="{
        transform: isNavVisible ? 'translateY(0)' : 'translateY(-100%)',
      }"
    >
      <div class="container nav-shell">
        <div class="nav-col nav-col-start">
          <a href="#home" class="brand">
            <div class="brand-mark">
              <span class="brand-mark-text" :class="{ 'show-ak': showAk }">
                {{ showAk ? "AK" : "Hi" }}
              </span>
            </div>
            <div class="brand-copy">
              <strong>{{ t.brand }}</strong>
              <div class="brand-role-marquee">
                <div class="brand-role-track">
                  <span>{{ t.hero.role }}</span>
                  <span>{{ t.hero.role }}</span>
                </div>
              </div>
            </div>
          </a>
        </div>

        <div class="nav-col nav-col-center">
          <nav class="nav-links">
            <a href="#home">{{ t.nav.home }}</a>
            <a href="#skills">{{ t.nav.skills }}</a>
            <a href="#about">{{ t.nav.about }}</a>
            <a href="#projects">{{ t.nav.projects }}</a>
            <a href="#contact">{{ t.nav.contact }}</a>
          </nav>
        </div>

        <div class="nav-col nav-col-end">
          <div class="nav-actions">
            <button
              class="switch-btn lang-switch"
              @click="toggleLang"
              type="button"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap">
                  <span class="switch-icon">🌐</span>
                </span>
                <span class="switch-text-wrap">
                  <span class="switch-text" :key="lang">
                    {{ isFa ? "English" : "فارسی" }}
                  </span>
                </span>
              </span>
            </button>

            <button
              class="switch-btn theme-switch"
              @click="toggleTheme"
              type="button"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap">
                  <span class="switch-icon" :key="theme">
                    {{ theme === "dark" ? "☀️" : "🌙" }}
                  </span>
                </span>
                <span class="switch-text-wrap">
                  <span class="switch-text" :key="theme + '-text'">
                    {{ theme === "dark" ? t.actions.light : t.actions.dark }}
                  </span>
                </span>
              </span>
            </button>
          </div>

          <button
            class="hamburger-btn"
            @click.stop="toggleMenu"
            :class="{ active: menuOpen }"
            type="button"
          >
            <span></span>
            <span></span>
            <span></span>
          </button>
        </div>
      </div>
    </header>

    <!-- ============================================================ -->
    <!--  اُورلی منوی موبایل (تمام‌صفحه با Blur)                      -->
    <!-- ============================================================ -->
    <Transition name="mobile-menu">
      <div
        v-if="menuOpen"
        class="mobile-menu-overlay"
        @click="menuOpen = false"
      >
        <div class="mobile-menu-panel" @click.stop>
          <nav class="mobile-nav-links">
            <a href="#home" @click="menuOpen = false">{{ t.nav.home }}</a>
            <a href="#skills" @click="menuOpen = false">{{ t.nav.skills }}</a>
            <a href="#about" @click="menuOpen = false">{{ t.nav.about }}</a>
            <a href="#projects" @click="menuOpen = false">{{
              t.nav.projects
            }}</a>
            <a href="#contact" @click="menuOpen = false">{{ t.nav.contact }}</a>
          </nav>

          <div class="mobile-actions">
            <button
              class="switch-btn lang-switch"
              @click="toggleLang"
              type="button"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap">
                  <span class="switch-icon">🌐</span>
                </span>
                <span class="switch-text-wrap">
                  <span class="switch-text" :key="lang">
                    {{ isFa ? "English" : "فارسی" }}
                  </span>
                </span>
              </span>
            </button>

            <button
              class="switch-btn theme-switch"
              @click="toggleTheme"
              type="button"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap">
                  <span class="switch-icon" :key="theme">
                    {{ theme === "dark" ? "☀️" : "🌙" }}
                  </span>
                </span>
                <span class="switch-text-wrap">
                  <span class="switch-text" :key="theme + '-text'">
                    {{ theme === "dark" ? t.actions.light : t.actions.dark }}
                  </span>
                </span>
              </span>
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- ============================================================ -->
    <!--  بخش Hero (خانه)                                            -->
    <!-- ============================================================ -->
    <section id="home" class="hero full-screen-section">
      <div class="hero-overlay-line"></div>
      <div class="container hero-grid">
        <div class="hero-left">
          <div class="hero-chip">
            <span class="live-dot"></span>
            {{ t.hero.badge }}
          </div>

          <h1 class="hero-title">
            <span class="hero-title-muted">{{ t.hero.title1 }}</span>
            <span class="hero-name">{{ t.hero.name }}</span>
            <span class="hero-title-muted">{{ t.hero.title2 }}</span>
          </h1>

          <h2 class="hero-subtitle">
            <span>{{ t.hero.typingPrefix }}</span>
            <span class="typing-word">{{ displayedWord }}</span>
            <span class="cursor">|</span>
          </h2>

          <p class="hero-desc">
            {{ t.hero.description }}
          </p>

          <div class="hero-actions">
            <a href="#projects" class="premium-btn primary">
              <span class="btn-liquid"></span>
              <span class="btn-content">{{ t.hero.primaryBtn }}</span>
            </a>

            <a href="#contact" class="premium-btn ghost">
              <span class="btn-liquid"></span>
              <span class="btn-content">{{ t.hero.secondaryBtn }}</span>
            </a>
          </div>

          <div class="floating-techs">
            <span v-for="item in t.hero.techs" :key="item">{{ item }}</span>
          </div>
        </div>

        <div class="hero-right">
          <div class="glass-stage">
            <div class="stage-ring stage-ring-1"></div>
            <div class="stage-ring stage-ring-2"></div>

            <div class="stage-core">
              <!-- لایه شیشه‌ای داخلی با blur -->
              <div class="glass-bg"></div>

              <!-- ===== مانیتور شیک و کاربرپسند ===== -->
              <div class="dev-core-container">
                <!-- مدار کدها -->
                <div class="code-orbit">
                  <span class="orbit-snippet snippet-1"
                    >const app = Vue.createApp()</span
                  >
                  <span class="orbit-snippet snippet-2"
                    >import { ref } from 'vue'</span
                  >
                  <span class="orbit-snippet snippet-3"
                    >export default { ... }</span
                  >
                  <span class="orbit-snippet snippet-4">app.mount('#app')</span>
                </div>

                <!-- مانیتور -->
                <div class="monitor-wrapper">
                  <div class="monitor">
                    <div class="monitor-screen">
                      <div class="screen-glare"></div>
                      <div class="mini-terminal">
                        <div class="terminal-header">
                          <span class="dot red"></span>
                          <span class="dot yellow"></span>
                          <span class="dot green"></span>
                          <span class="title">app.vue</span>
                        </div>
                        <div class="terminal-body">
                          <div class="code-line">
                            <span class="code-keyword">import</span>
                            <span class="code-var">{ ref }</span>
                            <span class="code-keyword">from</span>
                            <span class="code-string">'vue'</span>
                          </div>
                          <div class="code-line">
                            <span class="code-keyword">const</span>
                            <span class="code-var">app</span> =
                            <span class="code-func">createApp</span>({...})
                          </div>
                          <div class="code-line">
                            <span class="code-keyword">export</span>
                            <span class="code-keyword">default</span> { ... }
                          </div>
                          <div class="code-line cursor-line">
                            <span class="blinking-cursor">|</span>
                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="monitor-stand">
                      <div class="stand-neck"></div>
                      <div class="stand-base"></div>
                    </div>
                  </div>
                </div>

                <!-- حلقه‌های نئونی -->
                <div class="neon-ring ring-1"></div>
                <div class="neon-ring ring-2"></div>
                <div class="neon-ring ring-3"></div>

                <!-- ذرات پویا -->
                <div class="particles">
                  <span
                    v-for="p in 8"
                    :key="p"
                    class="particle"
                    :style="{ '--i': p }"
                  ></span>
                </div>
              </div>

              <!-- کلاس force-sharp برای حفظ وضوح متن -->
              <div class="identity force-sharp">
                <h3>{{ t.hero.name }}</h3>
                <p>{{ t.hero.role }}</p>
              </div>

              <div class="skill-cloud">
                <span v-for="item in t.hero.techs" :key="item">{{ item }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <a href="#skills" class="scroll-indicator" aria-label="Scroll Down">
        <span></span>
      </a>
    </section>

    <!-- ============================================================ -->
    <!--  بخش مهارت‌ها (Skills)                                      -->
    <!-- ============================================================ -->
    <section id="skills" class="section flush-section">
      <div class="container">
        <div class="section-heading">
          <span class="section-badge">{{ t.skillsSection.tag }}</span>
          <h2>{{ t.skillsSection.title }}</h2>
          <p>{{ t.skillsSection.desc }}</p>
        </div>

        <div class="skills-grid">
          <article
            class="skill-panel"
            v-for="skill in localizedSkills"
            :key="skill.name"
          >
            <div class="panel-line"></div>
            <div class="skill-head">
              <div class="skill-icon">{{ skill.icon }}</div>
              <h3>{{ skill.name }}</h3>
            </div>
            <p>{{ skill.description }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ============================================================ -->
    <!--  بخش درباره من (About)                                      -->
    <!-- ============================================================ -->
    <section id="about" class="section flush-section soft-surface">
      <div class="container about-grid">
        <div class="about-main">
          <span class="section-badge">{{ t.about.tag }}</span>
          <h2>{{ t.about.title }}</h2>
          <p>{{ t.about.p1 }}</p>
          <p>{{ t.about.p2 }}</p>

          <div class="about-points">
            <div
              class="about-point"
              v-for="point in t.about.points"
              :key="point"
            >
              <span>✦</span>
              <p>{{ point }}</p>
            </div>
          </div>
        </div>

        <div class="about-side">
          <div
            class="about-card"
            v-for="card in t.about.cards"
            :key="card.title"
            :class="{ accent: card.accent }"
          >
            <h3>{{ card.title }}</h3>
            <p>{{ card.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ============================================================ -->
    <!--  بخش پروژه‌ها (Projects)                                    -->
    <!-- ============================================================ -->
    <section id="projects" class="section flush-section">
      <div class="container">
        <div class="section-heading">
          <span class="section-badge">{{ t.projectsSection.tag }}</span>
          <h2>{{ t.projectsSection.title }}</h2>
          <p>{{ t.projectsSection.desc }}</p>
        </div>

        <div class="projects-grid">
          <article
            class="project-panel"
            v-for="project in localizedProjects"
            :key="project.title"
          >
            <div class="project-head">
              <span class="project-type">{{ project.type }}</span>
              <div class="project-line"></div>
            </div>

            <h3>{{ project.title }}</h3>
            <p>{{ project.description }}</p>

            <div class="project-tags">
              <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- ============================================================ -->
    <!--  بخش تماس (Contact)                                         -->
    <!-- ============================================================ -->
    <section id="contact" class="section flush-section soft-surface">
      <div class="container">
        <div class="contact-shell">
          <span class="section-badge">{{ t.contact.tag }}</span>
          <h2>{{ t.contact.title }}</h2>
          <p>{{ t.contact.desc }}</p>

          <div class="contact-actions">
            <a
              href="https://mail.google.com/mail/?view=cm&fs=1&to=arian.kalantari@yamil.com"
              target="_blank"
              rel="noopener"
              class="premium-btn primary"
            >
              <span class="btn-liquid"></span>
              <span class="btn-content">{{ t.contact.emailBtn }}</span>
            </a>

            <a href="#" class="premium-btn ghost">
              <span class="btn-liquid"></span>
              <span class="btn-content">{{ t.contact.resumeBtn }}</span>
            </a>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount, watch } from "vue";

const lang = ref("fa");
const theme = ref("dark");
const isFa = computed(() => lang.value === "fa");
const menuOpen = ref(false);
const showAk = ref(false);
const isNavVisible = ref(true);
let lastScrollY = 0;

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value;
};

const translations = {
  fa: {
    brand: "آرین کلانتری",
    nav: {
      home: "خانه",
      skills: "مهارت‌ها",
      about: "درباره من",
      projects: "پروژه‌ها",
      contact: "ارتباط",
    },
    actions: { light: "روشن", dark: "دارک" },
    hero: {
      badge: "توسعه‌دهنده فرانت‌اند، موبایل و تجربه کاربری",
      title1: "سلام، من",
      name: "آرین کلانتری",
      title2: "هستم",
      typingPrefix: "مسلط به ",
      description:
        "من روی ساخت تجربه‌های دیجیتال شیک، سریع، روان و کاربرمحور تمرکز دارم؛ از رابط‌های مدرن وب با Vue گرفته تا اپلیکیشن‌های Flutter، منطق با JavaScript و Dart، کار با Go و طراحی حرفه‌ای UI/UX.",
      primaryBtn: "مشاهده پروژه‌ها",
      secondaryBtn: "ارتباط با من",
      role: "Frontend, Flutter, Go & UI/UX Developer",
      techs: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
    },
    skillsSection: {
      tag: "زبان‌ها و مهارت‌ها",
      title: "فناوری‌هایی که با آن‌ها تجربه می‌سازم",
      desc: "تمرکز من روی طراحی و توسعه محصولاتی است که هم از نظر بصری حرفه‌ای باشند و هم از نظر تجربه کاربر، روان، سریع و قابل اعتماد عمل کنند.",
    },
    about: {
      tag: "درباره من",
      title: "ترکیب طراحی، منطق و تجربه کاربری در یک مسیر حرفه‌ای",
      p1: "من به ساخت محصولاتی علاقه دارم که صرفاً زیبا نباشند، بلکه حس کیفیت، دقت و روان بودن را به کاربر منتقل کنند.",
      p2: "در توسعه فرانت‌اند، اپلیکیشن موبایل و UI/UX، تمرکز من روی خروجی‌هایی است که هم شکیل باشند و هم واقعاً کاربرپسند.",
      points: [
        "طراحی مدرن، برندمحور و چشم‌نواز",
        "تعاملات نرم و تجربه کاربری روان",
        "ساختار تمیز و مناسب برای توسعه آینده",
      ],
      cards: [
        { title: "Vue.js", desc: "برای ساخت رابط‌های مدرن، سریع و تعاملی" },
        { title: "Flutter", desc: "برای اپلیکیشن‌های موبایل زیبا و روان" },
        { title: "Go", desc: "برای درک و پیاده‌سازی منطق سریع و سبک" },
        {
          title: "UI / UX",
          desc: "برای طراحی تجربه‌های حرفه‌ای و کاربرپسند",
          accent: true,
        },
      ],
    },
    projectsSection: {
      tag: "پروژه‌ها",
      title: "خروجی‌هایی با حس حرفه‌ای، مدرن و قابل ارائه",
      desc: "از لندینگ‌پیج و پنل مدیریتی تا اپلیکیشن موبایل و طراحی رابط، هدف من ساخت تجربه‌هایی است که متمایز، تمیز و کاربردی باشند.",
    },
    contact: {
      tag: "ارتباط با من",
      title: "برای همکاری در پروژه‌های خود در تماس باشید",
      desc: "اگر به یک وب‌سایت شیک، رابط کاربری مدرن، اپ Flutter یا طراحی UI/UX نیاز دارید، خوشحال می‌شوم همکاری کنیم.",
      emailBtn: "ارسال ایمیل",
      resumeBtn: "مشاهده اطلاعات بیشتر",
    },
    words: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
  },
  en: {
    brand: "Arian Kalantari",
    nav: {
      home: "Home",
      skills: "Skills",
      about: "About",
      projects: "Projects",
      contact: "Contact",
    },
    actions: { light: "Light", dark: "Dark" },
    hero: {
      badge: "Frontend, Mobile & User Experience Developer",
      title1: "Hi, I am",
      name: "Arian Kalantari",
      title2: "",
      typingPrefix: "Skilled in ",
      description:
        "I focus on crafting elegant, fast, smooth and user-centered digital experiences — from modern web interfaces with Vue to Flutter apps, logic with JavaScript and Dart, working with Go, and professional UI/UX design.",
      primaryBtn: "View Projects",
      secondaryBtn: "Contact Me",
      role: "Frontend, Flutter, Go & UI/UX Developer",
      techs: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
    },
    skillsSection: {
      tag: "Languages & Skills",
      title: "Technologies I use to craft experiences",
      desc: "My focus is on designing and building products that feel visually polished while delivering smooth, fast and dependable user experiences.",
    },
    about: {
      tag: "About Me",
      title:
        "Blending design, logic and user experience into one polished direction",
      p1: "I enjoy building products that are not only visually attractive, but also communicate quality, clarity and smoothness to the user.",
      p2: "In frontend development, mobile apps and UI/UX, I focus on outputs that feel elegant, practical and truly user-friendly.",
      points: [
        "Modern, brand-oriented and polished design",
        "Smooth interactions and thoughtful UX",
        "Clean structure ready for future growth",
      ],
      cards: [
        {
          title: "Vue.js",
          desc: "For building modern, fast and interactive interfaces",
        },
        {
          title: "Flutter",
          desc: "For smooth and beautiful mobile applications",
        },
        {
          title: "Go",
          desc: "For understanding and building fast, lightweight logic",
        },
        {
          title: "UI / UX",
          desc: "For designing polished and user-friendly experiences",
          accent: true,
        },
      ],
    },
    projectsSection: {
      tag: "Projects",
      title: "Outputs with a modern, premium and presentable feel",
      desc: "From landing pages and admin dashboards to mobile apps and interface design, my goal is to create experiences that feel distinct, clean and practical.",
    },
    contact: {
      tag: "Contact",
      title: "Let's work together on polished digital products",
      desc: "If you need a beautiful website, modern UI, Flutter app or UI/UX design, I would be happy to collaborate.",
      emailBtn: "Send Email",
      resumeBtn: "More Information",
    },
    words: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
  },
};

const t = computed(() => translations[lang.value]);

const localizedSkills = computed(() => {
  return lang.value === "fa"
    ? [
        {
          name: "Vue.js",
          icon: "🟢",
          description:
            "ساخت رابط‌های مدرن، سریع و تمیز برای وب‌اپلیکیشن‌ها و صفحات حرفه‌ای.",
        },
        {
          name: "Flutter",
          icon: "🔵",
          description:
            "توسعه اپلیکیشن‌های موبایل با تجربه‌ای روان، زیبا و کاربرپسند.",
        },
        {
          name: "JavaScript",
          icon: "🟡",
          description:
            "پیاده‌سازی منطق تعاملی، داینامیک و توسعه‌پذیر برای رابط‌های مدرن.",
        },
        {
          name: "Dart",
          icon: "🧩",
          description:
            "کدنویسی ساخت‌یافته و مناسب برای اپلیکیشن‌های Flutter با نگهداری ساده.",
        },
        {
          name: "Go",
          icon: "⚙️",
          description:
            "کار با منطق سریع و سبک برای توسعه سرویس‌ها و درک بهتر معماری بک‌اند.",
        },
        {
          name: "UI / UX",
          icon: "✨",
          description:
            "طراحی تجربه کاربری و رابط کاربری با تمرکز بر وضوح، زیبایی و رفتار کاربر.",
        },
      ]
    : [
        {
          name: "Vue.js",
          icon: "🟢",
          description:
            "Building modern, fast and clean interfaces for web applications and premium pages.",
        },
        {
          name: "Flutter",
          icon: "🔵",
          description:
            "Developing mobile applications with smooth, elegant and user-friendly experiences.",
        },
        {
          name: "JavaScript",
          icon: "🟡",
          description:
            "Implementing interactive, dynamic and scalable logic for modern interfaces.",
        },
        {
          name: "Dart",
          icon: "🧩",
          description:
            "Structured development for Flutter apps with cleaner maintenance and readability.",
        },
        {
          name: "Go",
          icon: "⚙️",
          description:
            "Working with fast and lightweight logic for services and backend-oriented thinking.",
        },
        {
          name: "UI / UX",
          icon: "✨",
          description:
            "Designing user interfaces and experiences focused on clarity, beauty and behavior.",
        },
      ];
});

const localizedProjects = computed(() => {
  return lang.value === "fa"
    ? [
        {
          title: "لندینگ‌پیج مدرن و کاربرپسند",
          type: "Landing Page",
          description:
            "طراحی و پیاده‌سازی صفحه‌ای مدرن با تمرکز بر زیبایی بصری، اعتماد و تجربه کاربری روان.",
          tags: ["Vue", "Responsive", "UI Design"],
        },
        {
          title: "اپلیکیشن موبایل حرفه‌ای",
          type: "Mobile App",
          description:
            "ساخت اپ Flutter با طراحی تمیز، تعاملات نرم و ساختاری مناسب برای رشد آینده.",
          tags: ["Flutter", "Dart", "UX"],
        },
        {
          title: "پنل مدیریتی مدرن",
          type: "Dashboard",
          description:
            "رابطی حرفه‌ای و کاربردی برای مدیریت داده‌ها با ساختاری تمیز و توسعه‌پذیر.",
          tags: ["Vue.js", "JavaScript", "Admin UI"],
        },
      ]
    : [
        {
          title: "Modern User-Friendly Landing Page",
          type: "Landing Page",
          description:
            "A modern page focused on visual quality, trust and a smooth user experience.",
          tags: ["Vue", "Responsive", "UI Design"],
        },
        {
          title: "Professional Mobile App",
          type: "Mobile App",
          description:
            "A Flutter app with clean design, soft interactions and a structure ready for future growth.",
          tags: ["Flutter", "Dart", "UX"],
        },
        {
          title: "Modern Admin Panel",
          type: "Dashboard",
          description:
            "A professional and practical interface for managing data with clean structure and scalability.",
          tags: ["Vue.js", "JavaScript", "Admin UI"],
        },
      ];
});

const displayedWord = ref("");
let wordIndex = 0;
let charIndex = 0;
let deleting = false;
let typingTimeout = null;

const typeEffect = () => {
  const words = t.value.words;
  const currentWord = words[wordIndex];

  if (!deleting) {
    displayedWord.value = currentWord.slice(0, charIndex + 1);
    charIndex++;
    if (charIndex === currentWord.length) {
      deleting = true;
      typingTimeout = setTimeout(typeEffect, 1200);
      return;
    }
  } else {
    displayedWord.value = currentWord.slice(0, charIndex - 1);
    charIndex--;
    if (charIndex === 0) {
      deleting = false;
      wordIndex = (wordIndex + 1) % words.length;
    }
  }

  typingTimeout = setTimeout(typeEffect, deleting ? 40 : 78);
};

const resetTyping = () => {
  if (typingTimeout) clearTimeout(typingTimeout);
  displayedWord.value = "";
  wordIndex = 0;
  charIndex = 0;
  deleting = false;
  typeEffect();
};

const toggleTheme = () => {
  theme.value = theme.value === "dark" ? "light" : "dark";
};

const toggleLang = () => {
  lang.value = lang.value === "fa" ? "en" : "fa";
};

const stars = ref([]);
let starId = 0;
let lastEmit = 0;
let mouseMoveHandler = null;
let mouseOutHandler = null;

const pick = (arr) => arr[Math.floor(Math.random() * arr.length)];

const getPalette = () => {
  return theme.value === "dark"
    ? [
        { color: "#ffffff", glow: "rgba(255,255,255,0.92)" },
        { color: "#9be7ff", glow: "rgba(155,231,255,0.88)" },
        { color: "#b9acff", glow: "rgba(185,172,255,0.86)" },
        { color: "#95ffd9", glow: "rgba(149,255,217,0.82)" },
      ]
    : [
        { color: "#ffffff", glow: "rgba(255,255,255,0.92)" },
        { color: "#7c3aed", glow: "rgba(124,58,237,0.28)" },
        { color: "#0ea5e9", glow: "rgba(14,165,233,0.30)" },
        { color: "#14b8a6", glow: "rgba(20,184,166,0.26)" },
      ];
};

const createSoftStar = (x, y, spread = 12) => {
  const tone = pick(getPalette());
  const id = starId++;

  const item = {
    id,
    x: x + (Math.random() * spread - spread / 2),
    y: y + (Math.random() * spread - spread / 2),
    size: Math.random() * 3 + 2,
    opacity: 1,
    scale: 1,
    rotate: Math.random() * 180,
    color: tone.color,
    glow: tone.glow,
    vx: Math.random() * 0.45 - 0.225,
    vy: Math.random() * -0.55 - 0.1,
  };

  stars.value.push(item);

  const duration = 320 + Math.random() * 260;
  const start = performance.now();

  const animate = (now) => {
    const star = stars.value.find((s) => s.id === id);
    if (!star) return;

    const progress = (now - start) / duration;
    if (progress >= 1) {
      stars.value = stars.value.filter((s) => s.id !== id);
      return;
    }

    star.opacity = 1 - progress;
    star.scale = 0.9 + progress * 0.55;
    star.x += star.vx;
    star.y += star.vy;
    star.rotate += 0.7;

    requestAnimationFrame(animate);
  };

  requestAnimationFrame(animate);
};

const handleGlobalMouseMove = (e) => {
  const now = performance.now();
  if (now - lastEmit < 10) return;
  lastEmit = now;

  createSoftStar(e.clientX, e.clientY, 10);
  if (Math.random() > 0.42) createSoftStar(e.clientX, e.clientY, 16);
};

const clearTrail = () => {
  stars.value = [];
};

const handleScroll = () => {
  const currentScrollY = window.scrollY;
  if (currentScrollY > lastScrollY && currentScrollY > 100) {
    isNavVisible.value = false;
  } else {
    isNavVisible.value = true;
  }
  lastScrollY = currentScrollY;
};

onMounted(() => {
  const savedTheme = localStorage.getItem("ak-theme");
  const savedLang = localStorage.getItem("ak-lang");
  if (savedTheme === "dark" || savedTheme === "light") theme.value = savedTheme;
  if (savedLang === "fa" || savedLang === "en") lang.value = savedLang;

  document.documentElement.setAttribute("dir", isFa.value ? "rtl" : "ltr");
  document.documentElement.setAttribute("lang", lang.value);

  typeEffect();

  setTimeout(() => {
    showAk.value = true;
  }, 5000);

  mouseMoveHandler = (e) => handleGlobalMouseMove(e);
  mouseOutHandler = (e) => {
    if (!e.relatedTarget && !e.toElement) clearTrail();
  };

  window.addEventListener("mousemove", mouseMoveHandler, { passive: true });
  window.addEventListener("mouseout", mouseOutHandler);
  window.addEventListener("scroll", handleScroll);
});

onBeforeUnmount(() => {
  if (typingTimeout) clearTimeout(typingTimeout);
  if (mouseMoveHandler)
    window.removeEventListener("mousemove", mouseMoveHandler);
  if (mouseOutHandler) window.removeEventListener("mouseout", mouseOutHandler);
  window.removeEventListener("scroll", handleScroll);
});

watch(theme, (val) => localStorage.setItem("ak-theme", val));
watch(lang, (val) => {
  localStorage.setItem("ak-lang", val);
  document.documentElement.setAttribute("dir", isFa.value ? "rtl" : "ltr");
  document.documentElement.setAttribute("lang", val);
  resetTyping();
});
</script>

<style scoped>
/* ================================================================= */
/*  Reset & Base                                                     */
/* ================================================================= */
:global(*),
:global(*::before),
:global(*::after) {
  box-sizing: border-box;
  min-width: 0;
}

:global(html) {
  scroll-behavior: smooth;
  scroll-padding-top: 100px;
}

:global(body) {
  margin: 0;
  font-family: "Plus Jakarta Sans", "Vazirmatn", sans-serif;
  font-weight: 400;
  background: #07111f;
  font-feature-settings: "ss02", "ss03";
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
}

:global(a) {
  text-decoration: none;
}

:global(button),
:global(input),
:global(textarea),
:global(select) {
  font-family: inherit;
}

:global(img),
:global(svg),
:global(video),
:global(canvas) {
  max-width: 100%;
  height: auto;
  display: block;
}

/* ================================================================= */
/*  Theme Variables                                                  */
/* ================================================================= */
.app {
  --bg-main: #07111f;
  --bg-2: #091321;
  --surface: rgba(255, 255, 255, 0.04);
  --surface-strong: rgba(255, 255, 255, 0.065);
  --surface-soft: rgba(255, 255, 255, 0.03);
  --text-main: #f7fbff;
  --text-soft: #b3c0d1;
  --text-muted: #8fa1b5;
  --border: rgba(255, 255, 255, 0.08);
  --border-strong: rgba(126, 202, 255, 0.18);
  --primary: #68dcff;
  --primary-2: #7f67ff;
  --accent: #8bffd2;
  --shadow: 0 20px 60px rgba(0, 0, 0, 0.24);
  --nav-bg: rgba(7, 17, 31, 0.58);
  --hero-bg:
    radial-gradient(
      circle at 10% 10%,
      rgba(104, 220, 255, 0.14),
      transparent 26%
    ),
    radial-gradient(
      circle at 90% 10%,
      rgba(127, 103, 255, 0.14),
      transparent 26%
    ),
    radial-gradient(
      circle at 50% 85%,
      rgba(139, 255, 210, 0.08),
      transparent 24%
    ),
    linear-gradient(180deg, #06101d 0%, #081321 40%, #07111f 100%);
  --mesh-1: rgba(104, 220, 255, 0.1);
  --mesh-2: rgba(127, 103, 255, 0.1);
  --mesh-3: rgba(139, 255, 210, 0.06);
  --grid: rgba(255, 255, 255, 0.035);
  --soft-section: rgba(255, 255, 255, 0.02);
  --btn-ghost: rgba(255, 255, 255, 0.05);

  position: relative;
  min-height: 100vh;
  overflow-x: hidden;
  color: var(--text-main);
  background: var(--hero-bg);
  padding-top: 88px; /* height of navbar */
}

.app.light {
  --bg-main: #f6f9ff;
  --bg-2: #edf4ff;
  --surface: rgba(255, 255, 255, 0.45);
  --surface-strong: rgba(255, 255, 255, 0.72);
  --surface-soft: rgba(255, 255, 255, 0.36);
  --text-main: #0f172a;
  --text-soft: #4e5d73;
  --text-muted: #6c7a90;
  --border: rgba(15, 23, 42, 0.08);
  --border-strong: rgba(76, 111, 255, 0.14);
  --primary: #0ea5e9;
  --primary-2: #7c3aed;
  --accent: #14b8a6;
  --shadow: 0 18px 48px rgba(91, 117, 171, 0.12);
  --nav-bg: rgba(255, 255, 255, 0.62);
  --hero-bg:
    radial-gradient(
      circle at 10% 10%,
      rgba(14, 165, 233, 0.12),
      transparent 28%
    ),
    radial-gradient(
      circle at 90% 10%,
      rgba(124, 58, 237, 0.12),
      transparent 28%
    ),
    radial-gradient(
      circle at 50% 85%,
      rgba(20, 184, 166, 0.07),
      transparent 24%
    ),
    linear-gradient(180deg, #f8fbff 0%, #eef5ff 46%, #f7faff 100%);
  --mesh-1: rgba(14, 165, 233, 0.1);
  --mesh-2: rgba(124, 58, 237, 0.08);
  --mesh-3: rgba(20, 184, 166, 0.06);
  --grid: rgba(15, 23, 42, 0.05);
  --soft-section: rgba(255, 255, 255, 0.22);
  --btn-ghost: rgba(255, 255, 255, 0.62);
}

/* ================================================================= */
/*  Background Layers                                                */
/* ================================================================= */
.bg-base,
.bg-mesh,
.bg-grid,
.bg-noise,
.aurora {
  position: fixed;
  inset: 0;
  pointer-events: none;
}

.bg-base {
  z-index: 0;
  background: var(--hero-bg);
}

.bg-mesh {
  z-index: 0;
  background:
    radial-gradient(circle at 20% 25%, var(--mesh-1), transparent 24%),
    radial-gradient(circle at 75% 22%, var(--mesh-2), transparent 24%),
    radial-gradient(circle at 48% 80%, var(--mesh-3), transparent 26%);
  filter: blur(10px);
  animation: meshMove 18s ease-in-out infinite alternate;
}

.bg-grid {
  z-index: 0;
  background-image:
    linear-gradient(var(--grid) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid) 1px, transparent 1px);
  background-size: 42px 42px;
  mask-image: radial-gradient(circle at center, black 34%, transparent 96%);
  -webkit-mask-image: radial-gradient(
    circle at center,
    black 34%,
    transparent 96%
  );
}

.bg-noise {
  z-index: 0;
  opacity: 0.05;
  background-image:
    radial-gradient(circle at 20% 20%, #fff 1px, transparent 1px),
    radial-gradient(circle at 80% 40%, #fff 1px, transparent 1px),
    radial-gradient(circle at 30% 70%, #fff 1px, transparent 1px),
    radial-gradient(circle at 60% 90%, #fff 1px, transparent 1px);
  background-size: 170px 170px;
}

.aurora {
  z-index: 0;
  filter: blur(90px);
  opacity: 0.34;
  animation: auroraFloat 16s ease-in-out infinite alternate;
}

.aurora-1 {
  background: radial-gradient(
    circle,
    rgba(104, 220, 255, 0.28),
    transparent 65%
  );
  transform: translate(-12%, -8%);
}

.aurora-2 {
  background: radial-gradient(
    circle,
    rgba(127, 103, 255, 0.26),
    transparent 65%
  );
  transform: translate(38%, 10%);
  animation-delay: 3s;
}

.aurora-3 {
  background: radial-gradient(
    circle,
    rgba(139, 255, 210, 0.18),
    transparent 65%
  );
  transform: translate(6%, 55%);
  animation-delay: 6s;
}

/* ================================================================= */
/*  Mouse Trail                                                      */
/* ================================================================= */
.mouse-trail-layer {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 90;
}

.trail-star {
  position: fixed;
  border-radius: 50%;
  pointer-events: none;
  will-change: transform, opacity;
}

/* ================================================================= */
/*  Layout                                                           */
/* ================================================================= */
.container {
  width: min(1280px, calc(100% - 40px));
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

.section {
  position: relative;
  padding: 120px 0;
  z-index: 2;
}

.flush-section {
  border-top: 1px solid rgba(255, 255, 255, 0.03);
}

.soft-surface {
  background: linear-gradient(
    180deg,
    transparent,
    var(--soft-section),
    transparent
  );
}

.full-screen-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
}

.section-heading {
  text-align: center;
  max-width: 880px;
  margin: 0 auto 60px;
}

.section-badge {
  display: inline-flex;
  align-items: center;
  padding: 10px 16px;
  border-radius: 999px;
  background: var(--surface);
  border: 1px solid var(--border-strong);
  color: var(--primary);
  font-size: 14px;
  font-weight: 800;
  margin-bottom: 16px;
  backdrop-filter: blur(14px);
}

.section-heading h2,
.about-main h2,
.contact-shell h2 {
  margin: 0 0 16px;
  font-size: clamp(36px, 4vw, 60px);
  line-height: 1.12;
  color: var(--text-main);
  font-weight: 800;
  letter-spacing: -0.02em;
}

.section-heading p,
.about-main p,
.contact-shell p {
  margin: 0;
  color: var(--text-soft);
  font-size: 17px;
  font-weight: 400;
  line-height: 1.85;
}

/* ================================================================= */
/*  Navbar                                                           */
/* ================================================================= */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 60;
  direction: ltr !important;
  backdrop-filter: blur(18px);
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.05),
    rgba(255, 255, 255, 0.015)
  );
  border-bottom: 1px solid var(--border);
  transition: transform 0.3s ease;
}

.nav-shell {
  min-height: 88px;
  display: grid;
  grid-template-columns: minmax(250px, 290px) 1fr minmax(250px, 290px);
  align-items: center;
  gap: 18px;
}

.nav-col {
  display: flex;
  align-items: center;
  min-width: 0;
}

.nav-col-start {
  justify-content: flex-start;
}

.nav-col-center {
  justify-content: center;
}

.nav-col-end {
  justify-content: flex-end;
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  color: var(--text-main);
  min-width: 0;
  direction: ltr;
  text-align: left;
}

/* ================================================================ */
/* Compact Premium Brand Mark - Blue Accent + Soft Shine            */
/* ================================================================ */

.brand-mark {
  width: 38px;
  height: 38px;
  border-radius: 12px;
  display: grid;
  place-items: center;
  position: relative;
  overflow: hidden;
  flex-shrink: 0;
  isolation: isolate;

  background:
    linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.07),
      rgba(255, 255, 255, 0.025)
    ),
    linear-gradient(145deg, #1a1d26 0%, #12151c 100%);
  border: 1px solid rgba(120, 160, 255, 0.14);

  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    0 6px 18px rgba(0, 0, 0, 0.24),
    0 0 18px rgba(90, 140, 255, 0.08);

  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  transition:
    transform 0.28s ease,
    box-shadow 0.28s ease,
    border-color 0.28s ease,
    filter 0.28s ease;
}

.brand-mark::before {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: inherit;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.1) 0%,
    transparent 35%,
    transparent 65%,
    rgba(130, 170, 255, 0.04) 100%
  );
  pointer-events: none;
  z-index: 0;
}

.brand-mark::after {
  content: "";
  position: absolute;
  top: -35%;
  left: -90%;
  width: 55%;
  height: 170%;
  transform: rotate(18deg);
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(255, 255, 255, 0.05) 35%,
    rgba(255, 255, 255, 0.28) 50%,
    rgba(160, 190, 255, 0.12) 58%,
    transparent 100%
  );
  pointer-events: none;
  z-index: 1;
  animation: softShine 4.8s ease-in-out infinite;
}

.brand-mark:hover {
  transform: translateY(-1px);
  border-color: rgba(120, 160, 255, 0.22);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.1),
    0 8px 22px rgba(0, 0, 0, 0.28),
    0 0 22px rgba(90, 140, 255, 0.12);
  filter: saturate(1.03);
}

@keyframes softShine {
  0% {
    left: -90%;
    opacity: 0;
  }
  8% {
    opacity: 1;
  }
  38% {
    left: 135%;
    opacity: 1;
  }
  55%,
  100% {
    left: 135%;
    opacity: 0;
  }
}

.brand-mark-text {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 13px;
  line-height: 1;
  font-weight: 800;
  letter-spacing: 0.2px;

  color: rgba(255, 255, 255, 0.94);
  text-shadow: 0 1px 6px rgba(0, 0, 0, 0.24);

  opacity: 1;
  transform: translateY(0) scale(1);
}

.brand-mark-text.show-ak {
  animation: softSwap 0.55s cubic-bezier(0.22, 1, 0.36, 1);
}

.brand-mark-text:not(.show-ak) {
  animation: softHello 0.55s cubic-bezier(0.22, 1, 0.36, 1);
}

@keyframes softHello {
  from {
    opacity: 0;
    transform: translateY(-5px) scale(0.92);
    filter: blur(2px);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
    filter: blur(0);
  }
}

@keyframes softSwap {
  0% {
    opacity: 0;
    transform: translateY(4px) scale(0.9);
    filter: blur(3px);
  }
  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
    filter: blur(0);
  }
}

.brand-copy {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 3px;
  min-width: 0;
  width: 190px;
}

.brand-copy strong {
  font-size: 16px;
  line-height: 1.1;
  white-space: nowrap;
  color: var(--text-main);
}

.brand-role-marquee {
  position: relative;
  width: 100%;
  overflow: hidden;
  height: 16px;
  mask-image: linear-gradient(
    to right,
    transparent,
    black 10%,
    black 90%,
    transparent
  );
  -webkit-mask-image: linear-gradient(
    to right,
    transparent,
    black 10%,
    black 90%,
    transparent
  );
}

.brand-role-track {
  width: max-content;
  display: flex;
  align-items: center;
  gap: 34px;
  animation: brandMarquee 14s linear infinite;
}

.brand-role-track span {
  color: var(--text-muted);
  font-size: 12px;
  white-space: nowrap;
  line-height: 1;
}

.nav-links {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 24px;
  flex-wrap: nowrap;
  direction: ltr;
}

.nav-links a {
  position: relative;
  color: var(--text-soft);
  font-weight: 600;
  font-size: 14px;
  transition: 0.3s ease;
  white-space: nowrap;
}

.nav-links a:hover {
  color: var(--text-main);
}

.nav-links a::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -8px;
  width: 0;
  height: 2px;
  border-radius: 999px;
  background: linear-gradient(90deg, var(--primary), var(--primary-2));
  transition: width 0.3s ease;
}

.nav-links a:hover::after {
  width: 100%;
}

.nav-actions {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 10px;
  flex-wrap: nowrap;
  direction: ltr;
}

/* ================================================================= */
/*  Switch Buttons                                                   */
/* ================================================================= */
.switch-btn {
  position: relative;
  width: 150px;
  height: 48px;
  border: 1px solid var(--border);
  background: transparent;
  border-radius: 18px;
  padding: 0;
  overflow: hidden;
  cursor: pointer;
  transition:
    transform 0.32s ease,
    border-color 0.32s ease,
    box-shadow 0.32s ease;
  box-shadow: var(--shadow);
}

.switch-btn:hover {
  transform: translateY(-3px);
  border-color: var(--border-strong);
}

.switch-btn:active {
  transform: translateY(-1px) scale(0.985);
}

.switch-bg {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.1),
      rgba(255, 255, 255, 0.02)
    ),
    var(--surface);
  backdrop-filter: blur(14px);
}

.switch-inner {
  position: relative;
  z-index: 2;
  height: 100%;
  padding: 10px 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.switch-icon-wrap {
  width: 30px;
  height: 30px;
  border-radius: 10px;
  display: grid;
  place-items: center;
  background: rgba(255, 255, 255, 0.12);
  flex-shrink: 0;
  overflow: hidden;
}

.switch-icon {
  display: inline-block;
  animation: switchPop 0.28s ease;
}

.switch-text-wrap {
  text-align: center;
}

.switch-text {
  display: inline-block;
  color: var(--text-main);
  font-weight: 800;
  font-size: 13px;
  animation: textSlide 0.28s ease;
  white-space: nowrap;
}

/* ================================================================= */
/*  Hamburger                                                        */
/* ================================================================= */
.hamburger-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  width: 30px;
  height: 24px;
  position: relative;
  flex-direction: column;
  justify-content: space-between;
  padding: 0;
  margin-left: 10px;
}

.hamburger-btn span {
  display: block;
  width: 100%;
  height: 3px;
  background: var(--text-main);
  border-radius: 3px;
  transition: all 0.3s ease;
  transform-origin: center;
}

.hamburger-btn.active span:nth-child(1) {
  transform: translateY(10px) rotate(45deg);
}

.hamburger-btn.active span:nth-child(2) {
  opacity: 0;
  transform: scaleX(0);
}

.hamburger-btn.active span:nth-child(3) {
  transform: translateY(-10px) rotate(-45deg);
}

/* ================================================================= */
/*  Mobile Menu                                                      */
/* ================================================================= */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  z-index: 70;
  background: rgba(0, 0, 0, 0.75);
  backdrop-filter: blur(12px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.mobile-menu-panel {
  background: rgba(10, 20, 40, 0.95);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 24px;
  padding: 32px;
  display: flex;
  flex-direction: column;
  gap: 24px;
  align-items: center;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.6);
  max-width: 340px;
  width: calc(100% - 40px);
}

.mobile-nav-links {
  display: flex;
  flex-direction: column;
  gap: 16px;
  align-items: center;
}

.mobile-nav-links a {
  color: #fff;
  font-size: 18px;
  font-weight: 700;
  transition: opacity 0.2s;
}

.mobile-nav-links a:hover {
  opacity: 0.7;
}

.mobile-actions {
  display: flex;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
}

.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: opacity 0.3s ease;
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
}

/* ================================================================= */
/*  Hero                                                             */
/* ================================================================= */
.hero {
  position: relative;
  padding-top: clamp(28px, 6vh, 64px);
  padding-bottom: clamp(24px, 4vh, 42px);
}

.hero-overlay-line {
  position: absolute;
  inset-inline: 0;
  top: 50%;
  height: 1px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.08),
    transparent
  );
  pointer-events: none;
}

.hero-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.08fr) minmax(380px, 0.92fr);
  gap: 42px;
  align-items: center;
  width: 100%;
}

.hero-chip {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 11px 16px;
  border-radius: 999px;
  background: var(--surface);
  border: 1px solid var(--border-strong);
  color: var(--text-main);
  font-size: 14px;
  font-weight: 600;
  backdrop-filter: blur(14px);
  line-height: 1.3;
  margin-bottom: 26px;
}

.live-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--accent);
  animation: pulse 2s infinite;
}

.hero-title {
  margin: 0 0 18px;
  font-size: clamp(44px, 6vw, 92px);
  line-height: 1.02;
  color: var(--text-main);
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  align-items: baseline;
  font-weight: 800;
  letter-spacing: -0.02em;
}

.hero-title-muted {
  color: var(--text-main);
  opacity: 0.92;
}

.hero-name {
  position: relative;
  display: inline-block;
  white-space: nowrap;
  font-weight: 900;
  letter-spacing: -0.03em;
  color: #ffffff;
  background: linear-gradient(
    135deg,
    #ffffff 0%,
    #e0f0ff 20%,
    #9be7ff 38%,
    #a78bfa 60%,
    #c4b5fd 78%,
    #ffffff 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: #44aff7;
  text-shadow:
    0 0 8px rgba(155, 231, 255, 0.3),
    0 0 4px rgba(167, 139, 250, 0.2);
}

.app.light .hero-name {
  color: #0f172a;
  background: linear-gradient(
    135deg,
    #0f172a 0%,
    #0ea5e9 28%,
    #6366f1 58%,
    #8b5cf6 82%,
    #0f172a 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: #167ec3;
  text-shadow:
    0 0 6px rgba(14, 165, 233, 0.2),
    0 0 4px rgba(139, 92, 246, 0.15);
}

.hero-subtitle {
  margin: 0 0 20px;
  min-height: 52px;
  font-size: clamp(24px, 3vw, 40px);
  color: var(--text-main);
  font-weight: 600;
}

.typing-word {
  color: var(--primary);
  font-weight: 900;
}

.cursor {
  animation: blink 1s infinite;
}

.hero-desc {
  max-width: 760px;
  color: var(--text-soft);
  margin: 0;
  font-size: 18px;
  font-weight: 400;
  line-height: 1.85;
}

.hero-actions {
  display: flex;
  gap: 14px;
  flex-wrap: wrap;
  margin-top: 30px;
}

.floating-techs {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  margin-top: 32px;
}

.floating-techs span,
.skill-cloud span {
  padding: 10px 15px;
  border-radius: 999px;
  background: var(--surface);
  border: 1px solid var(--border);
  color: var(--text-main);
  font-size: 13px;
  font-weight: 600;
  backdrop-filter: blur(10px);
}

/* ================================================================= */
/*  Buttons                                                          */
/* ================================================================= */
.premium-btn {
  position: relative;
  overflow: hidden;
  min-width: 182px;
  min-height: 56px;
  border-radius: 18px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0 24px;
  border: 1px solid transparent;
  transition:
    transform 0.35s ease,
    box-shadow 0.35s ease,
    border-color 0.35s ease;
}

.premium-btn:hover {
  transform: translateY(-4px);
}

.premium-btn:active {
  transform: translateY(-1px) scale(0.985);
}

.premium-btn.primary {
  background: linear-gradient(135deg, var(--primary), var(--primary-2));
  color: white;
  box-shadow: 0 16px 36px rgba(97, 116, 255, 0.24);
}

.premium-btn.ghost {
  background: var(--btn-ghost);
  color: var(--text-main);
  border-color: var(--border);
  backdrop-filter: blur(14px);
}

.btn-liquid {
  position: absolute;
  inset: -40%;
  background:
    radial-gradient(
      circle at 20% 40%,
      rgba(255, 255, 255, 0.28),
      transparent 18%
    ),
    radial-gradient(
      circle at 70% 30%,
      rgba(255, 255, 255, 0.16),
      transparent 16%
    ),
    radial-gradient(
      circle at 60% 70%,
      rgba(255, 255, 255, 0.12),
      transparent 20%
    );
  transform: translateX(-12%);
  transition: transform 0.65s ease;
  opacity: 0.95;
}

.premium-btn:hover .btn-liquid {
  transform: translateX(10%);
}

.btn-content {
  position: relative;
  z-index: 2;
  font-weight: 700;
  letter-spacing: 0.01em;
  font-size: 15px;
}

/* ================================================================= */
/*  Stage                                                            */
/* ================================================================= */
.glass-stage {
  position: relative;
  min-height: 560px;
  display: grid;
  place-items: center;
}

.stage-ring {
  position: absolute;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.08);
  animation: spinSlow 18s linear infinite;
}

.stage-ring-1 {
  width: 440px;
  height: 440px;
  border-top-color: var(--primary);
  border-bottom-color: var(--primary-2);
}

.stage-ring-2 {
  width: 320px;
  height: 320px;
  animation-direction: reverse;
  animation-duration: 14s;
  border-top-color: var(--accent);
  border-bottom-color: rgba(255, 255, 255, 0.18);
}

/* اصلاح شده: حذف شیشه‌ای مستقیم، اضافه شدن glass-bg و force-sharp */
.stage-core {
  position: relative;
  z-index: 2;
  width: min(100%, 420px);
  padding: 34px 26px;
  border-radius: 34px;
  text-align: center;
  background: transparent;
  border: 1px solid var(--border);
  backdrop-filter: none;
  box-shadow: var(--shadow);
  isolation: isolate;
  -webkit-font-smoothing: subpixel-antialiased;
}

.stage-core .glass-bg {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  z-index: -1;
  background:
    linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.08),
      rgba(255, 255, 255, 0.02)
    ),
    var(--surface-soft);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
}

/* کلاس کمکی برای وضوح کامل متن */
.force-sharp {
  position: relative;
  z-index: 1;
  transform: translateZ(0);
  backface-visibility: hidden;
  -webkit-font-smoothing: antialiased;
}

/* ================================================================= */
/*  Premium Monitor                                                  */
/* ================================================================= */
.dev-core-container {
  position: relative;
  width: 100%;
  height: 370px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 10px;
  perspective: 800px;
}

.code-orbit {
  position: absolute;
  width: 100%;
  height: 100%;
  animation: rotateOrbit 20s linear infinite;
  z-index: 1;
}

.orbit-snippet {
  position: absolute;
  font-family: "Fira Code", monospace;
  font-size: 12px;
  color: var(--primary);
  white-space: nowrap;
  opacity: 0;
  text-shadow: 0 0 12px var(--primary);
  animation: fadeInOut 6s ease-in-out infinite;
}

.snippet-1 {
  top: 2%;
  left: 50%;
  transform: translateX(-50%);
  animation-delay: 0s;
}

.snippet-2 {
  top: 20%;
  right: 2%;
  animation-delay: 1.5s;
}

.snippet-3 {
  bottom: 15%;
  left: 50%;
  transform: translateX(-50%);
  animation-delay: 3s;
}

.snippet-4 {
  top: 35%;
  left: 2%;
  animation-delay: 4.5s;
}

.monitor-wrapper {
  position: relative;
  z-index: 5;
  transform: rotateX(2deg) rotateY(-1deg);
  transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
  filter: drop-shadow(0 20px 30px rgba(0, 0, 0, 0.5));
}

.monitor-wrapper:hover {
  transform: rotateX(0deg) rotateY(0deg) scale(1.03) translateY(-4px);
}

.monitor {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.monitor-screen {
  width: 230px;
  height: 155px;
  background: linear-gradient(135deg, #0f111a 0%, #1a1d2e 100%);
  border-radius: 18px;
  border: 1.5px solid rgba(255, 255, 255, 0.12);
  box-shadow:
    0 0 50px rgba(104, 220, 255, 0.12),
    inset 0 0 40px rgba(127, 103, 255, 0.08);
  position: relative;
  overflow: hidden;
}

.screen-glare {
  position: absolute;
  top: -30%;
  left: -20%;
  width: 140%;
  height: 120%;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.06) 0%,
    transparent 60%
  );
  z-index: 3;
  pointer-events: none;
}

.mini-terminal {
  position: absolute;
  inset: 10px;
  background: rgba(13, 17, 23, 0.95);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  z-index: 2;
}

.terminal-header {
  height: 22px;
  background: linear-gradient(180deg, #1a1e2b, #141822);
  display: flex;
  align-items: center;
  padding: 0 10px;
  gap: 6px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.dot.red {
  background: #ff5f56;
}

.dot.yellow {
  background: #ffbd2e;
}

.dot.green {
  background: #27c93f;
}

.title {
  margin-left: 10px;
  font-family: "Fira Code", monospace;
  font-size: 9px;
  color: #9aa0b0;
  letter-spacing: 0.3px;
}

.terminal-body {
  flex: 1;
  padding: 10px 12px;
  font-family: "Fira Code", monospace;
  font-size: 9px;
  line-height: 1.8;
  color: #c0caf5;
}

.code-line {
  white-space: nowrap;
  margin-bottom: 4px;
}

.code-keyword {
  color: #bb9af7;
  font-weight: 600;
}

.code-var {
  color: #7dcfff;
}

.code-string {
  color: #9ece6a;
}

.code-func {
  color: #e0af68;
}

.cursor-line {
  margin-top: 6px;
}

.blinking-cursor {
  font-size: 11px;
  color: var(--primary);
  animation: blink 1s infinite;
  font-weight: 100;
}

.monitor-stand {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: -2px;
}

.stand-neck {
  width: 36px;
  height: 22px;
  background: linear-gradient(180deg, #2d3242, #1a1e2a);
  border-radius: 4px 4px 0 0;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.6);
}

.stand-base {
  width: 80px;
  height: 10px;
  background: linear-gradient(180deg, #353b4d, #1e2230);
  border-radius: 0 0 12px 12px;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.7);
}

.neon-ring {
  position: absolute;
  border-radius: 50%;
  border: 2px solid transparent;
  animation: rotateRing 8s linear infinite;
  pointer-events: none;
  z-index: 2;
}

.ring-1 {
  width: 240px;
  height: 240px;
  border-top-color: var(--primary);
  border-bottom-color: var(--primary-2);
  animation-duration: 6s;
}

.ring-2 {
  width: 270px;
  height: 270px;
  border-left-color: var(--accent);
  border-right-color: var(--primary-2);
  animation-duration: 10s;
  animation-direction: reverse;
}

.ring-3 {
  width: 300px;
  height: 300px;
  border-top-color: rgba(255, 255, 255, 0.25);
  border-bottom-color: var(--primary);
  animation-duration: 14s;
}

.particles {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 3;
}

.particle {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 4px;
  height: 4px;
  background: var(--primary);
  border-radius: 50%;
  box-shadow: 0 0 14px var(--primary);
  animation: particleFly 3s ease-out infinite;
  animation-delay: calc(var(--i) * 0.4s);
  opacity: 0;
}

/* ================================================================= */
/*  Identity                                                         */
/* ================================================================= */
.identity h3 {
  margin: 0 0 8px;
  font-size: 30px;
  color: var(--text-main);
}

.identity p {
  margin: 0 0 18px;
  color: var(--text-soft);
  line-height: 1.8;
}

.skill-cloud {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
}

/* ================================================================= */
/*  Scroll Indicator                                                 */
/* ================================================================= */
.scroll-indicator {
  position: absolute;
  left: 50%;
  bottom: 24px;
  transform: translateX(-50%);
  width: 36px;
  height: 58px;
  border: 2px solid var(--border-strong);
  border-radius: 999px;
  display: flex;
  justify-content: center;
  padding-top: 10px;
}

.scroll-indicator span {
  width: 6px;
  height: 14px;
  border-radius: 999px;
  background: linear-gradient(180deg, var(--primary), var(--primary-2));
  animation: scrollDot 1.8s ease-in-out infinite;
}

/* ==========================================================
   Skills Grid
========================================================== */

.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0;

  overflow: hidden;

  border-radius: 32px;
  border: 1px solid var(--border);

  background: var(--surface);
}

/* حذف بردرهای اضافه */

.skill-panel:nth-child(3n) {
  border-inline-end: none;
}

.skill-panel:nth-last-child(-n + 3) {
  border-bottom: none;
}

/* ==========================================================
   Skill Card
========================================================== */

.skill-panel {
  position: relative;

  padding: 30px;
  min-height: 220px;

  background: transparent;

  border-inline-end: 1px solid var(--border);
  border-bottom: 1px solid var(--border);

  overflow: hidden;

  transition:
    transform 0.25s cubic-bezier(0.2, 0.8, 0.2, 1),
    background-color 0.25s ease,
    border-color 0.25s ease,
    box-shadow 0.25s ease;
}

.skill-panel:hover {
  transform: translateY(-4px);

  background: var(--surface-strong);

  border-color: var(--border);

  box-shadow: var(--shadow);
}

/* ==========================================================
   Top Accent Line
========================================================== */

.panel-line {
  width: 56px;
  height: 3px;

  margin-bottom: 20px;

  border-radius: 999px;

  background: linear-gradient(90deg, var(--primary), var(--primary-2));

  transition: width 0.25s ease;
}

.skill-panel:hover .panel-line {
  width: 74px;
}

/* ==========================================================
   Header
========================================================== */

.skill-head {
  display: flex;
  align-items: center;
  gap: 14px;

  margin-bottom: 14px;
}

/* ==========================================================
   Icon
========================================================== */

.skill-icon {
  width: 52px;
  height: 52px;

  display: flex;
  align-items: center;
  justify-content: center;

  flex-shrink: 0;

  border-radius: 16px;

  background: var(--surface-strong);

  border: 1px solid var(--border);

  color: var(--text-main);

  font-size: 24px;

  transition:
    transform 0.25s cubic-bezier(0.2, 0.8, 0.2, 1),
    color 0.25s ease,
    background-color 0.25s ease;
}

.skill-panel:hover .skill-icon {
  transform: scale(1.08) rotate(-3deg);

  color: var(--primary);

  background: var(--surface);

  /* هیچ بردر یا Glow اضافه‌ای ندارد */
}

/* ==========================================================
   Title
========================================================== */

.skill-head h3 {
  margin: 0;

  font-size: 24px;

  font-weight: 700;

  color: var(--text-main);

  transition:
    color 0.25s ease,
    transform 0.25s ease;
}

.skill-panel:hover h3 {
  color: var(--primary);

  transform: translateX(2px);
}

/* ==========================================================
   Paragraph
========================================================== */

.skill-panel p {
  margin: 0;

  color: var(--text-soft);

  line-height: 1.8;

  transition: color 0.25s ease;
}

.skill-panel:hover p {
  color: var(--text-main);
}

/* ==========================================================
   Responsive
========================================================== */

@media (max-width: 992px) {
  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .skill-panel:nth-child(3n) {
    border-inline-end: 1px solid var(--border);
  }

  .skill-panel:nth-child(2n) {
    border-inline-end: none;
  }

  .skill-panel:nth-last-child(-n + 3) {
    border-bottom: 1px solid var(--border);
  }

  .skill-panel:nth-last-child(-n + 2) {
    border-bottom: none;
  }
}

@media (max-width: 768px) {
  .skills-grid {
    grid-template-columns: 1fr;
  }

  .skill-panel {
    border-inline-end: none;
  }

  .skill-panel:last-child {
    border-bottom: none;
  }
}
/* ================================================================= */
/*  About                                                            */
/* ================================================================= */
.about-grid {
  display: grid;
  grid-template-columns: 1.08fr 0.92fr;
  gap: 24px;
  align-items: start;
}

.about-main p {
  margin-bottom: 16px;
  font-weight: 400;
  line-height: 1.8;
}

.about-points {
  margin-top: 26px;
  display: grid;
  gap: 14px;
}

.about-point {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 16px;
  border-radius: 18px;
  background: var(--surface);
  border: 1px solid var(--border);
  backdrop-filter: blur(12px);
}

.about-point span {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: rgba(104, 220, 255, 0.12);
  color: var(--primary);
  font-weight: 900;
  flex-shrink: 0;
}

.about-point p {
  margin: 0;
}

.about-side {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 18px;
}

.about-card {
  padding: 24px;
  border-radius: 26px;
  background: var(--surface);
  border: 1px solid var(--border);
  backdrop-filter: blur(14px);
  transition:
    transform 0.35s ease,
    border-color 0.35s ease;
}

.about-card:hover {
  transform: translateY(-8px);
  border-color: var(--border-strong);
}

.about-card.accent {
  background: linear-gradient(
    135deg,
    rgba(104, 220, 255, 0.1),
    rgba(127, 103, 255, 0.12)
  );
}

.about-card h3 {
  margin: 0 0 12px;
  font-size: 22px;
  color: var(--text-main);
  font-weight: 700;
}

.about-card p {
  margin: 0;
  color: var(--text-soft);
  font-weight: 400;
  line-height: 1.8;
}

/* ================================================================= */
/*  Projects                                                         */
/* ================================================================= */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 18px;
}

.project-panel {
  padding: 26px;
  border-radius: 28px;
  background: var(--surface);
  border: 1px solid var(--border);
  backdrop-filter: blur(14px);
  transition:
    transform 0.35s ease,
    border-color 0.35s ease,
    background 0.35s ease;
}

.project-panel:hover {
  transform: translateY(-8px);
  border-color: var(--border-strong);
  background: var(--surface-strong);
}

.project-head {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 18px;
}

.project-type {
  padding: 7px 12px;
  border-radius: 999px;
  background: rgba(104, 220, 255, 0.12);
  color: var(--primary);
  font-size: 12px;
  font-weight: 800;
}

.project-line {
  flex: 1;
  height: 1px;
  background: linear-gradient(
    90deg,
    rgba(104, 220, 255, 0.55),
    rgba(127, 103, 255, 0)
  );
}

.project-panel h3 {
  margin: 0 0 12px;
  color: var(--text-main);
  font-size: 24px;
}

.project-panel p {
  margin: 0;
  color: var(--text-soft);
  font-weight: 400;
  line-height: 1.8;
}

.project-tags {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 18px;
}

.project-tags span {
  padding: 8px 12px;
  border-radius: 999px;
  background: var(--surface-soft);
  border: 1px solid var(--border);
  color: var(--text-main);
  font-size: 12px;
  font-weight: 600;
}

/* ================================================================= */
/*  Contact                                                          */
/* ================================================================= */
.contact-shell {
  text-align: center;
  padding: 24px 0;
}

.contact-shell p {
  max-width: 860px;
  margin: 0 auto;
}

.contact-actions {
  margin-top: 30px;
  display: flex;
  justify-content: center;
  gap: 14px;
  flex-wrap: wrap;
}

/* ================================================================= */
/*  Keyframes                                                        */
/* ================================================================= */
@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(139, 255, 210, 0.55);
  }
  70% {
    box-shadow: 0 0 0 12px rgba(139, 255, 210, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(139, 255, 210, 0);
  }
}

@keyframes blink {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
    opacity: 0;
  }
}

@keyframes spinSlow {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes scrollDot {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  60% {
    transform: translateY(16px);
    opacity: 0.35;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes meshMove {
  0% {
    transform: translate3d(0, 0, 0) scale(1);
  }
  100% {
    transform: translate3d(0, -18px, 0) scale(1.04);
  }
}

@keyframes auroraFloat {
  0% {
    transform: translate(-8%, -4%) scale(1);
  }
  50% {
    transform: translate(8%, 5%) scale(1.08);
  }
  100% {
    transform: translate(-5%, 10%) scale(0.98);
  }
}

@keyframes switchPop {
  0% {
    opacity: 0;
    transform: scale(0.72) translateY(4px);
  }
  100% {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

@keyframes textSlide {
  0% {
    opacity: 0;
    transform: translateY(6px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes brandMarquee {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

@keyframes rotateOrbit {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes fadeInOut {
  0%,
  100% {
    opacity: 0;
    transform: translateY(6px) scale(0.95);
  }
  50% {
    opacity: 0.7;
    transform: translateY(0) scale(1);
  }
}

@keyframes rotateRing {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes particleFly {
  0% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 1;
  }
  100% {
    transform: translate(
        calc((var(--i) - 4) * 55px),
        calc((var(--i) - 4) * 55px)
      )
      scale(0);
    opacity: 0;
  }
}

/* ================================================================= */
/*  1440                                                             */
/* ================================================================= */
@media (max-width: 1440px) {
  .container {
    width: min(1220px, calc(100% - 40px));
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1.02fr) minmax(360px, 0.98fr);
    gap: 34px;
  }

  .glass-stage {
    min-height: 530px;
  }

  .stage-ring-1 {
    width: 400px;
    height: 400px;
  }

  .stage-ring-2 {
    width: 300px;
    height: 300px;
  }
}

/* ================================================================= */
/*  1280                                                             */
/* ================================================================= */
@media (max-width: 1280px) {
  :global(html) {
    scroll-padding-top: 92px;
  }

  .container {
    width: min(100% - 32px, 1180px);
  }

  .nav-shell {
    grid-template-columns: minmax(220px, 250px) 1fr minmax(220px, 250px);
    gap: 14px;
  }

  .nav-links {
    gap: 18px;
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1fr) minmax(340px, 0.92fr);
    gap: 28px;
  }

  .hero-title {
    font-size: clamp(42px, 5vw, 78px);
  }

  .hero-subtitle {
    font-size: clamp(22px, 2.7vw, 34px);
  }

  .hero-desc {
    font-size: 16px;
  }

  .glass-stage {
    min-height: 500px;
  }

  .stage-ring-1 {
    width: 360px;
    height: 360px;
  }

  .stage-ring-2 {
    width: 270px;
    height: 270px;
  }

  .stage-core {
    width: min(100%, 380px);
    padding: 28px 22px;
  }

  .dev-core-container {
    height: 330px;
  }

  .monitor-screen {
    width: 200px;
    height: 136px;
  }

  .ring-1 {
    width: 210px;
    height: 210px;
  }

  .ring-2 {
    width: 236px;
    height: 236px;
  }

  .ring-3 {
    width: 262px;
    height: 262px;
  }

  .projects-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .skills-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* ================================================================= */
/*  1100                                                             */
/* ================================================================= */
@media (max-width: 1100px) {
  :global(html) {
    scroll-padding-top: 84px;
  }

  .container {
    width: min(100% - 28px, 1060px);
  }

  .section {
    padding: 100px 0;
  }

  .nav-shell {
    grid-template-columns: minmax(180px, 220px) 1fr minmax(180px, 220px);
    gap: 12px;
  }

  .nav-links {
    gap: 14px;
  }

  .nav-links a {
    font-size: 13px;
  }

  .switch-btn {
    width: 132px;
    height: 46px;
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1fr) minmax(300px, 0.9fr);
    gap: 22px;
  }

  .hero-title {
    font-size: clamp(38px, 5vw, 64px);
  }

  .hero-subtitle {
    font-size: clamp(21px, 2.5vw, 30px);
  }

  .hero-desc {
    font-size: 15px;
    line-height: 1.85;
  }

  .floating-techs {
    margin-top: 26px;
    gap: 10px;
  }

  .floating-techs span,
  .skill-cloud span {
    padding: 9px 13px;
    font-size: 12px;
  }

  .glass-stage {
    min-height: 450px;
  }

  .stage-ring-1 {
    width: 310px;
    height: 310px;
  }

  .stage-ring-2 {
    width: 230px;
    height: 230px;
  }

  .stage-core {
    width: min(100%, 335px);
    padding: 24px 18px;
    border-radius: 28px;
  }

  .dev-core-container {
    height: 280px;
  }

  .monitor-screen {
    width: 170px;
    height: 112px;
  }

  .mini-terminal {
    inset: 7px;
  }

  .terminal-header {
    height: 18px;
    padding: 0 8px;
  }

  .title {
    font-size: 8px;
    margin-left: 8px;
  }

  .terminal-body {
    padding: 8px;
    font-size: 7px;
    line-height: 1.65;
  }

  .stand-neck {
    width: 26px;
    height: 15px;
  }

  .stand-base {
    width: 58px;
    height: 8px;
  }

  .ring-1 {
    width: 178px;
    height: 178px;
  }

  .ring-2 {
    width: 198px;
    height: 198px;
  }

  .ring-3 {
    width: 220px;
    height: 220px;
  }

  .orbit-snippet {
    font-size: 9px;
  }

  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .about-grid {
    grid-template-columns: 1fr;
  }

  .about-side {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* ================================================================= */
/*  920                                                              */
/* ================================================================= */
@media (max-width: 920px) {
  .navbar {
    backdrop-filter: blur(16px);
  }

  .app {
    padding-top: 72px;
  }

  .nav-shell {
    display: flex;
    align-items: center;
    justify-content: space-between;
    min-height: auto;
    padding: 12px 0;
  }

  .nav-col-start {
    flex: 1;
  }

  .nav-col-center {
    display: none;
  }

  .nav-col-end {
    flex-shrink: 0;
  }

  .nav-actions {
    display: none;
  }

  .hamburger-btn {
    display: flex;
  }

  .brand {
    max-width: 100%;
  }

  .brand-copy {
    width: auto;
    max-width: 180px;
  }

  .brand-copy strong {
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .section {
    padding: 90px 0;
  }

  .hero {
    padding-top: 28px;
    padding-bottom: 24px;
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1fr) minmax(260px, 320px);
    gap: 20px;
    align-items: center;
  }

  .hero-title {
    font-size: clamp(34px, 6vw, 52px);
    gap: 8px;
  }

  .hero-subtitle {
    font-size: 20px;
    min-height: 40px;
  }

  .hero-desc {
    font-size: 14.5px;
  }

  .premium-btn {
    min-width: 160px;
    min-height: 52px;
    padding: 0 18px;
  }

  .btn-content {
    font-size: 14px;
  }

  .glass-stage {
    min-height: 390px;
  }

  .stage-ring-1 {
    width: 260px;
    height: 260px;
  }

  .stage-ring-2 {
    width: 192px;
    height: 192px;
  }

  .stage-core {
    width: 100%;
    max-width: 300px;
    padding: 20px 14px;
    border-radius: 24px;
  }

  .dev-core-container {
    height: 235px;
  }

  .monitor-screen {
    width: 144px;
    height: 94px;
    border-radius: 14px;
  }

  .terminal-body {
    font-size: 6.2px;
    line-height: 1.6;
  }

  .ring-1 {
    width: 150px;
    height: 150px;
  }

  .ring-2 {
    width: 170px;
    height: 170px;
  }

  .ring-3 {
    width: 188px;
    height: 188px;
  }

  .identity h3 {
    font-size: 24px;
  }

  .identity p {
    font-size: 14px;
  }

  .scroll-indicator {
    display: none;
  }
}

/* ================================================================= */
/*  760                                                              */
/* ================================================================= */
@media (max-width: 760px) {
  .container {
    width: calc(100% - 22px);
  }

  .app {
    padding-top: 64px;
  }

  .section {
    padding: 80px 0;
  }

  .section-heading {
    margin-bottom: 42px;
  }

  .section-badge {
    padding: 9px 14px;
    font-size: 12px;
  }

  .section-heading h2,
  .about-main h2,
  .contact-shell h2 {
    font-size: clamp(28px, 7vw, 40px);
    line-height: 1.18;
  }

  .section-heading p,
  .about-main p,
  .contact-shell p {
    font-size: 15px;
    line-height: 1.85;
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1fr) 260px;
    gap: 16px;
  }

  .hero-chip {
    font-size: 12px;
    padding: 9px 13px;
    margin-bottom: 18px;
  }

  .hero-title {
    font-size: clamp(30px, 7vw, 42px);
    line-height: 1.08;
  }

  .hero-subtitle {
    font-size: 18px;
    margin-bottom: 14px;
  }

  .hero-desc {
    font-size: 14px;
    line-height: 1.9;
  }

  .hero-actions {
    gap: 10px;
    margin-top: 24px;
  }

  .floating-techs,
  .skill-cloud {
    gap: 8px;
  }

  .floating-techs span,
  .skill-cloud span {
    padding: 8px 11px;
    font-size: 11.5px;
  }

  .glass-stage {
    min-height: 350px;
  }

  .stage-ring-1 {
    width: 225px;
    height: 225px;
  }

  .stage-ring-2 {
    width: 168px;
    height: 168px;
  }

  .stage-core {
    max-width: 265px;
    padding: 18px 12px;
    border-radius: 22px;
  }

  .dev-core-container {
    height: 205px;
  }

  .monitor-wrapper {
    transform: none;
  }

  .monitor-wrapper:hover {
    transform: scale(1.02);
  }

  .monitor-screen {
    width: 126px;
    height: 82px;
  }

  .mini-terminal {
    inset: 6px;
  }

  .terminal-header {
    height: 16px;
    padding: 0 6px;
    gap: 4px;
  }

  .title {
    font-size: 7px;
    margin-left: 6px;
  }

  .terminal-body {
    padding: 6px;
    font-size: 5.6px;
    line-height: 1.55;
  }

  .dot {
    width: 6px;
    height: 6px;
  }

  .stand-neck {
    width: 20px;
    height: 12px;
  }

  .stand-base {
    width: 46px;
    height: 7px;
  }

  .ring-1 {
    width: 132px;
    height: 132px;
  }

  .ring-2 {
    width: 148px;
    height: 148px;
  }

  .ring-3 {
    width: 164px;
    height: 164px;
  }

  .orbit-snippet {
    font-size: 6.5px;
  }

  .skills-grid {
    grid-template-columns: 1fr;
  }

  .projects-grid {
    grid-template-columns: 1fr;
  }

  .about-side {
    grid-template-columns: 1fr;
  }

  .skill-panel,
  .project-panel,
  .about-card {
    padding: 20px;
    border-radius: 22px;
  }

  .skill-head h3,
  .project-panel h3,
  .about-card h3 {
    font-size: 20px;
  }

  .skill-panel p,
  .project-panel p,
  .about-card p {
    font-size: 14px;
  }
}

/* ================================================================= */
/*  640                                                              */
/* ================================================================= */
@media (max-width: 640px) {
  .container {
    width: calc(100% - 18px);
  }

  .brand-mark {
    width: 34px;
    height: 34px;
    border-radius: 10px;
  }

  .brand-mark-text {
    font-size: 12px;
  }

  .brand-copy strong {
    font-size: 14px;
  }

  .brand-role-track span {
    font-size: 11px;
  }

  .hero-grid {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  .hero-left {
    text-align: center;
    order: 1;
  }

  .hero-right {
    order: 2;
  }

  .hero-chip {
    margin-inline: auto;
    justify-content: center;
  }

  .hero-title {
    justify-content: center;
  }

  .hero-subtitle {
    min-height: auto;
    text-align: center;
  }

  .hero-desc {
    margin-inline: auto;
    text-align: center;
    max-width: 560px;
  }

  .hero-actions {
    justify-content: center;
  }

  .floating-techs {
    justify-content: center;
  }

  .glass-stage {
    min-height: 390px;
  }

  .stage-ring-1 {
    width: 250px;
    height: 250px;
  }

  .stage-ring-2 {
    width: 185px;
    height: 185px;
  }

  .stage-core {
    max-width: 300px;
    padding: 20px 14px;
  }

  .dev-core-container {
    height: 240px;
  }

  .monitor-screen {
    width: 140px;
    height: 92px;
  }

  .ring-1 {
    width: 150px;
    height: 150px;
  }

  .ring-2 {
    width: 170px;
    height: 170px;
  }

  .ring-3 {
    width: 190px;
    height: 190px;
  }

  .identity h3 {
    font-size: 22px;
  }

  .identity p {
    font-size: 14px;
  }

  .premium-btn,
  .switch-btn {
    width: 100%;
    max-width: 280px;
  }

  .hero-actions,
  .contact-actions {
    flex-direction: column;
    align-items: center;
  }

  .mobile-menu-panel {
    padding: 26px 20px;
    border-radius: 20px;
  }
}

/* ================================================================= */
/*  520                                                              */
/* ================================================================= */
@media (max-width: 520px) {
  :global(html) {
    scroll-padding-top: 68px;
  }

  .container {
    width: calc(100% - 16px);
  }

  .nav-shell {
    padding: 10px 0;
  }

  .brand {
    gap: 10px;
  }

  .brand-mark {
    width: 32px;
    height: 32px;
    border-radius: 9px;
  }

  .brand-mark-text {
    font-size: 11px;
  }

  .brand-copy {
    max-width: 135px;
  }

  .brand-copy strong {
    font-size: 13px;
  }

  .brand-role-marquee {
    height: 14px;
  }

  .brand-role-track {
    gap: 24px;
  }

  .brand-role-track span {
    font-size: 10px;
  }

  .hamburger-btn {
    width: 26px;
    height: 20px;
    margin-left: 6px;
  }

  .hamburger-btn span {
    height: 2.5px;
  }

  .section {
    padding: 68px 0;
  }

  .section-heading {
    margin-bottom: 34px;
  }

  .section-heading h2,
  .about-main h2,
  .contact-shell h2 {
    font-size: 26px;
  }

  .section-heading p,
  .about-main p,
  .contact-shell p {
    font-size: 14px;
  }

  .hero-chip {
    font-size: 11px;
    padding: 8px 12px;
    gap: 8px;
  }

  .live-dot {
    width: 8px;
    height: 8px;
  }

  .hero-title {
    font-size: 28px;
    gap: 6px;
  }

  .hero-subtitle {
    font-size: 18px;
  }

  .hero-desc {
    font-size: 13.5px;
  }

  .floating-techs span,
  .skill-cloud span,
  .project-tags span {
    font-size: 11px;
    padding: 7px 10px;
  }

  .premium-btn {
    min-height: 50px;
    border-radius: 15px;
  }

  .btn-content {
    font-size: 13px;
  }

  .glass-stage {
    min-height: 350px;
  }

  .stage-ring-1 {
    width: 220px;
    height: 220px;
  }

  .stage-ring-2 {
    width: 160px;
    height: 160px;
  }

  .stage-core {
    max-width: 275px;
    padding: 18px 12px;
    border-radius: 20px;
  }

  .dev-core-container {
    height: 210px;
  }

  .monitor-screen {
    width: 126px;
    height: 82px;
  }

  .mini-terminal {
    inset: 5px;
    border-radius: 8px;
  }

  .terminal-body {
    padding: 6px;
    font-size: 5.5px;
    line-height: 1.55;
  }

  .stand-neck {
    width: 18px;
    height: 11px;
  }

  .stand-base {
    width: 42px;
    height: 7px;
  }

  .ring-1 {
    width: 136px;
    height: 136px;
  }

  .ring-2 {
    width: 152px;
    height: 152px;
  }

  .ring-3 {
    width: 168px;
    height: 168px;
  }

  .particle {
    width: 3px;
    height: 3px;
  }

  .identity h3 {
    font-size: 20px;
  }

  .identity p {
    font-size: 13px;
  }

  .skills-grid,
  .projects-grid {
    border-radius: 22px;
  }

  .skill-panel,
  .project-panel,
  .about-card {
    padding: 18px;
    border-radius: 18px;
  }

  .skill-head {
    gap: 10px;
  }

  .skill-head h3,
  .project-panel h3,
  .about-card h3 {
    font-size: 18px;
  }

  .skill-panel p,
  .project-panel p,
  .about-card p,
  .about-point p {
    font-size: 13px;
    line-height: 1.8;
  }

  .project-head {
    margin-bottom: 14px;
  }

  .project-type {
    font-size: 11px;
    padding: 6px 10px;
  }

  .about-point {
    gap: 10px;
    padding: 12px;
    align-items: flex-start;
  }

  .about-point span {
    width: 26px;
    height: 26px;
    font-size: 12px;
  }

  .mobile-menu-panel {
    width: calc(100% - 24px);
    padding: 22px 16px;
    gap: 18px;
  }

  .mobile-nav-links a {
    font-size: 16px;
  }
}

/* ================================================================= */
/*  400                                                              */
/* ================================================================= */
@media (max-width: 400px) {
  .container {
    width: calc(100% - 14px);
  }

  .brand-copy {
    max-width: 110px;
  }

  .brand-copy strong {
    font-size: 12px;
  }

  .hero-title {
    font-size: 24px;
  }

  .hero-subtitle {
    font-size: 16px;
  }

  .hero-desc {
    font-size: 13px;
  }

  .hero-chip {
    font-size: 10px;
    padding: 7px 10px;
  }

  .section-heading h2,
  .about-main h2,
  .contact-shell h2 {
    font-size: 23px;
  }

  .glass-stage {
    min-height: 320px;
  }

  .stage-core {
    max-width: 245px;
    padding: 16px 10px;
  }

  .dev-core-container {
    height: 180px;
  }

  .monitor-screen {
    width: 108px;
    height: 70px;
  }

  .terminal-body {
    font-size: 4.8px;
  }

  .ring-1 {
    width: 116px;
    height: 116px;
  }

  .ring-2 {
    width: 130px;
    height: 130px;
  }

  .ring-3 {
    width: 144px;
    height: 144px;
  }

  .premium-btn {
    min-height: 47px;
  }

  .btn-content,
  .switch-text {
    font-size: 12px;
  }

  .switch-icon-wrap {
    width: 26px;
    height: 26px;
  }
}

/* ================================================================= */
/*  Reduced Motion                                                   */
/* ================================================================= */
@media (prefers-reduced-motion: reduce) {
  :global(html) {
    scroll-behavior: auto;
  }

  *,
  *::before,
  *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
  }
}
</style>
