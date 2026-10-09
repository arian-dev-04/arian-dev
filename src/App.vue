<template>
  <div
    class="app"
    :class="[
      theme,
      isFa ? 'lang-fa' : 'lang-en',
      { 'is-loading': isInitialLoading },
    ]"
    :dir="isFa ? 'rtl' : 'ltr'"
    :lang="isFa ? 'fa' : 'en'"
  >
    <!-- =========================================================
         INITIAL LOADER (lightweight: CSS rings + one lazy Lottie)
    ========================================================== -->
    <Transition name="initial-loader" @after-leave="destroyLoaderLottie">
      <div
        v-if="isInitialLoading"
        class="initial-loader"
        role="status"
        aria-live="polite"
        :aria-label="isFa ? 'در حال آماده‌سازی سایت' : 'Initializing website'"
      >
        <div class="initial-loader-grid" aria-hidden="true"></div>

        <div class="initial-loader-center">
          <div class="initial-loader-animation" aria-hidden="true">
            <span class="initial-loader-ring"></span>
            <span class="initial-loader-ring initial-loader-ring-2"></span>
            <span class="initial-loader-ring initial-loader-ring-3"></span>
            <span class="initial-loader-ring initial-loader-ring-4"></span>
            <div
              ref="loaderLottieContainer"
              class="initial-loader-lottie"
            ></div>
          </div>

          <h2>{{ loaderTitle }}</h2>

          <div class="initial-loader-progress-row">
            <div
              ref="loaderProgressEl"
              class="initial-loader-progress"
              role="progressbar"
              aria-valuemin="0"
              aria-valuemax="100"
              aria-valuenow="0"
            >
              <span ref="loaderBarEl"></span>
            </div>
            <strong ref="loaderPercentEl" class="initial-loader-percent"
              >0%</strong
            >
          </div>
        </div>
      </div>
    </Transition>

    <a class="skip-link" href="#main-content">
      {{ isFa ? "پرش به محتوای اصلی" : "Skip to main content" }}
    </a>

    <div v-if="trailEnabled" class="mouse-trail-layer" aria-hidden="true">
      <span
        v-for="star in stars"
        :key="star.id"
        class="trail-star"
        :style="{
          left: `${star.x}px`,
          top: `${star.y}px`,
          width: `${star.size}px`,
          height: `${star.size}px`,
          background: star.color,
          '--glow': star.glow,
          '--trail-duration': `${star.duration}ms`,
          '--drift-x': `${star.driftX}px`,
          '--drift-y': `${star.driftY}px`,
          '--star-rotation': `${star.rotation}deg`,
        }"
      ></span>
    </div>

    <div class="bg-base" aria-hidden="true"></div>
    <div class="bg-orb bg-orb-1" aria-hidden="true"></div>
    <div class="bg-orb bg-orb-2" aria-hidden="true"></div>

    <!-- =========================================================
         NAVBAR
    ========================================================== -->
    <header class="navbar">
      <div class="container nav-shell">
        <div class="nav-col nav-col-start">
          <a href="#home" class="brand" @click="menuOpen = false">
            <div class="brand-mark" aria-hidden="true">
              <svg
                class="code-mark-svg"
                viewBox="0 0 64 64"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <defs>
                  <linearGradient
                    id="navCodeGradient"
                    gradientUnits="userSpaceOnUse"
                    x1="8"
                    y1="12"
                    x2="56"
                    y2="52"
                  >
                    <stop offset="0" style="stop-color: var(--primary)" />
                    <stop offset="1" style="stop-color: var(--primary-2)" />
                  </linearGradient>

                  <linearGradient id="navShine" x1="0" y1="0" x2="1" y2="0">
                    <stop offset="0" stop-color="#fff" stop-opacity="0" />
                    <stop offset="0.5" stop-color="#fff" stop-opacity="0.6" />
                    <stop offset="1" stop-color="#fff" stop-opacity="0" />
                  </linearGradient>

                  <radialGradient id="navAurora">
                    <stop
                      offset="0"
                      style="stop-color: var(--primary)"
                      stop-opacity="0.42"
                    />
                    <stop
                      offset="1"
                      style="stop-color: var(--primary)"
                      stop-opacity="0"
                    />
                  </radialGradient>
                </defs>

                <circle
                  class="code-mark-aurora"
                  cx="18"
                  cy="16"
                  r="24"
                  fill="url(#navAurora)"
                />

                <g class="code-mark-rain">
                  <line x1="12" y1="2" x2="12" y2="62" />
                  <line x1="32" y1="2" x2="32" y2="62" />
                  <line x1="52" y1="2" x2="52" y2="62" />
                </g>

                <circle class="code-mark-pulse" cx="32" cy="32" r="8" />

                <g class="code-mark-shift shift-left">
                  <path
                    class="code-mark-path code-mark-left"
                    d="M23 21 L10 32 L23 43"
                    pathLength="100"
                  />
                </g>

                <path
                  class="code-mark-path code-mark-slash"
                  d="M36 18 L28 46"
                  pathLength="100"
                />

                <g class="code-mark-shift shift-right">
                  <path
                    class="code-mark-path code-mark-right"
                    d="M41 21 L54 32 L41 43"
                    pathLength="100"
                  />
                </g>

                <rect
                  class="code-mark-cursor"
                  x="25"
                  y="52"
                  width="14"
                  height="3"
                  rx="1.5"
                />

                <g class="code-mark-led">
                  <circle class="led-ring" cx="53" cy="11" r="2.4" />
                  <circle class="led-dot" cx="53" cy="11" r="2.4" />
                </g>

                <circle class="code-mark-spark" cx="11" cy="53" r="1.3" />

                <g transform="rotate(24 32 32)">
                  <rect
                    class="code-mark-shine"
                    x="-26"
                    y="-14"
                    width="14"
                    height="92"
                    fill="url(#navShine)"
                  />
                </g>
              </svg>
            </div>

            <div class="brand-copy">
              <strong>{{ t.brand }}</strong>
              <span class="brand-role">{{ t.hero.role }}</span>
            </div>
          </a>
        </div>

        <div class="nav-col nav-col-center">
          <nav class="nav-links" aria-label="Primary navigation">
            <a v-for="item in NAV_ITEMS" :key="item" :href="`#${item}`">
              {{ t.nav[item] }}
            </a>
          </nav>
        </div>

        <div class="nav-col nav-col-end">
          <div class="nav-actions">
            <button
              class="switch-btn"
              type="button"
              :aria-label="isFa ? 'Switch to English' : 'تغییر به فارسی'"
              @click="toggleLang"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">🌐</span>
                <span class="switch-text" :key="lang">{{
                  isFa ? "English" : "فارسی"
                }}</span>
              </span>
            </button>

            <button
              class="switch-btn"
              type="button"
              :aria-label="
                theme === 'dark'
                  ? 'Switch to light mode'
                  : 'Switch to dark mode'
              "
              @click="toggleTheme"
            >
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">
                  {{ theme === "dark" ? "☀️" : "🌙" }}
                </span>
                <span class="switch-text" :key="theme">
                  {{ theme === "dark" ? t.actions.light : t.actions.dark }}
                </span>
              </span>
            </button>
          </div>

          <button
            class="hamburger-btn"
            :class="{ active: menuOpen }"
            type="button"
            :aria-expanded="menuOpen"
            aria-controls="mobile-navigation"
            aria-label="Toggle navigation"
            @click.stop="menuOpen = !menuOpen"
          >
            <span></span>
            <span></span>
            <span></span>
          </button>
        </div>
      </div>
    </header>

    <!-- =========================================================
         MOBILE MENU
    ========================================================== -->
    <Transition name="mobile-menu" @after-leave="releaseMobileLottie">
      <div
        v-if="menuOpen"
        class="mobile-menu-overlay"
        @click="menuOpen = false"
      >
        <div class="mobile-menu-panel" @click.stop>
          <div class="mobile-menu-header">
            <div class="mobile-menu-brand">
              <div class="mobile-menu-mark">
                <div
                  ref="mobileLottieContainer"
                  class="mobile-lottie"
                  aria-hidden="true"
                ></div>
              </div>

              <div>
                <strong>{{ t.brand }}</strong>
                <span>{{ t.hero.role }}</span>
              </div>
            </div>

            <button
              class="mobile-close"
              type="button"
              aria-label="Close menu"
              @click="menuOpen = false"
            >
              ×
            </button>
          </div>

          <nav
            id="mobile-navigation"
            class="mobile-nav-links"
            aria-label="Mobile navigation"
          >
            <a
              v-for="item in NAV_ITEMS"
              :key="item"
              :href="`#${item}`"
              @click="menuOpen = false"
            >
              {{ t.nav[item] }}
            </a>
          </nav>

          <div class="mobile-actions">
            <button class="switch-btn" type="button" @click="toggleLang">
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">🌐</span>
                <span class="switch-text">{{
                  isFa ? "English" : "فارسی"
                }}</span>
              </span>
            </button>

            <button class="switch-btn" type="button" @click="toggleTheme">
              <span class="switch-bg"></span>
              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">
                  {{ theme === "dark" ? "☀️" : "🌙" }}
                </span>
                <span class="switch-text">
                  {{ theme === "dark" ? t.actions.light : t.actions.dark }}
                </span>
              </span>
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <main id="main-content">
      <!-- HERO -->
      <section id="home" class="hero full-screen-section">
        <div class="container hero-grid">
          <div class="hero-left reveal reveal-left">
            <div class="hero-chip">
              <span class="live-dot" aria-hidden="true"></span>
              {{ t.hero.badge }}
            </div>

            <h1 class="hero-title">
              <span>{{ t.hero.title1 }}</span>
              <span class="hero-name">{{ t.hero.name }}</span>
              <span v-if="t.hero.title2">{{ t.hero.title2 }}</span>
            </h1>

            <h2 class="hero-subtitle">
              <span>{{ t.hero.typingPrefix }}</span>
              <span class="typing-word">{{ displayedWord }}</span>
              <span class="cursor" aria-hidden="true">|</span>
            </h2>

            <p class="hero-desc">{{ t.hero.description }}</p>

            <div class="hero-actions">
              <a href="#projects" class="premium-btn primary">
                <span class="btn-liquid" aria-hidden="true"></span>
                <span class="btn-content">{{ t.hero.primaryBtn }}</span>
                <span class="btn-arrow" aria-hidden="true">↗</span>
              </a>

              <a href="#contact" class="premium-btn ghost">
                <span class="btn-content">{{ t.hero.secondaryBtn }}</span>
                <span class="btn-arrow" aria-hidden="true">→</span>
              </a>
            </div>

            <div class="floating-techs" aria-label="Technologies">
              <span v-for="item in TECHS" :key="item">{{ item }}</span>
            </div>
          </div>

          <div class="hero-right reveal reveal-right">
            <div class="glass-stage">
              <div class="stage-glow" aria-hidden="true"></div>

              <div class="lottie-card">
                <div
                  ref="lottieContainer"
                  class="hero-lottie"
                  aria-hidden="true"
                ></div>

                <div class="lottie-caption">
                  <span class="lottie-caption-dot" aria-hidden="true"></span>

                  <div>
                    <strong>{{ t.hero.name }}</strong>
                    <small>{{ t.hero.role }}</small>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- SKILLS -->
      <section id="skills" class="section flush-section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">{{ t.skillsSection.tag }}</span>
            <h2>{{ t.skillsSection.title }}</h2>
            <p>{{ t.skillsSection.desc }}</p>
          </div>

          <div class="skills-grid">
            <article
              v-for="(skill, index) in localizedSkills"
              :key="skill.name"
              class="skill-panel reveal"
              :style="{ '--delay': `${index * 70}ms` }"
            >
              <div class="panel-line"></div>

              <div class="skill-head">
                <div class="skill-icon" aria-hidden="true">
                  {{ skill.icon }}
                </div>

                <div>
                  <small>0{{ index + 1 }}</small>
                  <h3>{{ skill.name }}</h3>
                </div>
              </div>

              <p>{{ skill.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <!-- SERVICES -->
      <section id="services" class="section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">{{ t.services.tag }}</span>
            <h2>{{ t.services.title }}</h2>
            <p>{{ t.services.desc }}</p>
          </div>

          <div class="services-layout">
            <div class="services-lottie-wrap reveal reveal-left">
              <div class="services-lottie-glow" aria-hidden="true"></div>

              <div class="services-lottie-card">
                <div
                  ref="servicesLottieContainer"
                  class="services-lottie"
                  aria-hidden="true"
                ></div>
              </div>
            </div>

            <div class="services-grid reveal reveal-right">
              <article
                v-for="(service, index) in localizedServices"
                :key="service.title"
                class="service-card"
              >
                <div class="service-number">0{{ index + 1 }}</div>
                <div class="service-icon" aria-hidden="true">
                  {{ service.icon }}
                </div>

                <h3>{{ service.title }}</h3>
                <p>{{ service.desc }}</p>

                <div class="service-tags">
                  <span v-for="tag in service.tags" :key="tag">{{ tag }}</span>
                </div>
              </article>
            </div>
          </div>
        </div>
      </section>

      <!-- SHOWCASE -->
      <section class="section showcase-section section-auto">
        <div class="container showcase-grid">
          <div class="showcase-media reveal reveal-left">
            <div class="showcase-image-frame">
              <img
                :src="workspaceImage"
                alt="Developer working at a modern coding workspace"
                width="1400"
                height="930"
                loading="lazy"
                decoding="async"
              />
              <div class="showcase-image-overlay" aria-hidden="true"></div>
            </div>

            <div class="showcase-card showcase-card-1">
              <strong>Vue.js</strong>
              <span>Frontend</span>
            </div>

            <div class="showcase-card showcase-card-2">
              <strong>98%</strong>
              <span>{{ t.showcase.quality }}</span>
            </div>
          </div>

          <div class="showcase-content reveal reveal-right">
            <span class="section-badge">{{ t.showcase.tag }}</span>
            <h2>{{ t.showcase.title }}</h2>
            <p>{{ t.showcase.desc }}</p>

            <div class="showcase-points">
              <div
                v-for="(point, index) in t.showcase.points"
                :key="point.title"
              >
                <span class="showcase-point-icon">0{{ index + 1 }}</span>

                <div>
                  <strong>{{ point.title }}</strong>
                  <p>{{ point.desc }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- ABOUT -->
      <section id="about" class="section soft-surface section-auto">
        <div class="container about-grid">
          <div class="about-main reveal reveal-left">
            <span class="section-badge">{{ t.about.tag }}</span>
            <h2>{{ t.about.title }}</h2>
            <p>{{ t.about.p1 }}</p>
            <p>{{ t.about.p2 }}</p>

            <div class="about-points">
              <div
                v-for="(point, index) in t.about.points"
                :key="index"
                class="about-point reveal"
                :style="{ '--delay': `${index * 80}ms` }"
              >
                <span aria-hidden="true">✦</span>
                <p>{{ point }}</p>
              </div>
            </div>
          </div>

          <div class="about-side">
            <div
              v-for="(card, index) in t.about.cards"
              :key="card.title"
              class="about-card reveal"
              :class="{ accent: card.accent }"
              :style="{ '--delay': `${index * 80}ms` }"
            >
              <small>0{{ index + 1 }}</small>
              <h3>{{ card.title }}</h3>
              <p>{{ card.desc }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- PROJECTS -->
      <section id="projects" class="section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">{{ t.projectsSection.tag }}</span>
            <h2>{{ t.projectsSection.title }}</h2>
            <p>{{ t.projectsSection.desc }}</p>
          </div>

          <div class="pc-toolbar reveal">
            <div
              class="pc-filters"
              role="group"
              :aria-label="t.projectsSection.filterLabel"
            >
              <button
                v-for="filter in projectFilters"
                :key="filter.key"
                type="button"
                class="pc-filter"
                :class="{ active: activeFilter === filter.key }"
                :aria-pressed="activeFilter === filter.key"
                @click="activeFilter = filter.key"
              >
                {{ filter.label }}
                <span class="pc-filter-count">{{ filter.count }}</span>
              </button>
            </div>
          </div>

          <div class="reveal">
            <TransitionGroup name="pc" tag="div" class="projects-showcase">
              <article
                v-for="project in visibleProjects"
                :key="project.key"
                class="pc"
                :style="{ '--pc-rgb': project.rgb }"
              >
                <div class="pc-media">
                  <img
                    :src="workspaceImage"
                    :alt="`${project.title} preview`"
                    width="1200"
                    height="760"
                    loading="lazy"
                    decoding="async"
                    :style="{
                      objectPosition: project.focus,
                      filter: `hue-rotate(${project.hue}deg)`,
                    }"
                  />

                  <div class="pc-shade" aria-hidden="true"></div>

                  <div class="pc-bar" aria-hidden="true">
                    <span class="pc-dots"><i></i><i></i><i></i></span>
                    <span class="pc-url">{{ project.urlLabel }}</span>
                  </div>

                  <div class="pc-overlay">
                    <strong class="pc-preview-title">{{
                      project.previewTitle
                    }}</strong>

                    <span class="pc-status" :class="{ live: project.url }">
                      <i aria-hidden="true"></i>
                      {{
                        project.url
                          ? t.projectsSection.live
                          : t.projectsSection.demo
                      }}
                    </span>
                  </div>
                </div>

                <div class="pc-body">
                  <div class="pc-meta">
                    <span class="pc-number">{{ project.number }}</span>
                    <span class="pc-category">{{ project.category }}</span>
                  </div>

                  <h3>{{ project.title }}</h3>
                  <p>{{ project.description }}</p>

                  <ul
                    class="pc-chips"
                    :aria-label="t.projectsSection.stackLabel"
                  >
                    <li v-for="tag in project.tags" :key="tag">{{ tag }}</li>
                  </ul>

                  <div class="pc-footer">
                    <span class="pc-role">{{ project.role }}</span>

                    <a
                      v-if="project.url"
                      :href="project.url"
                      class="pc-cta"
                      target="_blank"
                      rel="noopener noreferrer"
                    >
                      {{ t.projectsSection.viewLabel }}
                      <span aria-hidden="true">↗</span>
                    </a>

                    <span v-else class="pc-soon">
                      <i aria-hidden="true"></i>
                      {{ t.projectsSection.pendingLabel }}
                    </span>
                  </div>
                </div>
              </article>
            </TransitionGroup>
          </div>
        </div>
      </section>

      <!-- PROCESS -->
      <section class="section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">{{ t.process.tag }}</span>
            <h2>{{ t.process.title }}</h2>
            <p>{{ t.process.desc }}</p>
          </div>

          <div class="process-grid">
            <article
              v-for="(step, index) in t.process.items"
              :key="index"
              class="process-card reveal"
              :style="{ '--delay': `${index * 90}ms` }"
            >
              <div class="process-top">
                <span>0{{ index + 1 }}</span>
                <div></div>
              </div>

              <h3>{{ step.title }}</h3>
              <p>{{ step.desc }}</p>
            </article>
          </div>
        </div>
      </section>

      <!-- CONTACT -->
      <section id="contact" class="section soft-surface section-auto">
        <div class="container">
          <div class="contact-shell reveal">
            <div class="contact-light" aria-hidden="true"></div>

            <div class="contact-content">
              <span class="section-badge">{{ t.contact.tag }}</span>
              <h2>{{ t.contact.title }}</h2>
              <p>{{ t.contact.desc }}</p>

              <div class="contact-actions">
                <a
                  :href="`https://mail.google.com/mail/?view=cm&fs=1&to=${CONTACT_EMAIL}`"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="premium-btn primary"
                >
                  <span class="btn-liquid" aria-hidden="true"></span>
                  <span class="btn-content">{{ t.contact.emailBtn }}</span>
                  <span class="btn-arrow" aria-hidden="true">↗</span>
                </a>

                <a href="#about" class="premium-btn ghost">
                  <span class="btn-content">{{ t.contact.resumeBtn }}</span>
                  <span class="btn-arrow" aria-hidden="true">→</span>
                </a>
              </div>
            </div>

            <div class="contact-lottie-wrap">
              <div
                ref="contactLottieContainer"
                class="contact-lottie"
                aria-hidden="true"
              ></div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <div class="container site-footer-inner">
        <p>
          © {{ currentYear }} {{ t.brand }}.
          {{ isFa ? "تمام حقوق محفوظ است." : "All rights reserved." }}
        </p>

        <nav aria-label="Footer navigation">
          <a href="#home">{{ t.nav.home }}</a>
          <a href="#projects">{{ t.nav.projects }}</a>
          <a href="#contact">{{ t.nav.contact }}</a>
        </nav>
      </div>
    </footer>
  </div>
</template>

<script setup>
import {
  ref,
  computed,
  nextTick,
  onMounted,
  onBeforeUnmount,
  watch,
} from "vue";
import workspaceImage from "./assets/images/photo-programming.avif";

/* =========================================================
   CONSTANTS
========================================================= */
const NAV_ITEMS = [
  "home",
  "skills",
  "services",
  "about",
  "projects",
  "contact",
];
const TECHS = ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"];
const CONTACT_EMAIL = "arian.kalantari@yamil.com";
const SKILL_META = [
  { name: "Vue.js", icon: "🟢" },
  { name: "Flutter", icon: "🔵" },
  { name: "JavaScript", icon: "🟡" },
  { name: "Dart", icon: "🧩" },
  { name: "Go", icon: "⚙️" },
  { name: "UI / UX", icon: "✨" },
];
const SERVICE_META = [
  {
    title: "Web Development",
    icon: "◈",
    tags: ["Vue.js", "JavaScript", "Responsive"],
  },
  {
    title: "Mobile Development",
    icon: "⌁",
    tags: ["Flutter", "Dart", "Mobile"],
  },
  { title: "UI / UX", icon: "✦", tags: ["UI", "UX", "Design"] },
  {
    title: "Interactive Experience",
    icon: "✺",
    tags: ["Animation", "Motion", "Lottie"],
  },
];
const PROJECTS = [
  {
    key: "web",
    number: "01",
    rgb: "88, 216, 255",
    hue: 0,
    focus: "50% 35%",
    title: "Modern Business Website",
    category: "Corporate / Landing",
    previewTitle: "Modern Digital Experience",
    urlLabel: "preview.local / business",
    role: "Design + Frontend",
    tags: ["Vue.js", "Responsive", "UI/UX"],
    url: "",
  },
  {
    key: "mobile",
    number: "02",
    rgb: "111, 240, 200",
    hue: -22,
    focus: "18% 60%",
    title: "Mobile Application",
    category: "Flutter Application",
    previewTitle: "Smooth Mobile Interface",
    urlLabel: "app.preview / mobile",
    role: "UI + Flutter",
    tags: ["Flutter", "Dart", "UX"],
    url: "",
  },
  {
    key: "dashboard",
    number: "03",
    rgb: "132, 121, 255",
    hue: 28,
    focus: "82% 40%",
    title: "Admin Dashboard",
    category: "Web Application",
    previewTitle: "Data Management System",
    urlLabel: "dashboard.local / admin",
    role: "Frontend Development",
    tags: ["Vue.js", "JavaScript", "Admin UI"],
    url: "",
  },
];

/* Lottie JSON files are code-split: nothing is downloaded until needed. */
const ANIMATIONS = {
  loader: () => import("./assets/animations/Jellyfish greeting you!.json"),
  hero: () => import("./assets/animations/pollwon.json"),
  services: () => import("./assets/animations/Window layout.json"),
  contact: () => import("./assets/animations/tech startup.json"),
  mobile: () => import("./assets/animations/bird (1).json"),
};

const MIN_LOADING_MS = 3800;
const MAX_LOADING_MS = 6500;

/* =========================================================
   TRANSLATIONS
========================================================= */
const translations = {
  fa: {
    brand: "آرین کلانتری",
    nav: {
      home: "خانه",
      skills: "مهارت‌ها",
      services: "خدمات",
      about: "درباره من",
      projects: "پروژه‌ها",
      contact: "ارتباط",
    },
    actions: { light: "روشن", dark: "دارک" },
    loader: "تبدیل ایده‌ها به کد",
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
      role: "Frontend, Flutter, Go Developer",
    },
    skillsSection: {
      tag: "زبان‌ها و مهارت‌ها",
      title: "فناوری‌هایی که با آن‌ها تجربه می‌سازم",
      desc: "تمرکز من روی طراحی و توسعه محصولاتی است که هم از نظر بصری حرفه‌ای باشند و هم از نظر تجربه کاربر، روان، سریع و قابل اعتماد عمل کنند.",
      items: [
        "ساخت رابط‌های مدرن، سریع و تمیز برای وب‌اپلیکیشن‌ها و صفحات حرفه‌ای.",
        "توسعه اپلیکیشن‌های موبایل با تجربه‌ای روان، زیبا و کاربرپسند.",
        "پیاده‌سازی منطق تعاملی، داینامیک و توسعه‌پذیر برای رابط‌های مدرن.",
        "کدنویسی ساخت‌یافته و مناسب برای اپلیکیشن‌های Flutter با نگهداری ساده.",
        "کار با منطق سریع و سبک برای توسعه سرویس‌ها و درک بهتر معماری بک‌اند.",
        "طراحی تجربه کاربری و رابط کاربری با تمرکز بر وضوح، زیبایی و رفتار کاربر.",
      ],
    },
    services: {
      tag: "خدمات",
      title: "چیزهایی که برای ساختن یک محصول دیجیتال انجام می‌دهم",
      desc: "از ایده و رابط کاربری تا توسعه و جزئیات نهایی، تمرکزم روی ساخت تجربه‌ای حرفه‌ای و قابل استفاده است.",
      items: [
        "ساخت وب‌سایت‌ها و وب‌اپلیکیشن‌های مدرن، سریع و ریسپانسیو.",
        "طراحی و توسعه رابط‌های موبایل با تمرکز روی تجربه کاربری روان.",
        "طراحی رابط و تجربه کاربری با تمرکز روی جزئیات، وضوح و رفتار کاربر.",
        "ساخت تعاملات و انیمیشن‌های سبک برای ایجاد تجربه‌ای زنده و متفاوت.",
      ],
    },
    showcase: {
      tag: "رویکرد من",
      title: "طراحی دقیق، کدنویسی تمیز و تجربه‌ای که حس کیفیت می‌دهد",
      desc: "هر پروژه را به‌عنوان یک محصول کامل می‌بینم؛ از ساختار و سرعت گرفته تا جزئیات بصری، تعاملات و کیفیت تجربه کاربر.",
      points: [
        {
          title: "سرعت در اولویت",
          desc: "کد سبک، انیمیشن کنترل‌شده و بارگذاری تصاویر با lazy loading.",
        },
        {
          title: "ریسپانسیو از پایه",
          desc: "چیدمان از ابتدا برای موبایل، تبلت و دسکتاپ طراحی می‌شود.",
        },
        {
          title: "رابط کاربری دقیق",
          desc: "فاصله‌ها، رنگ‌ها، تایپوگرافی و تعاملات با یک سیستم بصری یکپارچه.",
        },
      ],
      quality: "کیفیت طراحی",
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
      title: "کارهایی که با آن‌ها تجربه‌های دیجیتال می‌سازم",
      desc: "نمونه‌هایی از وب‌سایت‌ها، اپلیکیشن‌ها و رابط‌هایی که با تمرکز روی طراحی، تجربه کاربری و پیاده‌سازی حرفه‌ای ساخته شده‌اند.",
      stackLabel: "تکنولوژی",
      viewLabel: "مشاهده پروژه",
      pendingLabel: "در حال آماده‌سازی",
      live: "آنلاین",
      demo: "نمونه",
      filterLabel: "فیلتر پروژه‌ها",
      filters: {
        all: "همه",
        web: "وب‌سایت",
        mobile: "موبایل",
        dashboard: "داشبورد",
      },
      descriptions: [
        "یک وب‌سایت مدرن با تمرکز روی ساختار حرفه‌ای، انیمیشن‌های نرم، ریسپانسیو بودن و تجربه کاربری روان.",
        "طراحی و توسعه رابط یک اپلیکیشن موبایل با تمرکز روی سرعت، تعاملات نرم و تجربه کاربری مدرن.",
        "یک داشبورد مدیریتی با ساختار تمیز، کامپوننت‌های قابل توسعه و رابط کاربری مناسب برای داده‌های زیاد.",
      ],
    },
    process: {
      tag: "مراحل کار",
      title: "از ایده تا محصول نهایی",
      desc: "فرآیند توسعه را به بخش‌های مشخص تقسیم می‌کنم تا نتیجه تمیز، قابل کنترل و قابل توسعه باشد.",
      items: [
        {
          title: "شناخت",
          desc: "شناخت نیاز، هدف پروژه، کاربران و ساختار اصلی محصول.",
        },
        {
          title: "طراحی",
          desc: "ساخت ساختار بصری، کامپوننت‌ها و تجربه کاربری.",
        },
        {
          title: "پیاده‌سازی",
          desc: "پیاده‌سازی تمیز، ریسپانسیو و بهینه با تکنولوژی مناسب.",
        },
        {
          title: "بهبود",
          desc: "تست، اصلاح جزئیات، بهینه‌سازی و آماده‌سازی نسخه نهایی.",
        },
      ],
    },
    contact: {
      tag: "ارتباط با من",
      title: "برای همکاری در پروژه‌های خود در تماس باشید",
      desc: "اگر به یک وب‌سایت شیک، رابط کاربری مدرن، اپ Flutter یا طراحی UI/UX نیاز دارید، خوشحال می‌شوم همکاری کنیم.",
      emailBtn: "ارسال ایمیل",
      resumeBtn: "مشاهده اطلاعات بیشتر",
    },
    seo: {
      title: "آرین کلانتری | توسعه‌دهنده Frontend، Flutter، Go و UI/UX",
      description:
        "پورتفولیوی آرین کلانتری؛ توسعه‌دهنده Frontend با Vue.js، توسعه‌دهنده Flutter و طراح UI/UX با تمرکز بر ساخت تجربه‌های دیجیتال سریع، مدرن و حرفه‌ای.",
      locale: "fa_IR",
    },
  },

  en: {
    brand: "Arian Kalantari",
    nav: {
      home: "Home",
      skills: "Skills",
      services: "Services",
      about: "About",
      projects: "Projects",
      contact: "Contact",
    },
    actions: { light: "Light", dark: "Dark" },
    loader: "Where ideas meet code",
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
    },
    skillsSection: {
      tag: "Languages & Skills",
      title: "Technologies I use to craft experiences",
      desc: "My focus is on designing and building products that feel visually polished while delivering smooth, fast and dependable user experiences.",
      items: [
        "Building modern, fast and clean interfaces for web applications and premium pages.",
        "Developing mobile applications with smooth, elegant and user-friendly experiences.",
        "Implementing interactive, dynamic and scalable logic for modern interfaces.",
        "Structured development for Flutter apps with cleaner maintenance and readability.",
        "Working with fast and lightweight logic for services and backend-oriented thinking.",
        "Designing user interfaces and experiences focused on clarity, beauty and behavior.",
      ],
    },
    services: {
      tag: "SERVICES",
      title: "What I build for digital products",
      desc: "From interface design to development and final polish, I focus on creating digital experiences that feel refined and usable.",
      items: [
        "Modern, fast and responsive websites and web applications.",
        "Smooth mobile interfaces with a strong focus on interaction and usability.",
        "User interfaces and experiences designed around clarity, detail and behavior.",
        "Lightweight interactions and animations that make interfaces feel alive and distinctive.",
      ],
    },
    showcase: {
      tag: "APPROACH",
      title: "Precise design, clean code and experiences that feel premium",
      desc: "I treat every project as a complete product — from structure and performance to visual details, interactions and user experience.",
      points: [
        {
          title: "Performance First",
          desc: "Lean code, controlled animation and lazy-loaded imagery.",
        },
        {
          title: "Responsive by Design",
          desc: "Layouts are designed from the start for mobile, tablet and desktop.",
        },
        {
          title: "Polished UI",
          desc: "Spacing, color, typography and interaction work as one visual system.",
        },
      ],
      quality: "design quality",
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
      tag: "PROJECTS",
      title: "Selected work and digital experiences",
      desc: "A showcase of websites, applications and interfaces built with a focus on design, user experience and polished implementation.",
      stackLabel: "STACK",
      viewLabel: "VIEW PROJECT",
      pendingLabel: "Preparing project",
      live: "LIVE",
      demo: "DEMO",
      filterLabel: "Filter projects",
      filters: {
        all: "All",
        web: "Websites",
        mobile: "Mobile",
        dashboard: "Dashboards",
      },
      descriptions: [
        "A polished business website focused on strong visual hierarchy, smooth animation, responsive layout and refined UX.",
        "A mobile experience designed around fluid interactions, clean interfaces and scalable Flutter architecture.",
        "A modern admin interface with clean structure, reusable components and a scalable visual system.",
      ],
    },
    process: {
      tag: "WORKFLOW",
      title: "From idea to final product",
      desc: "A structured process keeps the result clean, controlled and ready for future growth.",
      items: [
        {
          title: "Discover",
          desc: "Understanding the goals, users, requirements and product structure.",
        },
        {
          title: "Design",
          desc: "Creating the visual system, components and user experience.",
        },
        {
          title: "Build",
          desc: "Developing a clean, responsive and optimized product.",
        },
        {
          title: "Refine",
          desc: "Testing, polishing details and preparing the final experience.",
        },
      ],
    },
    contact: {
      tag: "CONTACT",
      title: "Let's work together on polished digital products",
      desc: "If you need a beautiful website, modern UI, Flutter app or UI/UX design, I would be happy to collaborate.",
      emailBtn: "Send Email",
      resumeBtn: "More Information",
    },
    seo: {
      title: "Arian Kalantari | Frontend, Flutter, Go & UI/UX Developer",
      description:
        "Arian Kalantari's portfolio — Frontend developer specializing in Vue.js, Flutter, Go and UI/UX, focused on building modern, fast and polished digital experiences.",
      locale: "en_US",
    },
  },
};

/* =========================================================
   STATE
========================================================= */
const readPref = (key, allowed, fallback) => {
  try {
    const value = localStorage.getItem(key);
    return allowed.includes(value) ? value : fallback;
  } catch {
    return fallback;
  }
};

const writePref = (key, value) => {
  try {
    localStorage.setItem(key, value);
  } catch {
    /* storage unavailable (private mode) */
  }
};

const lang = ref(readPref("ak-lang", ["fa", "en"], "fa"));
const theme = ref(readPref("ak-theme", ["dark", "light"], "dark"));
const menuOpen = ref(false);
const isInitialLoading = ref(true);
const activeFilter = ref("all");
const displayedWord = ref("");
const trailEnabled = ref(false);

const isFa = computed(() => lang.value === "fa");
const t = computed(() => translations[lang.value]);
const loaderTitle = computed(() => t.value.loader);
const currentYear = new Date().getFullYear();

const toggleLang = () => {
  lang.value = isFa.value ? "en" : "fa";
};
const toggleTheme = () => {
  theme.value = theme.value === "dark" ? "light" : "dark";
};

const localizedSkills = computed(() =>
  SKILL_META.map((skill, i) => ({
    ...skill,
    description: t.value.skillsSection.items[i],
  })),
);

const localizedServices = computed(() =>
  SERVICE_META.map((service, i) => ({
    ...service,
    desc: t.value.services.items[i],
  })),
);

const projectsView = computed(() =>
  PROJECTS.map((project, i) => ({
    ...project,
    description: t.value.projectsSection.descriptions[i],
  })),
);

const visibleProjects = computed(() =>
  activeFilter.value === "all"
    ? projectsView.value
    : projectsView.value.filter(
        (project) => project.key === activeFilter.value,
      ),
);

const projectFilters = computed(() => {
  const labels = t.value.projectsSection.filters;

  return [
    { key: "all", label: labels.all, count: PROJECTS.length },
    ...PROJECTS.map(({ key }) => ({ key, label: labels[key], count: 1 })),
  ];
});

/* =========================================================
   TEMPLATE REFS
========================================================= */
const loaderBarEl = ref(null);
const loaderPercentEl = ref(null);
const loaderProgressEl = ref(null);
const loaderLottieContainer = ref(null);
const lottieContainer = ref(null);
const servicesLottieContainer = ref(null);
const contactLottieContainer = ref(null);
const mobileLottieContainer = ref(null);

/* =========================================================
   SEO
========================================================= */
const setMeta = (selector, attributes) => {
  let element = document.head.querySelector(selector);

  if (!element) {
    element = document.createElement("meta");
    document.head.appendChild(element);
  }

  Object.entries(attributes).forEach(([key, value]) =>
    element.setAttribute(key, value),
  );
};

const ROBOTS =
  "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1";

const updateThemeColor = () => {
  setMeta('meta[name="theme-color"]', {
    name: "theme-color",
    content: theme.value === "dark" ? "#07111f" : "#f6f8fb",
  });
};

const updateSeo = () => {
  const { title, description, locale } = t.value.seo;

  document.title = title;

  setMeta('meta[name="description"]', {
    name: "description",
    content: description,
  });
  setMeta('meta[name="robots"]', { name: "robots", content: ROBOTS });
  setMeta('meta[name="googlebot"]', { name: "googlebot", content: ROBOTS });
  setMeta('meta[name="author"]', {
    name: "author",
    content: "Arian Kalantari",
  });
  setMeta('meta[property="og:title"]', {
    property: "og:title",
    content: title,
  });
  setMeta('meta[property="og:description"]', {
    property: "og:description",
    content: description,
  });
  setMeta('meta[property="og:type"]', {
    property: "og:type",
    content: "website",
  });
  setMeta('meta[property="og:locale"]', {
    property: "og:locale",
    content: locale,
  });
  setMeta('meta[property="og:site_name"]', {
    property: "og:site_name",
    content: "Arian Kalantari",
  });
  setMeta('meta[name="twitter:card"]', {
    name: "twitter:card",
    content: "summary_large_image",
  });
  setMeta('meta[name="twitter:title"]', {
    name: "twitter:title",
    content: title,
  });
  setMeta('meta[name="twitter:description"]', {
    name: "twitter:description",
    content: description,
  });

  let canonical = document.head.querySelector('link[rel="canonical"]');

  if (!canonical) {
    canonical = document.createElement("link");
    canonical.rel = "canonical";
    document.head.appendChild(canonical);
  }

  canonical.href = window.location.origin + window.location.pathname;

  document.documentElement.setAttribute("lang", lang.value);
  document.documentElement.setAttribute("dir", isFa.value ? "rtl" : "ltr");

  if (!document.getElementById("portfolio-structured-data")) {
    const script = document.createElement("script");

    script.type = "application/ld+json";
    script.id = "portfolio-structured-data";
    script.textContent = JSON.stringify({
      "@context": "https://schema.org",
      "@type": "ProfilePage",
      mainEntity: {
        "@type": "Person",
        name: "Arian Kalantari",
        jobTitle: "Frontend, Flutter, Go & UI/UX Developer",
        description:
          "Frontend developer specializing in Vue.js, Flutter, Go and UI/UX.",
        knowsAbout: [
          "Vue.js",
          "JavaScript",
          "Flutter",
          "Dart",
          "Go",
          "UI/UX",
          "Frontend Development",
          "Web Development",
          "Mobile Development",
        ],
      },
    });

    document.head.appendChild(script);
  }
};

/* =========================================================
   FONTS
========================================================= */
const loadFonts = () => {
  if (document.getElementById("ak-fonts")) return;

  const addLink = (attrs) => {
    const link = document.createElement("link");

    Object.entries(attrs).forEach(([key, value]) =>
      link.setAttribute(key, value),
    );
    document.head.appendChild(link);
  };

  addLink({ rel: "preconnect", href: "https://fonts.googleapis.com" });
  addLink({
    rel: "preconnect",
    href: "https://fonts.gstatic.com",
    crossorigin: "",
  });
  addLink({
    id: "ak-fonts",
    rel: "stylesheet",
    href:
      "https://fonts.googleapis.com/css2" +
      "?family=Plus+Jakarta+Sans:wght@400;500;600;700;800" +
      "&family=Space+Grotesk:wght@500;600;700" +
      "&family=Vazirmatn:wght@400;500;600;700;800" +
      "&family=JetBrains+Mono:wght@500;700" +
      "&display=swap",
  });
};

/* =========================================================
   TYPING EFFECT
========================================================= */
let typingTimeout = 0;
let wordIndex = 0;
let charIndex = 0;
let deleting = false;

const typeEffect = () => {
  const word = TECHS[wordIndex];

  if (!deleting) {
    charIndex += 1;
    displayedWord.value = word.slice(0, charIndex);

    if (charIndex === word.length) {
      deleting = true;
      typingTimeout = window.setTimeout(typeEffect, 1200);

      return;
    }
  } else {
    charIndex -= 1;
    displayedWord.value = word.slice(0, charIndex);

    if (charIndex === 0) {
      deleting = false;
      wordIndex = (wordIndex + 1) % TECHS.length;
    }
  }

  typingTimeout = window.setTimeout(typeEffect, deleting ? 42 : 78);
};

const resetTyping = () => {
  clearTimeout(typingTimeout);
  displayedWord.value = "";
  wordIndex = 0;
  charIndex = 0;
  deleting = false;
  typeEffect();
};

/* =========================================================
   LOTTIE (lazy, code-split, paused off-screen)
========================================================= */
const prefersReducedMotion =
  typeof window !== "undefined" &&
  window.matchMedia("(prefers-reduced-motion: reduce)").matches;

let lottiePromise = null;

const loadLottie = () =>
  (lottiePromise ??= import("lottie-web").then((module) => module.default));

const mountLottie = async (container, key, { speed = 1 } = {}) => {
  const [lottie, animation] = await Promise.all([
    loadLottie(),
    ANIMATIONS[key](),
  ]);

  if (!container?.isConnected) return null;

  const instance = lottie.loadAnimation({
    container,
    renderer: "svg",
    loop: true,
    autoplay: !prefersReducedMotion,
    animationData: animation.default,
    rendererSettings: {
      preserveAspectRatio: "xMidYMid meet",
      progressiveLoad: true,
      hideOnTransparent: true,
    },
  });

  instance.setSpeed(speed);

  return instance;
};

/* loader animation */
let loaderLottieInstance = null;

const initLoaderLottie = async () => {
  const instance = await mountLottie(loaderLottieContainer.value, "loader", {
    speed: 0.9,
  });

  if (!isInitialLoading.value) {
    instance?.destroy();

    return;
  }

  loaderLottieInstance = instance;
};

const destroyLoaderLottie = () => {
  loaderLottieInstance?.destroy();
  loaderLottieInstance = null;
};

/* page animations */
const lottieEntries = new Map();
let lottieObserver = null;
let mobileLottieEl = null;

const startEntry = (entry) => {
  if (entry.instance) {
    if (!prefersReducedMotion) entry.instance.play();

    return;
  }

  if (entry.loading) return;

  entry.loading = true;

  mountLottie(entry.el, entry.key, entry.options).then((instance) => {
    entry.loading = false;

    if (entry.disposed) {
      instance?.destroy();

      return;
    }

    entry.instance = instance;

    if (!entry.visible) instance?.pause();
  });
};

const setupLottieObserver = () => {
  lottieObserver = new IntersectionObserver(
    (records) => {
      records.forEach(({ target, isIntersecting }) => {
        const entry = lottieEntries.get(target);

        if (!entry) return;

        entry.visible = isIntersecting;

        if (isIntersecting) startEntry(entry);
        else entry.instance?.pause();
      });
    },
    { rootMargin: "160px 0px" },
  );
};

const observeLottie = (el, key, options = {}) => {
  if (!el || lottieEntries.has(el)) return;

  lottieEntries.set(el, {
    el,
    key,
    options,
    instance: null,
    loading: false,
    visible: false,
    disposed: false,
  });

  lottieObserver.observe(el);
};

const unobserveLottie = (el) => {
  const entry = lottieEntries.get(el);

  if (!entry) return;

  entry.disposed = true;
  entry.instance?.destroy();
  lottieObserver?.unobserve(el);
  lottieEntries.delete(el);
};

const initLotties = () => {
  observeLottie(lottieContainer.value, "hero");
  observeLottie(servicesLottieContainer.value, "services", { speed: 0.8 });
  observeLottie(contactLottieContainer.value, "contact", { speed: 0.75 });
};

/* the mobile menu is rendered with v-if: its animation lives only while open */
const releaseMobileLottie = () => {
  if (!mobileLottieEl) return;

  unobserveLottie(mobileLottieEl);
  mobileLottieEl = null;
};

watch(menuOpen, async (open) => {
  if (!open) return;

  await nextTick();

  const el = mobileLottieContainer.value;

  if (!el || el === mobileLottieEl) return;

  mobileLottieEl = el;
  observeLottie(el, "mobile", { speed: 0.7 });
});

/* =========================================================
   MOUSE TRAIL (original look: soft glowing stars)
   Desktop / fine pointer only, so touch devices pay nothing.
========================================================= */
const stars = ref([]);

let starId = 0;
let lastEmit = 0;
let lastMove = { x: 0, y: 0, time: 0 };

const pick = (arr) => arr[Math.floor(Math.random() * arr.length)];

const getPalette = () =>
  theme.value === "dark"
    ? [
        { color: "#ffffff", glow: "rgba(255,255,255,0.90)" },
        { color: "#62e8ff", glow: "rgba(98,232,255,0.72)" },
        { color: "#8f7cff", glow: "rgba(143,124,255,0.68)" },
        { color: "#73f4cf", glow: "rgba(115,244,207,0.62)" },
        { color: "#ff8eea", glow: "rgba(255,142,234,0.54)" },
        { color: "#ffd36b", glow: "rgba(255,211,107,0.48)" },
        { color: "#75a9ff", glow: "rgba(117,169,255,0.56)" },
      ]
    : [
        { color: "#ffffff", glow: "rgba(255,255,255,0.92)" },
        { color: "#7c3aed", glow: "rgba(124,58,237,0.34)" },
        { color: "#0ea5e9", glow: "rgba(14,165,233,0.34)" },
        { color: "#14b8a6", glow: "rgba(20,184,166,0.30)" },
        { color: "#db5cff", glow: "rgba(219,92,255,0.27)" },
        { color: "#f59e0b", glow: "rgba(245,158,11,0.24)" },
      ];

const createSoftStar = (x, y, spread = 10) => {
  const tone = pick(getPalette());
  const duration = 430 + Math.random() * 300;
  const id = starId++;

  if (stars.value.length >= 22) stars.value.shift();

  stars.value.push({
    id,
    x: x + (Math.random() * spread - spread / 2),
    y: y + (Math.random() * spread - spread / 2),
    size: Math.random() * 3.1 + 1.15,
    color: tone.color,
    glow: tone.glow,
    duration,
    driftX: (Math.random() - 0.5) * 32,
    driftY: -10 - Math.random() * 30,
    rotation: -45 + Math.random() * 90,
  });

  window.setTimeout(() => {
    const index = stars.value.findIndex((star) => star.id === id);

    if (index !== -1) stars.value.splice(index, 1);
  }, duration + 80);
};

const clearTrail = () => {
  stars.value = [];
};

const handleMouseMove = (e) => {
  const now = performance.now();
  const deltaTime = Math.max(8, now - (lastMove.time || now));
  const dx = e.clientX - (lastMove.x || e.clientX);
  const dy = e.clientY - (lastMove.y || e.clientY);
  const speed = Math.hypot(dx, dy) / deltaTime;

  lastMove = { x: e.clientX, y: e.clientY, time: now };

  const emitGap = speed > 0.85 ? 16 : speed > 0.35 ? 22 : 30;

  if (now - lastEmit < emitGap) return;

  lastEmit = now;

  createSoftStar(e.clientX, e.clientY, speed > 0.85 ? 10 : 7);

  if (speed > 0.4) createSoftStar(e.clientX, e.clientY, 12);
  if (speed > 0.9 && Math.random() > 0.35)
    createSoftStar(e.clientX, e.clientY, 17);
  if (speed > 1.35 && Math.random() > 0.55)
    createSoftStar(e.clientX, e.clientY, 21);
};

const handleMouseOut = (e) => {
  if (!e.relatedTarget) clearTrail();
};

const setupTrail = () => {
  const canUseTrail =
    !prefersReducedMotion &&
    window.matchMedia("(hover: hover) and (pointer: fine)").matches;

  if (!canUseTrail) return;

  trailEnabled.value = true;

  window.addEventListener("mousemove", handleMouseMove, { passive: true });
  window.addEventListener("mouseout", handleMouseOut);
  document.documentElement.addEventListener("mouseleave", clearTrail);
};

/* =========================================================
   REVEAL ON SCROLL
========================================================= */
let revealObserver = null;

const setupReveal = () => {
  const pending = document.querySelectorAll(".reveal:not(.in-view)");

  revealObserver ??= new IntersectionObserver(
    (records) => {
      records.forEach((record) => {
        if (!record.isIntersecting) return;

        record.target.classList.add("in-view");
        revealObserver.unobserve(record.target);
      });
    },
    { threshold: 0.12, rootMargin: "0px 0px -40px 0px" },
  );

  pending.forEach((element) => revealObserver.observe(element));
};

/* =========================================================
   INITIAL LOADER
   Real readiness (window load) with a short minimum, instead of a
   fixed 3.4s timer. The loop only writes transform/text: no re-render.
========================================================= */
let loaderFrame = 0;
let lastPercent = -1;

const renderLoaderProgress = (value) => {
  const percent = Math.round(value * 100);

  if (loaderBarEl.value) loaderBarEl.value.style.transform = `scaleX(${value})`;

  if (percent === lastPercent) return;

  lastPercent = percent;

  if (loaderPercentEl.value) loaderPercentEl.value.textContent = `${percent}%`;
  if (loaderProgressEl.value)
    loaderProgressEl.value.setAttribute("aria-valuenow", String(percent));
};

const startPage = () => {
  typeEffect();
  initLotties();
  setupReveal();
};

const finishInitialLoading = () => {
  if (!isInitialLoading.value) return;

  isInitialLoading.value = false;
  nextTick(startPage);
};

const runLoader = () => {
  const startedAt = performance.now();
  let pageReady = document.readyState === "complete";
  let progress = 0;

  window.addEventListener(
    "load",
    () => {
      pageReady = true;
    },
    { once: true },
  );

  const tick = (now) => {
    const elapsed = now - startedAt;
    const canFinish =
      (pageReady && elapsed >= MIN_LOADING_MS) || elapsed >= MAX_LOADING_MS;
    const raw = Math.min(1, elapsed / MIN_LOADING_MS);
    const target = canFinish ? 1 : 0.92 * (0.5 - Math.cos(Math.PI * raw) / 2);

    progress += (target - progress) * 0.14;

    if (canFinish && progress > 0.995) {
      renderLoaderProgress(1);
      finishInitialLoading();

      return;
    }

    renderLoaderProgress(progress);
    loaderFrame = requestAnimationFrame(tick);
  };

  loaderFrame = requestAnimationFrame(tick);
};

/* =========================================================
   LIFECYCLE
========================================================= */
onMounted(() => {
  loadFonts();
  updateSeo();
  updateThemeColor();
  setupLottieObserver();
  initLoaderLottie();
  runLoader();
  setupTrail();
});

onBeforeUnmount(() => {
  clearTimeout(typingTimeout);
  cancelAnimationFrame(loaderFrame);

  destroyLoaderLottie();

  lottieEntries.forEach((entry) => {
    entry.disposed = true;
    entry.instance?.destroy();
  });
  lottieEntries.clear();
  lottieObserver?.disconnect();
  revealObserver?.disconnect();

  window.removeEventListener("mousemove", handleMouseMove);
  window.removeEventListener("mouseout", handleMouseOut);
  document.documentElement.removeEventListener("mouseleave", clearTrail);
});

watch(theme, (value) => {
  writePref("ak-theme", value);
  updateThemeColor();
});

watch(lang, (value) => {
  writePref("ak-lang", value);
  menuOpen.value = false;
  updateSeo();

  if (!isInitialLoading.value) resetTyping();

  nextTick(setupReveal);
});
</script>

<style>
/* Global resets (intentionally unscoped) */
*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  scroll-padding-top: 84px;
  background: #07111f;
}

body {
  margin: 0;
  overflow-x: hidden;
  background: #07111f;
  font-family:
    "Plus Jakarta Sans",
    "Vazirmatn",
    system-ui,
    -apple-system,
    "Segoe UI",
    sans-serif;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
}

a {
  text-decoration: none;
}

button,
input,
textarea,
select {
  font-family: inherit;
}

img,
svg,
video,
canvas {
  max-width: 100%;
  display: block;
}

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }
}
</style>

<style scoped>
/* =========================================================
   ROOT + THEME TOKENS
========================================================= */
.app {
  --bg: #07111f;
  --surface: rgba(255, 255, 255, 0.045);
  --surface-strong: rgba(255, 255, 255, 0.07);
  --surface-soft: rgba(255, 255, 255, 0.022);
  --text: #f3f7fd;
  --text-soft: #b4c1d4;
  --muted: #8c9bb2;
  --border: rgba(255, 255, 255, 0.1);
  --border-strong: rgba(90, 217, 255, 0.28);
  --primary: #5ad9ff;
  --primary-2: #8479ff;
  --accent: #6ff0c8;
  --shadow: 0 25px 60px rgba(0, 0, 0, 0.23);
  --on-primary: #06121f;
  --nav-bg: rgba(7, 17, 31, 0.78);
  --font-sans:
    "Plus Jakarta Sans", "Vazirmatn", system-ui, -apple-system, "Segoe UI",
    sans-serif;
  --font-display:
    "Space Grotesk", "Vazirmatn", "Plus Jakarta Sans", system-ui, sans-serif;
  --font-mono:
    "JetBrains Mono", "Fira Code", ui-monospace, SFMono-Regular, Menlo,
    monospace;

  position: relative;
  min-width: 0;
  min-height: 100vh;
  padding-top: 74px;
  overflow-x: clip;
  isolation: isolate;
  color: var(--text);
  background: var(--bg);
  font-family: var(--font-sans);
}

.app.light {
  --bg: #f5f7fb;
  --surface: rgba(255, 255, 255, 0.76);
  --surface-strong: rgba(255, 255, 255, 0.92);
  --surface-soft: rgba(255, 255, 255, 0.54);
  --text: #0e1626;
  --text-soft: #44536a;
  --muted: #5d6c82;
  --border: rgba(15, 23, 42, 0.1);
  --border-strong: rgba(6, 120, 170, 0.28);
  --primary: #0678aa;
  --primary-2: #5a49d6;
  --accent: #0c9a7e;
  --shadow: 0 25px 60px rgba(57, 77, 108, 0.1);
  --on-primary: #ffffff;
  --nav-bg: rgba(255, 255, 255, 0.92);
}

.app.is-loading {
  max-height: 100vh;
  overflow: hidden;
}

.app h2,
.app h3 {
  font-family: var(--font-display);
}

.app :focus-visible {
  outline: 2px solid var(--primary);
  outline-offset: 3px;
  border-radius: 8px;
}

/* Lottie SVGs are injected at runtime, so they need :deep() */
.initial-loader-lottie :deep(svg),
.hero-lottie :deep(svg),
.services-lottie :deep(svg),
.contact-lottie :deep(svg),
.mobile-lottie :deep(svg) {
  width: 100% !important;
  height: 100% !important;
  display: block;
}

.skip-link {
  position: fixed;
  top: 12px;
  left: 12px;
  z-index: 9999;
  padding: 10px 14px;
  border-radius: 10px;
  background: var(--primary);
  color: #07111f;
  font-size: 12px;
  font-weight: 800;
  transform: translateY(-150%);
  transition: transform 0.2s ease;
}

.skip-link:focus {
  transform: translateY(0);
}

/* =========================================================
   INITIAL LOADER
========================================================= */
.initial-loader {
  position: fixed;
  inset: 0;
  z-index: 10000;
  display: grid;
  place-items: center;
  overflow: hidden;
  isolation: isolate;
  color: #fff;
  background:
    radial-gradient(
      circle at 50% 42%,
      rgba(88, 216, 255, 0.12),
      transparent 55%
    ),
    radial-gradient(
      circle at 82% 88%,
      rgba(116, 105, 255, 0.12),
      transparent 50%
    ),
    #050b14;
}

.initial-loader-grid {
  position: absolute;
  inset: 0;
  opacity: 0.3;
  background-image:
    linear-gradient(rgba(88, 216, 255, 0.055) 1px, transparent 1px),
    linear-gradient(90deg, rgba(88, 216, 255, 0.055) 1px, transparent 1px);
  background-size: 42px 42px;
  mask-image: radial-gradient(circle at center, #000 0%, transparent 78%);
  -webkit-mask-image: radial-gradient(
    circle at center,
    #000 0%,
    transparent 78%
  );
}

.initial-loader-center {
  position: relative;
  z-index: 2;
  width: min(760px, calc(100vw - 32px));
  padding: clamp(18px, 4vw, 34px);
  text-align: center;
}

.initial-loader-animation {
  position: relative;
  width: clamp(146px, 31vw, 198px);
  height: clamp(146px, 31vw, 198px);
  margin: 56px auto 84px;
  display: grid;
  place-items: center;
}

.initial-loader-ring {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 1px solid rgba(157, 225, 255, 0.11);
  border-top-color: #58d8ff;
  border-right-color: rgba(88, 216, 255, 0.75);
  border-bottom-color: rgba(114, 242, 203, 0.24);
  animation: loaderRingSpin 4.8s cubic-bezier(0.65, 0, 0.35, 1) infinite;
}

.initial-loader-ring-2 {
  inset: 15px;
  border-color: rgba(116, 105, 255, 0.1);
  border-bottom-color: #7469ff;
  border-left-color: rgba(116, 105, 255, 0.62);
  animation-duration: 6.6s;
  animation-direction: reverse;
}

/* big multicolor rings: conic gradient cut into a thin ring by a static mask */
.initial-loader-ring-3,
.initial-loader-ring-4 {
  border: 0;
  background: conic-gradient(
    from 0deg,
    #58d8ff,
    #7469ff,
    #ff8eea,
    #ffd36b,
    #72f2cb,
    #58d8ff
  );
  -webkit-mask: radial-gradient(
    farthest-side,
    transparent calc(100% - 3px),
    #000 calc(100% - 2px)
  );
  mask: radial-gradient(
    farthest-side,
    transparent calc(100% - 3px),
    #000 calc(100% - 2px)
  );
  animation: loaderRingSpin 9s linear infinite;
}

.initial-loader-ring-3 {
  inset: -30px;
  opacity: 0.85;
}

.initial-loader-ring-4 {
  inset: -56px;
  opacity: 0.4;
  -webkit-mask: radial-gradient(
    farthest-side,
    transparent calc(100% - 2px),
    #000 calc(100% - 1px)
  );
  mask: radial-gradient(
    farthest-side,
    transparent calc(100% - 2px),
    #000 calc(100% - 1px)
  );
  animation-duration: 14s;
  animation-direction: reverse;
}

.initial-loader-lottie {
  position: relative;
  z-index: 2;
  width: 68%;
  height: 68%;
}

.initial-loader-center h2 {
  margin: 0;
  color: #fff;
  font-size: clamp(16px, 2vw, 21px);
  line-height: 1.4;
  font-weight: 850;
  letter-spacing: 0.02em;
}

.app.lang-fa .initial-loader-center h2 {
  letter-spacing: 0;
}

.initial-loader-progress-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  align-items: center;
  gap: 12px;
  width: min(420px, 100%);
  margin: 20px auto 12px;
}

.initial-loader-progress {
  direction: ltr;
  width: 100%;
  height: 7px;
  overflow: hidden;
  border-radius: 999px;
  background: rgba(120, 146, 176, 0.15);
  border: 1px solid rgba(88, 216, 255, 0.11);
}

.initial-loader-progress span {
  display: block;
  width: 100%;
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, #58d8ff, #72f2cb, #7469ff);
  transform: scaleX(0);
  transform-origin: left center;
  will-change: transform;
}

.initial-loader-percent {
  min-width: 44px;
  color: #fff;
  font-size: 13px;
  font-weight: 900;
  font-variant-numeric: tabular-nums;
  letter-spacing: 0.04em;
  text-align: end;
}

.initial-loader-leave-active {
  transition: opacity 0.4s ease;
}

.initial-loader-leave-to {
  opacity: 0;
}

@media (max-width: 420px) {
  .initial-loader-progress-row {
    gap: 9px;
  }

  .initial-loader-percent {
    min-width: 40px;
    font-size: 12px;
  }

  .initial-loader-progress {
    height: 6px;
  }
}

@media (max-height: 540px) {
  .initial-loader-animation {
    width: 100px;
    height: 100px;
  }

  .initial-loader-center {
    padding: 18px 22px;
  }
}

/* =========================================================
   BACKGROUND
========================================================= */
.bg-base,
.bg-orb {
  position: fixed;
  pointer-events: none;
}

.bg-base {
  inset: 0;
  z-index: -20;
  background:
    radial-gradient(
      circle at 50% 28%,
      rgba(74, 194, 250, 0.045),
      transparent 44%
    ),
    linear-gradient(180deg, #07111f 0%, #081422 52%, #07101c 100%);
}

.app.light .bg-base {
  background:
    radial-gradient(
      circle at 50% 28%,
      rgba(60, 140, 220, 0.035),
      transparent 44%
    ),
    linear-gradient(180deg, #fbfcfe 0%, #f7f9fc 52%, #f1f4f8 100%);
}

/* gradient only: no blur filter, much cheaper to paint */
.bg-orb {
  z-index: -18;
  width: 420px;
  height: 420px;
  border-radius: 50%;
}

.bg-orb-1 {
  top: 2%;
  left: -180px;
  background: radial-gradient(
    circle,
    rgba(87, 212, 255, 0.07),
    transparent 70%
  );
}

.bg-orb-2 {
  top: 20%;
  right: -180px;
  background: radial-gradient(
    circle,
    rgba(116, 105, 255, 0.06),
    transparent 70%
  );
}

.app.light .bg-orb {
  opacity: 0.6;
}

.mouse-trail-layer {
  position: fixed;
  inset: 0;
  z-index: 90;
  pointer-events: none;
  overflow: hidden;
  contain: layout paint style;
}

.trail-star {
  position: fixed;
  z-index: 1;
  border-radius: 50%;
  pointer-events: none;
  opacity: 0;
  transform: translate3d(-50%, -50%, 0) scale(0.5) rotate(0deg);
  box-shadow:
    0 0 5px var(--glow),
    0 0 12px var(--glow),
    0 0 24px color-mix(in srgb, var(--glow) 55%, transparent);
  will-change: transform, opacity;
  animation: trailSpark var(--trail-duration, 460ms)
    cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

/* =========================================================
   LAYOUT
========================================================= */
.container {
  position: relative;
  z-index: 2;
  width: min(1240px, calc(100% - 32px));
  margin-inline: auto;
}

.section {
  position: relative;
  z-index: 2;
  padding-block: clamp(76px, 9vw, 118px);
}

.section-auto {
  content-visibility: auto;
  contain-intrinsic-size: auto 700px;
}

.full-screen-section {
  min-height: calc(100dvh - 74px);
  display: flex;
  align-items: center;
}

.section-heading {
  max-width: 880px;
  margin: 0 auto 56px;
  text-align: center;
}

.section-badge {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 9px 14px;
  margin-bottom: 15px;
  border-radius: 999px;
  background: var(--surface);
  border: 1px solid var(--border-strong);
  color: var(--primary);
  font-size: 18px;
  font-weight: 800;
  backdrop-filter: blur(10px);
}

.section-badge::before {
  content: "";
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: var(--primary);
}

.section-heading h2,
.about-main h2,
.showcase-content h2,
.contact-shell h2 {
  margin: 0 0 16px;
  color: var(--text);
  font-size: clamp(34px, 4vw, 58px);
  line-height: 1.1;
  font-weight: 850;
  letter-spacing: -0.03em;
}

.contact-shell h2 {
  font-size: clamp(30px, 3.4vw, 46px);
  line-height: 1.2;
}

.app.lang-fa .section-heading h2,
.app.lang-fa .about-main h2,
.app.lang-fa .showcase-content h2,
.app.lang-fa .contact-shell h2 {
  letter-spacing: 0;
}

.section-heading p,
.about-main p,
.showcase-content > p,
.contact-shell p {
  margin: 0;
  color: var(--text-soft);
  font-size: 16px;
  line-height: 1.9;
}

/* =========================================================
   NAVBAR
========================================================= */
.navbar {
  position: fixed;
  inset: 0 0 auto 0;
  z-index: 60;
  padding: 8px 0;
}

.nav-shell {
  min-height: 58px;
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto minmax(0, 1fr);
  align-items: center;
  gap: 12px;
  padding: 6px 8px;
  border: 1px solid var(--border);
  border-radius: 18px;
  background:
    linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.07),
      rgba(255, 255, 255, 0.025)
    ),
    var(--nav-bg);
  box-shadow:
    0 18px 45px rgba(0, 0, 0, 0.13),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(22px);
  -webkit-backdrop-filter: blur(22px);
}

.app.light .nav-shell {
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.96),
    rgba(245, 248, 252, 0.86)
  );
  box-shadow:
    0 18px 45px rgba(56, 77, 105, 0.08),
    inset 0 1px 0 rgba(255, 255, 255, 0.92);
}

.nav-col {
  display: flex;
  align-items: center;
  min-width: 0;
}

.nav-col-start {
  grid-column: 1;
  justify-content: flex-start;
}

.nav-col-center {
  grid-column: 2;
  justify-content: center;
}

.nav-col-end {
  grid-column: 3;
  justify-content: flex-end;
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
  color: var(--text);
}

.brand-mark {
  position: relative;
  width: 40px;
  height: 40px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
  overflow: hidden;
  isolation: isolate;
  contain: paint;
  border-radius: 12px;
  box-shadow: 0 8px 22px rgba(0, 0, 0, 0.2);
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.35s ease;
}

.brand-mark::before {
  content: "";
  position: absolute;
  inset: -60%;
  z-index: -2;
  background: conic-gradient(
    from 0deg,
    transparent 0 58%,
    var(--primary) 80%,
    var(--primary-2) 92%,
    transparent 100%
  );
  animation: codeMarkSpin 3.8s linear infinite;
  will-change: transform;
}

.brand-mark::after {
  content: "";
  position: absolute;
  inset: 1.25px;
  z-index: -1;
  border-radius: 10.75px;
  background:
    radial-gradient(
      circle at 28% 16%,
      rgba(88, 216, 255, 0.2),
      transparent 56%
    ),
    linear-gradient(145deg, #15202f, #0a111c);
}

.app.light .brand-mark {
  box-shadow: 0 8px 20px rgba(56, 77, 105, 0.14);
}

.app.light .brand-mark::after {
  background:
    radial-gradient(
      circle at 28% 16%,
      rgba(11, 158, 215, 0.14),
      transparent 56%
    ),
    linear-gradient(145deg, #ffffff, #e9f0f8);
}

.brand:hover .brand-mark,
.brand:focus-visible .brand-mark {
  transform: translateY(-2px) rotate(-4deg);
  box-shadow: 0 12px 28px rgba(88, 216, 255, 0.22);
}

.code-mark-svg {
  position: relative;
  width: 80%;
  height: 80%;
  overflow: visible;
}

.code-mark-aurora {
  animation: codeAurora 7s ease-in-out infinite alternate;
}

.code-mark-rain line {
  stroke: var(--primary);
  stroke-width: 1.4;
  stroke-linecap: round;
  stroke-dasharray: 1.5 7;
  opacity: 0.28;
  animation: codeRain 2.8s linear infinite;
}

.code-mark-rain line:nth-child(2) {
  opacity: 0.18;
  animation-duration: 4.2s;
  animation-direction: reverse;
}

.code-mark-rain line:nth-child(3) {
  animation-duration: 3.4s;
  animation-delay: -1.1s;
}

.code-mark-shift {
  transform-box: fill-box;
  transform-origin: center;
  transition: translate 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
  animation: codeBreathe 4.2s ease-in-out infinite;
}

.shift-left {
  --shift: -1.6px;
}

.shift-right {
  --shift: 1.6px;
}

.brand:hover .shift-left,
.brand:focus-visible .shift-left {
  translate: -3px 0;
}

.brand:hover .shift-right,
.brand:focus-visible .shift-right {
  translate: 3px 0;
}

.code-mark-pulse {
  fill: none;
  stroke: var(--accent);
  stroke-width: 1.4;
  opacity: 0;
  transform-box: fill-box;
  transform-origin: center;
  animation: codePulse 4.2s ease-out infinite;
}

.code-mark-path {
  fill: none;
  stroke: url(#navCodeGradient);
  stroke-width: 4.2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 100;
  animation: codeDraw 4.2s cubic-bezier(0.65, 0, 0.35, 1) infinite both;
}

.code-mark-slash {
  stroke-width: 3.6;
  animation-delay: 0.45s;
}

.code-mark-right {
  animation-delay: 0.9s;
}

.code-mark-cursor {
  fill: var(--accent);
  animation: codeCursor 1s steps(1) infinite;
}

.code-mark-spark {
  fill: var(--primary);
  transform-box: fill-box;
  transform-origin: center;
  animation: codeSpark 2.6s ease-in-out infinite 1.2s;
}

.led-dot {
  fill: var(--accent);
}

.led-ring {
  fill: none;
  stroke: var(--accent);
  stroke-width: 1;
  transform-box: fill-box;
  transform-origin: center;
  animation: ledPing 2.2s ease-out infinite;
}

.code-mark-shine {
  opacity: 0;
  animation: codeShine 4.2s ease-in-out infinite;
}

.brand-copy {
  width: 168px;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.brand-copy strong {
  padding-block: 0.05em 0.22em;
  font-size: 14px;
  line-height: 1.4;
  white-space: nowrap;
  overflow: visible;
  color: transparent;
  background: linear-gradient(
    100deg,
    var(--text) 40%,
    var(--primary) 50%,
    var(--text) 60%
  );
  background-size: 260% 100%;
  background-position: 0 0;
  -webkit-background-clip: text;
  background-clip: text;
  animation: brandShimmer 7s ease-in-out infinite;
}

.brand-role {
  display: block;
  max-width: 100%;
  color: var(--muted);
  font-size: 9px;
  line-height: 1.25;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-links {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 3px;
  padding: 3px;
  border: 1px solid var(--border);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.025);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.035);
}

.app.light .nav-links {
  background: rgba(15, 23, 42, 0.025);
}

.nav-links a {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 32px;
  padding: 0 10px;
  border-radius: 10px;
  color: var(--text-soft);
  font-size: 12px;
  font-weight: 700;
  white-space: nowrap;
  transition:
    color 0.22s ease,
    background 0.22s ease,
    transform 0.22s ease;
}

.nav-links a:hover {
  color: var(--text);
  background: var(--surface-strong);
  transform: translateY(-1px);
}

.nav-links a::after {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 3px;
  width: 0;
  height: 2px;
  border-radius: 999px;
  transform: translateX(-50%);
  background: linear-gradient(90deg, var(--primary), var(--primary-2));
  transition: width 0.25s ease;
}

.nav-links a:hover::after {
  width: 22px;
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 6px;
  direction: ltr;
}

.switch-btn {
  position: relative;
  width: 98px;
  height: 34px;
  padding: 0;
  overflow: hidden;
  border: 1px solid var(--border);
  border-radius: 11px;
  background: transparent;
  color: var(--text);
  cursor: pointer;
  transition:
    transform 0.25s ease,
    border-color 0.25s ease,
    box-shadow 0.25s ease;
}

.switch-btn:hover {
  transform: translateY(-2px);
  border-color: var(--border-strong);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
}

.switch-bg {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.065),
      rgba(255, 255, 255, 0.018)
    ),
    var(--surface);
  backdrop-filter: blur(12px);
}

.switch-inner {
  position: relative;
  z-index: 2;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 6px 8px;
}

.switch-icon-wrap {
  width: 22px;
  height: 22px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
  border-radius: 7px;
  background: rgba(255, 255, 255, 0.08);
}

.switch-text {
  color: var(--text);
  font-size: 11px;
  font-weight: 800;
  white-space: nowrap;
  animation: textSlide 0.25s ease;
}

.hamburger-btn {
  position: relative;
  display: none;
  width: 36px;
  height: 36px;
  padding: 8px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  border: 1px solid var(--border);
  border-radius: 10px;
  background: var(--surface);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.05);
  cursor: pointer;
}

.hamburger-btn::after {
  content: "";
  position: absolute;
  inset: -8px;
}

.hamburger-btn span {
  display: block;
  width: 16px;
  height: 2px;
  border-radius: 999px;
  background: var(--text);
  transition:
    transform 0.3s ease,
    opacity 0.2s ease;
}

.hamburger-btn.active span:nth-child(1) {
  transform: translateY(6px) rotate(45deg);
}

.hamburger-btn.active span:nth-child(2) {
  opacity: 0;
}

.hamburger-btn.active span:nth-child(3) {
  transform: translateY(-6px) rotate(-45deg);
}

/* =========================================================
   MOBILE MENU (EN opens from the left, FA from the right)
========================================================= */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  z-index: 70;
  display: flex;
  align-items: stretch;
  justify-content: flex-start;
  direction: ltr;
  background: rgba(4, 10, 18, 0.58);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
}

.app.lang-fa .mobile-menu-overlay {
  justify-content: flex-end;
}

.app.light .mobile-menu-overlay {
  background: rgba(226, 234, 243, 0.58);
}

.mobile-menu-panel {
  width: min(390px, 88vw);
  height: 100dvh;
  padding: max(20px, env(safe-area-inset-top)) 21px
    max(21px, env(safe-area-inset-bottom));
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  direction: ltr;
  border: 1px solid var(--border);
  border-width: 0 1px 0 0;
  border-radius: 0 26px 26px 0;
  background: rgba(12, 22, 37, 0.97);
  box-shadow: 25px 0 70px rgba(0, 0, 0, 0.32);
}

.app.lang-fa .mobile-menu-panel {
  direction: rtl;
  border-width: 0 0 0 1px;
  border-radius: 26px 0 0 26px;
  box-shadow: -25px 0 70px rgba(0, 0, 0, 0.32);
}

.app.light .mobile-menu-panel {
  background: rgba(255, 255, 255, 0.96);
  box-shadow: 25px 0 70px rgba(55, 75, 105, 0.12);
}

.app.light.lang-fa .mobile-menu-panel {
  box-shadow: -25px 0 70px rgba(55, 75, 105, 0.12);
}

.mobile-menu-header {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding-bottom: 15px;
  border-bottom: 1px solid var(--border);
}

.mobile-menu-brand {
  display: flex;
  align-items: center;
  gap: 14px;
  min-width: 0;
}

.mobile-menu-mark {
  width: 45px;
  height: 45px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
}

.mobile-lottie {
  width: 45px;
  height: 45px;
}

.mobile-menu-brand strong {
  display: block;
  color: var(--text);
  font-size: 14px;
}

.mobile-menu-brand span {
  display: block;
  margin-top: 3px;
  color: var(--muted);
  font-size: 10px;
}

.mobile-close {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border: 1px solid var(--border);
  border-radius: 12px;
  background: var(--surface);
  color: var(--text);
  font-size: 23px;
  cursor: pointer;
}

.mobile-nav-links {
  flex: 1 1 auto;
  min-height: 0;
  display: flex;
  flex-direction: column;
  gap: 7px;
  padding: 16px 2px;
  overflow-y: auto;
  overscroll-behavior: contain;
}

.mobile-nav-links a {
  position: relative;
  min-height: 52px;
  display: flex;
  align-items: center;
  padding: 13px 14px;
  border: 1px solid transparent;
  border-radius: 15px;
  color: var(--text);
  font-size: 15px;
  font-weight: 700;
  transition:
    background 0.22s ease,
    border-color 0.22s ease,
    color 0.22s ease;
}

.mobile-nav-links a::after {
  content: "";
  position: absolute;
  inset-inline-end: 14px;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--primary);
  opacity: 0;
  transform: scale(0.5);
  transition: 0.22s ease;
}

.mobile-nav-links a:hover,
.mobile-nav-links a:focus-visible {
  border-color: var(--border);
  color: var(--primary);
  background: linear-gradient(
    90deg,
    rgba(88, 216, 255, 0.08),
    rgba(116, 105, 255, 0.05)
  );
}

.mobile-nav-links a:hover::after,
.mobile-nav-links a:focus-visible::after {
  opacity: 1;
  transform: scale(1);
}

.mobile-actions {
  flex: 0 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  padding-top: 13px;
  border-top: 1px solid var(--border);
}

.mobile-actions .switch-btn {
  width: 100%;
  height: 44px;
}

.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: opacity 0.28s ease;
}

.mobile-menu-enter-active .mobile-menu-panel,
.mobile-menu-leave-active .mobile-menu-panel {
  transition: transform 0.38s cubic-bezier(0.22, 1, 0.36, 1);
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
}

.mobile-menu-enter-from .mobile-menu-panel,
.mobile-menu-leave-to .mobile-menu-panel {
  transform: translateX(-100%);
}

.app.lang-fa .mobile-menu-enter-from .mobile-menu-panel,
.app.lang-fa .mobile-menu-leave-to .mobile-menu-panel {
  transform: translateX(100%);
}

/* =========================================================
   HERO
========================================================= */
.hero {
  position: relative;
  min-height: calc(100dvh - 74px);
  padding: 42px 0 34px;
  overflow: clip;
  isolation: isolate;
}

.hero::before {
  content: "";
  position: absolute;
  width: 440px;
  height: 440px;
  left: 48%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.05),
    transparent 72%
  );
  pointer-events: none;
}

.hero-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.05fr) minmax(370px, 0.95fr);
  gap: 44px;
  align-items: center;
}

.hero-left {
  position: relative;
  z-index: 5;
  align-self: center;
  min-width: 0;
}

.hero-right {
  position: relative;
  z-index: 2;
  min-width: 0;
}

.hero-chip {
  position: relative;
  z-index: 8;
  width: fit-content;
  max-width: 100%;
  min-height: 40px;
  display: inline-flex;
  align-items: center;
  gap: 9px;
  padding: 10px 15px;
  margin-bottom: 24px;
  border-radius: 999px;
  border: 1px solid var(--border-strong);
  background: var(--surface);
  color: var(--text);
  font-size: 13px;
  font-weight: 650;
  backdrop-filter: blur(12px);
}

.live-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent);
  animation: pulse 2s infinite;
}

.hero-title {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 8px 12px;
  margin: 0 0 18px;
  color: var(--text);
  font-size: clamp(46px, 6vw, 84px);
  line-height: 1;
  font-weight: 850;
  letter-spacing: -0.04em;
}

.hero-name {
  display: inline-block;
  padding: 0.04em 0.06em 0.16em;
  white-space: nowrap;
  font-weight: 900;
  letter-spacing: -0.045em;
  color: transparent;
  background: linear-gradient(
    120deg,
    #fff 0%,
    #a8ecff 34%,
    #968aff 67%,
    #ddd8ff 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
}

.app.light .hero-name {
  background: linear-gradient(
    120deg,
    #101827 0%,
    #119fd6 35%,
    #6656db 70%,
    #101827 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
}

.app.lang-fa .hero-title {
  line-height: 1.32;
  letter-spacing: 0;
}

.app.lang-fa .hero-name {
  padding-bottom: 0.24em;
  letter-spacing: 0;
}

.hero-subtitle {
  min-height: 48px;
  margin: 0 0 16px;
  color: var(--text);
  font-size: clamp(22px, 2.9vw, 38px);
  line-height: 1.25;
  font-weight: 600;
}

.typing-word,
.cursor {
  color: var(--primary);
}

.typing-word {
  font-weight: 850;
}

.cursor {
  animation: blink 1s infinite;
}

.hero-desc {
  max-width: 780px;
  margin: 0;
  color: var(--text-soft);
  font-size: 17px;
  line-height: 1.9;
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 11px;
  margin-top: 28px;
}

.floating-techs {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 27px;
}

.floating-techs span {
  padding: 8px 12px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: var(--surface);
  color: var(--text);
  font-size: 11px;
  font-weight: 650;
  backdrop-filter: blur(10px);
  transition:
    transform 0.25s ease,
    border-color 0.25s ease,
    color 0.25s ease;
}

.floating-techs span:hover {
  transform: translateY(-2px);
  border-color: var(--border-strong);
  color: var(--primary);
}

.premium-btn {
  position: relative;
  min-width: 178px;
  min-height: 54px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 9px;
  padding: 0 20px;
  overflow: hidden;
  border: 1px solid transparent;
  border-radius: 16px;
  transition:
    transform 0.3s ease,
    box-shadow 0.3s ease,
    border-color 0.3s ease;
}

.premium-btn:hover {
  transform: translateY(-3px);
}

.premium-btn.primary {
  color: var(--on-primary);
  background: linear-gradient(135deg, var(--primary), var(--primary-2));
  box-shadow: 0 15px 32px rgba(74, 113, 246, 0.18);
}

.premium-btn.ghost {
  color: var(--text);
  border-color: var(--border);
  background: var(--surface);
  backdrop-filter: blur(12px);
}

.premium-btn.ghost:hover {
  border-color: var(--border-strong);
}

.btn-liquid {
  position: absolute;
  inset: -50%;
  background:
    radial-gradient(
      circle at 20% 40%,
      rgba(255, 255, 255, 0.23),
      transparent 18%
    ),
    radial-gradient(
      circle at 72% 28%,
      rgba(255, 255, 255, 0.11),
      transparent 16%
    );
  transform: translateX(-18%);
  transition: transform 0.6s ease;
}

.premium-btn:hover .btn-liquid {
  transform: translateX(15%);
}

.btn-content,
.btn-arrow {
  position: relative;
  z-index: 2;
  font-size: 14px;
}

.btn-content {
  font-weight: 750;
}

.btn-arrow {
  transition: transform 0.25s ease;
}

.premium-btn:hover .btn-arrow {
  transform: translate(2px, -2px);
}

/* hero visual */
.glass-stage {
  position: relative;
  min-height: 560px;
  display: grid;
  place-items: center;
}

.stage-glow {
  position: absolute;
  width: 440px;
  height: 440px;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.075),
    rgba(116, 105, 255, 0.03) 42%,
    transparent 72%
  );
  animation: stageGlow 7s ease-in-out infinite;
}

.lottie-card {
  position: relative;
  z-index: 4;
  width: min(100%, 470px);
  min-height: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.hero-lottie {
  position: relative;
  z-index: 2;
  width: min(100%, 410px);
  height: 410px;
  display: grid;
  place-items: center;
  overflow: hidden;
}

.lottie-caption {
  position: relative;
  z-index: 3;
  width: min(100%, 350px);
  margin-top: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 12px 15px;
  border-radius: 16px;
  border: 1px solid var(--border);
  background: rgba(12, 22, 37, 0.82);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  box-shadow:
    0 12px 28px rgba(0, 0, 0, 0.16),
    inset 0 1px 0 rgba(255, 255, 255, 0.04);
}

.lottie-caption-dot {
  width: 8px;
  height: 8px;
  flex-shrink: 0;
  border-radius: 50%;
  background: var(--accent);
  animation: pulse 2s infinite;
}

.lottie-caption strong {
  display: block;
  color: #f5f8fd;
  font-size: 14px;
  font-weight: 800;
  line-height: 1.3;
}

.lottie-caption small {
  display: block;
  margin-top: 3px;
  color: #aebbd0;
  font-size: 10px;
  line-height: 1.5;
}

.app.light .lottie-caption {
  background: rgba(255, 255, 255, 0.86);
  box-shadow:
    0 12px 28px rgba(57, 77, 108, 0.08),
    inset 0 1px 0 rgba(255, 255, 255, 0.92);
}

.app.light .lottie-caption strong {
  color: #0f172a;
}

.app.light .lottie-caption small {
  color: #536176;
}

/* =========================================================
   SKILLS
========================================================= */
.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}

.skill-panel {
  min-height: 224px;
  padding: 29px;
  border: 1px solid var(--border);
  border-radius: 24px;
  background: var(--surface);
  transition:
    transform 0.32s ease,
    background 0.32s ease;
  transition-delay: var(--delay);
}

.skill-panel:hover {
  transform: translateY(-4px);
  background: var(--surface-strong);
}

.panel-line {
  width: 52px;
  height: 3px;
  margin-bottom: 20px;
  border-radius: 999px;
  background: linear-gradient(90deg, var(--primary), var(--primary-2));
  transition: width 0.3s ease;
}

.skill-panel:hover .panel-line {
  width: 72px;
}

.skill-head {
  display: flex;
  align-items: center;
  gap: 13px;
  margin-bottom: 13px;
}

.skill-icon {
  width: 50px;
  height: 50px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
  border-radius: 15px;
  border: 1px solid var(--border);
  background: var(--surface-strong);
  font-size: 21px;
  transition:
    transform 0.3s ease,
    border-color 0.3s ease;
}

.skill-panel:hover .skill-icon {
  transform: scale(1.05) rotate(-3deg);
  border-color: var(--border-strong);
}

.skill-head small {
  display: block;
  margin-bottom: 2px;
  color: var(--muted);
  font-size: 10px;
  font-weight: 800;
}

.skill-head h3 {
  margin: 0;
  color: var(--text);
  font-size: 22px;
  font-weight: 750;
}

.skill-panel p {
  margin: 0;
  color: var(--text-soft);
  font-size: 14px;
  line-height: 1.82;
}

/* =========================================================
   SERVICES
========================================================= */
.services-layout {
  display: grid;
  grid-template-columns: 0.72fr 1.28fr;
  gap: 35px;
  align-items: stretch;
}

.services-lottie-wrap {
  position: relative;
  min-height: 490px;
  display: grid;
  place-items: center;
}

.services-lottie-glow {
  position: absolute;
  width: 320px;
  height: 320px;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.11),
    transparent 70%
  );
}

.services-lottie-card {
  position: relative;
  width: min(100%, 390px);
  aspect-ratio: 1;
  display: grid;
  place-items: center;
  overflow: hidden;
  border-radius: 32px;
  border: 1px solid var(--border);
  background: var(--surface);
  box-shadow: var(--shadow);
}

.services-lottie {
  width: 86%;
  height: 86%;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
}

.service-card {
  position: relative;
  min-height: 230px;
  padding: 25px;
  overflow: hidden;
  border: 1px solid var(--border);
  border-radius: 24px;
  background: var(--surface);
  transition:
    transform 0.3s ease,
    border-color 0.3s ease,
    background 0.3s ease;
}

.service-card:hover {
  transform: translateY(-6px);
  border-color: var(--border-strong);
  background: var(--surface-strong);
}

.service-number {
  position: absolute;
  top: 20px;
  inset-inline-end: 20px;
  color: var(--muted);
  font-size: 10px;
  font-weight: 800;
}

.service-icon {
  width: 48px;
  height: 48px;
  display: grid;
  place-items: center;
  margin-bottom: 22px;
  border-radius: 15px;
  border: 1px solid var(--border);
  background: rgba(88, 216, 255, 0.08);
  color: var(--primary);
  font-size: 20px;
}

.service-card h3 {
  margin: 0 0 10px;
  color: var(--text);
  font-size: 19px;
}

.service-card p {
  margin: 0;
  color: var(--text-soft);
  font-size: 12px;
  line-height: 1.8;
}

.service-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 15px;
}

.service-tags span {
  padding: 5px 8px;
  border-radius: 999px;
  border: 1px solid var(--border);
  color: var(--muted);
  font-size: 10px;
}

/* =========================================================
   SHOWCASE
========================================================= */
.showcase-section {
  padding-block: clamp(76px, 8vw, 105px);
}

.showcase-grid {
  display: grid;
  grid-template-columns: 1fr 0.9fr;
  gap: 44px;
  align-items: center;
}

.showcase-media {
  position: relative;
  min-height: 480px;
  display: grid;
  align-items: center;
}

.showcase-image-frame {
  position: relative;
  overflow: hidden;
  border-radius: 28px;
  border: 1px solid var(--border);
  background: #101826;
  box-shadow: var(--shadow);
}

.showcase-image-frame img {
  width: 100%;
  height: 480px;
  object-fit: cover;
  opacity: 0.9;
  transform: scale(1.02);
  transition:
    transform 0.8s cubic-bezier(0.2, 0.8, 0.2, 1),
    opacity 0.5s ease;
}

.showcase-media:hover .showcase-image-frame img {
  transform: scale(1.055);
  opacity: 1;
}

.showcase-image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    180deg,
    rgba(6, 12, 20, 0.06),
    rgba(6, 12, 20, 0.42)
  );
}

.showcase-card {
  position: absolute;
  padding: 13px 15px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 16px;
  background: rgba(9, 16, 27, 0.82);
  backdrop-filter: blur(14px);
  animation: floatCard 5s ease-in-out infinite;
}

.showcase-card strong {
  display: block;
  color: #fff;
  font-size: 16px;
}

.showcase-card span {
  display: block;
  margin-top: 3px;
  color: #a8b4c7;
  font-size: 10px;
}

.showcase-card-1 {
  inset-inline-start: -8px;
  bottom: 35px;
}

.showcase-card-2 {
  inset-inline-end: -8px;
  top: 32px;
  animation-delay: -2.2s;
}

.app.light .showcase-card {
  background: rgba(255, 255, 255, 0.88);
  border-color: rgba(15, 23, 42, 0.08);
}

.app.light .showcase-card strong {
  color: #0f172a;
}

.app.light .showcase-card span {
  color: #667487;
}

.showcase-content {
  max-width: 610px;
}

.showcase-content > p {
  margin-bottom: 25px;
}

.showcase-points {
  display: grid;
  gap: 13px;
}

.showcase-points > div {
  display: flex;
  gap: 12px;
  padding: 14px 15px;
  border: 1px solid var(--border);
  border-radius: 16px;
  background: var(--surface);
  transition:
    transform 0.28s ease,
    border-color 0.28s ease;
}

.showcase-points > div:hover {
  transform: translateX(4px);
  border-color: var(--border-strong);
}

.showcase-point-icon {
  width: 34px;
  height: 34px;
  flex-shrink: 0;
  display: grid;
  place-items: center;
  border-radius: 10px;
  background: rgba(88, 216, 255, 0.08);
  color: var(--primary);
  font-size: 10px;
  font-weight: 850;
}

.showcase-points strong {
  display: block;
  color: var(--text);
  font-size: 13px;
}

.showcase-points p {
  margin: 3px 0 0;
  color: var(--text-soft);
  font-size: 12px;
  line-height: 1.7;
}

/* =========================================================
   ABOUT
========================================================= */
.about-grid {
  display: grid;
  grid-template-columns: 1.08fr 0.92fr;
  gap: 28px;
  align-items: start;
}

.about-main p {
  margin-bottom: 15px;
}

.about-points {
  display: grid;
  gap: 11px;
  margin-top: 24px;
}

.about-point {
  display: flex;
  align-items: center;
  gap: 11px;
  padding: 13px 14px;
  border: 1px solid var(--border);
  border-radius: 16px;
  background: var(--surface);
  transition:
    transform 0.25s ease,
    border-color 0.25s ease;
  transition-delay: var(--delay);
}

.about-point:hover {
  transform: translateX(3px);
  border-color: var(--border-strong);
}

.about-point > span {
  width: 29px;
  height: 29px;
  flex-shrink: 0;
  display: grid;
  place-items: center;
  border-radius: 50%;
  background: rgba(89, 216, 255, 0.09);
  color: var(--primary);
  font-size: 11px;
}

.about-main .about-point p {
  margin: 0;
  color: var(--text);
  font-size: 13px;
}

.about-side {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
}

.about-card {
  min-height: 190px;
  padding: 22px;
  border: 1px solid var(--border);
  border-radius: 23px;
  background: var(--surface);
  transition:
    transform 0.32s ease,
    border-color 0.32s ease,
    background 0.32s ease;
  transition-delay: var(--delay);
}

.about-card:hover {
  transform: translateY(-6px);
  border-color: var(--border-strong);
}

.about-card.accent {
  background: linear-gradient(
    135deg,
    rgba(88, 216, 255, 0.075),
    rgba(116, 105, 255, 0.08)
  );
}

.about-card small {
  display: block;
  margin-bottom: 23px;
  color: var(--muted);
  font-size: 10px;
  font-weight: 850;
}

.about-card h3 {
  margin: 0 0 10px;
  color: var(--text);
  font-size: 20px;
}

.about-card p {
  margin: 0;
  color: var(--text-soft);
  font-size: 13px;
  line-height: 1.8;
}

/* =========================================================
   PROJECTS
========================================================= */
.pc-toolbar {
  display: flex;
  justify-content: center;
  margin: -16px 0 34px;
}

.pc-filters {
  display: inline-flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 4px;
  padding: 5px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: var(--surface);
  backdrop-filter: blur(10px);
}

.pc-filter {
  min-height: 40px;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 0 16px;
  border: 0;
  border-radius: 999px;
  background: transparent;
  color: var(--text-soft);
  font-size: 13px;
  font-weight: 700;
  cursor: pointer;
  transition:
    color 0.2s ease,
    background 0.2s ease;
}

.pc-filter:hover {
  color: var(--text);
  background: var(--surface-strong);
}

.pc-filter.active {
  color: var(--on-primary);
  background: linear-gradient(135deg, var(--primary), var(--primary-2));
}

.pc-filter-count {
  min-width: 22px;
  padding: 1px 7px;
  border-radius: 999px;
  background: rgba(127, 127, 127, 0.18);
  font-size: 11px;
  line-height: 18px;
  text-align: center;
}

.pc-filter.active .pc-filter-count {
  background: rgba(0, 0, 0, 0.2);
}

.projects-showcase {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 20px;
  align-items: stretch;
}

.pc {
  --pc-rgb: 88, 216, 255;

  position: relative;
  min-width: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border: 1px solid var(--border);
  border-radius: 22px;
  background: var(--surface);
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    border-color 0.3s ease,
    box-shadow 0.3s ease;
}

.pc:hover,
.pc:focus-within {
  transform: translateY(-6px);
  border-color: rgba(var(--pc-rgb), 0.55);
  box-shadow:
    0 24px 50px rgba(0, 0, 0, 0.22),
    0 0 0 1px rgba(var(--pc-rgb), 0.14);
}

.app.light .pc:hover,
.app.light .pc:focus-within {
  box-shadow:
    0 24px 50px rgba(57, 77, 108, 0.16),
    0 0 0 1px rgba(var(--pc-rgb), 0.18);
}

.pc-media {
  position: relative;
  aspect-ratio: 16 / 10;
  overflow: hidden;
  isolation: isolate;
  background: #0b1421;
}

.pc-media img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transform: scale(1.04);
  transition: transform 0.8s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.pc:hover .pc-media img {
  transform: scale(1.1);
}

.pc-shade {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(
      180deg,
      rgba(5, 10, 18, 0.5) 0%,
      rgba(5, 10, 18, 0.05) 38%,
      rgba(5, 10, 18, 0.86) 100%
    ),
    linear-gradient(135deg, rgba(var(--pc-rgb), 0.4), transparent 62%);
}

.pc-bar {
  position: absolute;
  top: 10px;
  left: 10px;
  right: 10px;
  height: 30px;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 11px;
  direction: ltr;
  border: 1px solid rgba(255, 255, 255, 0.13);
  border-radius: 10px;
  background: rgba(7, 17, 31, 0.58);
  backdrop-filter: blur(8px);
}

.pc-dots {
  display: flex;
  gap: 5px;
  flex-shrink: 0;
}

.pc-dots i {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
}

.pc-url {
  min-width: 0;
  overflow: hidden;
  color: #d6e1f1;
  font: 500 11px / 1 var(--font-mono);
  white-space: nowrap;
  text-overflow: ellipsis;
}

.pc-overlay {
  position: absolute;
  left: 16px;
  right: 16px;
  bottom: 14px;
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 12px;
}

.pc-preview-title {
  color: #fff;
  font-family: var(--font-display);
  font-size: clamp(16px, 1.5vw, 19px);
  line-height: 1.3;
  text-shadow: 0 2px 14px rgba(0, 0, 0, 0.45);
}

.pc-status {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  flex-shrink: 0;
  padding: 6px 11px;
  border: 1px solid rgba(255, 255, 255, 0.18);
  border-radius: 999px;
  background: rgba(7, 17, 31, 0.62);
  backdrop-filter: blur(8px);
  color: #fff;
  font-size: 11px;
  font-weight: 800;
}

.pc-status i,
.pc-soon i {
  width: 7px;
  height: 7px;
  flex-shrink: 0;
  border-radius: 50%;
  background: #f5b942;
}

.pc-status.live i {
  background: var(--accent);
  animation: pulse 2s infinite;
}

.pc-body {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 22px;
}

.pc-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.pc-number {
  color: rgb(var(--pc-rgb));
  font: 700 12px / 1 var(--font-mono);
}

.app.light .pc-number {
  color: var(--primary);
}

.pc-category {
  color: var(--muted);
  font-size: 12px;
  font-weight: 650;
}

.pc-body h3 {
  margin: 0;
  color: var(--text);
  font-size: 21px;
  line-height: 1.3;
  font-weight: 750;
}

.pc-body > p {
  margin: 0;
  color: var(--text-soft);
  font-size: 14px;
  line-height: 1.85;
}

.pc-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.pc-chips li {
  padding: 5px 11px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: var(--surface-soft);
  color: var(--text);
  font-size: 12px;
  font-weight: 650;
}

.pc-footer {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-top: auto;
  padding-top: 16px;
  border-top: 1px solid var(--border);
}

.pc-role {
  color: var(--muted);
  font-size: 12px;
  font-weight: 650;
}

.pc-cta {
  min-height: 40px;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 0 16px;
  border-radius: 12px;
  background: linear-gradient(135deg, var(--primary), var(--primary-2));
  color: var(--on-primary);
  font-size: 13px;
  font-weight: 800;
  transition:
    transform 0.25s ease,
    box-shadow 0.25s ease;
}

.pc-cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 24px rgba(88, 216, 255, 0.22);
}

.pc-soon {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: var(--text-soft);
  font-size: 12px;
  font-weight: 700;
}

.pc-enter-active {
  transition:
    opacity 0.45s ease,
    transform 0.45s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.pc-enter-from {
  opacity: 0;
  transform: translateY(16px) scale(0.98);
}

.pc-leave-active {
  display: none;
}

.pc-move {
  transition: transform 0.4s ease;
}

/* =========================================================
   PROCESS
========================================================= */
.process-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
}

.process-card {
  min-height: 235px;
  padding: 22px;
  border: 1px solid var(--border);
  border-radius: 22px;
  background: var(--surface);
  transition:
    transform 0.3s ease,
    border-color 0.3s ease;
  transition-delay: var(--delay);
}

.process-card:hover {
  transform: translateY(-6px);
  border-color: var(--border-strong);
}

.process-top {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 30px;
}

.process-top span {
  color: var(--primary);
  font-size: 13px;
  font-weight: 850;
}

.process-top div {
  flex: 1;
  height: 1px;
  background: var(--border);
}

.process-card h3 {
  margin: 0 0 9px;
  color: var(--text);
  font-size: 17px;
}

.process-card p {
  margin: 0;
  color: var(--text-soft);
  font-size: 11px;
  line-height: 1.8;
}

/* =========================================================
   CONTACT
========================================================= */
.contact-shell {
  position: relative;
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.9fr);
  align-items: center;
  gap: clamp(24px, 5vw, 64px);
  overflow: hidden;
  padding: clamp(32px, 5vw, 64px);
  border: 1px solid var(--border);
  border-radius: 30px;
  background: var(--surface);
}

.contact-light {
  position: absolute;
  inset-inline-end: 3%;
  top: 50%;
  width: 480px;
  height: 480px;
  margin-top: -240px;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.13),
    transparent 68%
  );
  pointer-events: none;
  animation: stageGlow 8s ease-in-out infinite;
}

.contact-content {
  position: relative;
  z-index: 2;
  min-width: 0;
  text-align: start;
}

.contact-shell p {
  max-width: 520px;
}

.contact-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 30px;
}

.contact-lottie-wrap {
  position: relative;
  z-index: 2;
  width: min(100%, 340px);
  aspect-ratio: 1;
  margin-inline: auto;
}

.contact-lottie {
  width: 100%;
  height: 100%;
}

/* =========================================================
   FOOTER
========================================================= */
.site-footer {
  position: relative;
  z-index: 2;
  border-top: 1px solid var(--border);
  background: var(--surface-soft);
}

.site-footer-inner {
  min-height: 74px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}

.site-footer p {
  margin: 0;
  color: var(--muted);
  font-size: 11px;
  line-height: 1.7;
}

.site-footer nav {
  display: flex;
  align-items: center;
  gap: 16px;
}

.site-footer nav a {
  color: var(--text-soft);
  font-size: 11px;
  font-weight: 700;
  transition: color 0.2s ease;
}

.site-footer nav a:hover {
  color: var(--primary);
}

/* =========================================================
   REVEAL
========================================================= */
.reveal {
  opacity: 0;
  transform: translateY(25px);
  transition:
    opacity 0.75s cubic-bezier(0.22, 1, 0.36, 1),
    transform 0.75s cubic-bezier(0.22, 1, 0.36, 1);
  transition-delay: var(--delay, 0ms);
}

.reveal-left {
  transform: translate(-25px, 8px);
}

.reveal-right {
  transform: translate(25px, 8px);
}

.app.lang-fa .reveal-left {
  transform: translate(25px, 8px);
}

.app.lang-fa .reveal-right {
  transform: translate(-25px, 8px);
}

.app .reveal.in-view {
  opacity: 1;
  transform: none;
}

/* =========================================================
   KEYFRAMES
========================================================= */
@keyframes trailSpark {
  0% {
    opacity: 0;
    transform: translate3d(-50%, -50%, 0) scale(0.25)
      rotate(var(--star-rotation, 0deg));
  }

  12% {
    opacity: 0.92;
  }

  50% {
    opacity: 0.62;
    transform: translate3d(
        calc(-50% + var(--drift-x, 0px) * 0.45),
        calc(-50% + var(--drift-y, 0px) * 0.45),
        0
      )
      scale(1) rotate(calc(var(--star-rotation, 0deg) * 0.5));
  }

  100% {
    opacity: 0;
    transform: translate3d(
        calc(-50% + var(--drift-x, 0px)),
        calc(-50% + var(--drift-y, 0px)),
        0
      )
      scale(1.65) rotate(var(--star-rotation, 0deg));
  }
}

@keyframes loaderRingSpin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes codeMarkSpin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes codeDraw {
  0% {
    stroke-dashoffset: 100;
    opacity: 0;
  }
  5% {
    opacity: 1;
  }
  26% {
    stroke-dashoffset: 0;
  }
  80% {
    stroke-dashoffset: 0;
    opacity: 1;
  }
  94% {
    stroke-dashoffset: 0;
    opacity: 0;
  }
  100% {
    stroke-dashoffset: 100;
    opacity: 0;
  }
}

@keyframes codeBreathe {
  0%,
  28%,
  80%,
  100% {
    transform: translateX(0);
  }
  50%,
  62% {
    transform: translateX(var(--shift, 0));
  }
}

@keyframes codePulse {
  0%,
  30% {
    opacity: 0;
    transform: scale(0.4);
  }
  33% {
    opacity: 0.75;
  }
  58%,
  100% {
    opacity: 0;
    transform: scale(3.4);
  }
}

@keyframes codeAurora {
  to {
    transform: translate(26px, 30px);
  }
}

@keyframes codeRain {
  to {
    stroke-dashoffset: -17;
  }
}

@keyframes brandShimmer {
  0%,
  35% {
    background-position: 100% 0;
  }
  70%,
  100% {
    background-position: 0 0;
  }
}

@keyframes codeShine {
  0%,
  58% {
    opacity: 0;
    transform: translateX(0);
  }
  62% {
    opacity: 1;
  }
  78% {
    opacity: 1;
    transform: translateX(124px);
  }
  80%,
  100% {
    opacity: 0;
    transform: translateX(124px);
  }
}

@keyframes ledPing {
  0% {
    opacity: 0.9;
    transform: scale(0.8);
  }
  80%,
  100% {
    opacity: 0;
    transform: scale(2.8);
  }
}

@keyframes codeCursor {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
    opacity: 0;
  }
}

@keyframes codeSpark {
  0%,
  100% {
    transform: scale(0.4);
    opacity: 0.2;
  }
  50% {
    transform: scale(1.25);
    opacity: 1;
  }
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(114, 244, 202, 0.45);
  }
  70% {
    box-shadow: 0 0 0 9px rgba(114, 244, 202, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(114, 244, 202, 0);
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

@keyframes textSlide {
  from {
    opacity: 0;
    transform: translateY(4px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes stageGlow {
  0%,
  100% {
    transform: scale(0.97);
    opacity: 0.6;
  }
  50% {
    transform: scale(1.03);
    opacity: 0.92;
  }
}

@keyframes floatCard {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-8px);
  }
}

/* =========================================================
   RESPONSIVE — one ordered ladder, widest to narrowest.
   Later rules intentionally win (same cascade the old
   "FINAL ..." override blocks produced).
========================================================= */
@media (max-width: 1140px) {
  .nav-shell {
    grid-template-columns: minmax(0, 1fr) auto;
  }

  .nav-col-center,
  .nav-actions {
    display: none;
  }

  .nav-col-end {
    grid-column: 2;
    flex: 0 0 auto;
  }

  .hamburger-btn {
    display: flex;
  }

  .brand-copy {
    width: auto;
    max-width: min(260px, 62vw);
  }

  .projects-showcase {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 1100px) {
  .container {
    width: min(100% - 28px, 1080px);
  }

  .hero-grid {
    grid-template-columns: minmax(0, 1fr) minmax(320px, 0.82fr);
    gap: 28px;
  }

  .hero-title {
    font-size: clamp(42px, 6vw, 66px);
  }

  .hero-desc {
    font-size: 15px;
  }

  .glass-stage {
    min-height: 500px;
  }

  .lottie-card {
    min-height: 450px;
  }

  .hero-lottie {
    width: min(100%, 370px);
    height: 370px;
  }
}

@media (max-width: 980px) {
  .hero-grid,
  .showcase-grid,
  .about-grid,
  .services-layout {
    grid-template-columns: minmax(0, 1fr);
  }

  .hero-grid {
    gap: 36px;
  }

  .hero-left {
    text-align: center;
  }

  .hero-chip {
    margin-inline: auto;
  }

  .hero-title,
  .hero-actions,
  .floating-techs {
    justify-content: center;
  }

  .hero-desc {
    margin-inline: auto;
  }

  .hero-right {
    width: 100%;
    max-width: 520px;
    margin-inline: auto;
  }

  .glass-stage {
    min-height: 480px;
  }

  .skills-grid,
  .process-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .showcase-content {
    max-width: none;
  }

  .services-lottie-wrap {
    min-height: 380px;
  }
}

@media (max-width: 900px) {
  .section-heading {
    max-width: 720px;
    margin-bottom: 42px;
  }

  .section-heading h2,
  .about-main h2,
  .showcase-content h2,
  .contact-shell h2 {
    font-size: clamp(29px, 5vw, 46px);
  }

  .showcase-media {
    min-height: 400px;
  }

  .showcase-image-frame img {
    height: min(58vw, 420px);
  }
}

@media (max-width: 860px) {
  .contact-shell {
    grid-template-columns: minmax(0, 1fr);
    gap: 20px;
    text-align: center;
  }

  .contact-content {
    text-align: center;
  }

  .contact-shell p {
    margin-inline: auto;
  }

  .contact-actions {
    justify-content: center;
  }

  .contact-lottie-wrap {
    order: -1;
    width: min(58vw, 220px);
  }

  .contact-light {
    inset-inline-end: auto;
    left: 50%;
    top: -40px;
    margin: 0 0 0 -240px;
  }
}

@media (max-width: 760px) {
  .app {
    padding-top: 78px;
  }

  .container {
    width: calc(100% - 24px);
  }

  .section {
    padding-block: 68px;
  }

  .full-screen-section,
  .hero {
    min-height: auto;
  }

  /* nav */
  .navbar {
    padding: 7px 0;
  }

  .nav-shell {
    min-height: 58px;
    padding: 6px 8px;
    gap: 8px;
    border-radius: 17px;
  }

  .brand {
    gap: 8px;
  }

  .brand-mark {
    width: 35px;
    height: 35px;
    border-radius: 10px;
  }

  .brand-copy strong {
    font-size: clamp(11px, 3.1vw, 13px);
  }

  .brand-role {
    font-size: 8px;
  }

  /* hero */
  .hero {
    padding: 28px 0 42px;
  }

  .hero-grid {
    gap: 34px;
  }

  .hero-chip {
    max-width: min(100%, 420px);
    justify-content: center;
  }

  .hero-title {
    gap: 6px 9px;
    font-size: clamp(28px, 8vw, 48px);
    line-height: 1.12;
  }

  .hero-subtitle {
    min-height: 0;
    font-size: clamp(17px, 4.7vw, 25px);
  }

  .hero-desc {
    max-width: 620px;
    font-size: 13px;
    line-height: 1.95;
  }

  .hero-actions {
    gap: 10px;
  }

  .premium-btn {
    min-height: 50px;
  }

  .hero-right {
    max-width: none;
    margin: 0;
  }

  .glass-stage {
    min-height: 0;
    padding: 8px 0 0;
  }

  .stage-glow {
    width: 250px;
    height: 250px;
  }

  .lottie-card {
    width: 100%;
    min-height: 0;
  }

  .hero-lottie {
    width: min(100%, 360px);
    height: min(78vw, 350px);
    min-height: 250px;
  }

  .lottie-caption {
    width: min(100%, 360px);
    margin-top: 10px;
    padding: 10px 13px;
  }

  /* grids */
  .skills-grid,
  .services-grid,
  .process-grid,
  .about-side,
  .projects-showcase {
    grid-template-columns: 1fr;
  }

  .skill-panel,
  .about-card,
  .process-card,
  .service-card {
    border-radius: 20px;
  }

  .services-layout {
    gap: 26px;
  }

  .services-lottie-wrap {
    min-height: 240px;
  }

  .services-lottie-card {
    width: min(230px, 70vw);
  }

  .showcase-media {
    min-height: 0;
  }

  .showcase-image-frame {
    border-radius: 22px;
  }

  .showcase-image-frame img {
    height: auto;
    min-height: 250px;
    aspect-ratio: 16 / 10;
  }

  .showcase-card-1 {
    inset-inline-start: 10px;
    bottom: 12px;
  }

  .showcase-card-2 {
    inset-inline-end: 10px;
    top: 12px;
  }

  /* touch devices: no hover lift */
  .premium-btn:hover,
  .skill-panel:hover,
  .service-card:hover,
  .about-card:hover,
  .process-card:hover,
  .showcase-points > div:hover,
  .pc:hover {
    transform: none;
  }

  /* ---- performance on mobile ---- */
  .bg-orb {
    display: none;
  }

  .nav-shell {
    background: var(--nav-bg);
  }

  .app.light .nav-shell {
    background: rgba(255, 255, 255, 0.96);
  }

  .nav-shell,
  .mobile-menu-overlay,
  .section-badge,
  .hero-chip,
  .floating-techs span,
  .premium-btn.ghost,
  .switch-bg,
  .lottie-caption,
  .showcase-card,
  .pc-filters,
  .pc-bar,
  .pc-status {
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }

  .code-mark-rain,
  .code-mark-aurora,
  .code-mark-shine {
    display: none;
  }

  .brand-copy strong {
    animation: none;
    color: var(--text);
    background: none;
  }
}

@media (max-width: 640px) {
  .site-footer-inner {
    min-height: 90px;
    flex-direction: column;
    justify-content: center;
    text-align: center;
  }

  .site-footer nav {
    gap: 12px;
    flex-wrap: wrap;
    justify-content: center;
  }
}

@media (max-width: 560px) {
  .container {
    width: calc(100% - 18px);
  }

  .section {
    padding-block: 58px;
  }

  .section-heading {
    margin-bottom: 30px;
  }

  .section-heading h2,
  .about-main h2,
  .showcase-content h2,
  .contact-shell h2 {
    font-size: clamp(24px, 7.1vw, 34px);
    line-height: 1.22;
  }

  .section-heading p,
  .about-main p,
  .showcase-content > p,
  .contact-shell p {
    font-size: 12.5px;
    line-height: 1.85;
  }

  .skill-panel,
  .about-card {
    padding: 18px;
  }

  .skill-head h3 {
    font-size: 18px;
  }

  .skill-panel p,
  .about-card p,
  .showcase-points p {
    font-size: 12px;
  }

  .service-card {
    min-height: 205px;
    padding: 19px;
  }

  .hero {
    padding-top: 22px;
  }

  .hero-chip {
    width: 100%;
    padding: 8px 10px;
    font-size: 10px;
    line-height: 1.45;
  }

  .hero-title {
    font-size: clamp(23px, 8.5vw, 34px);
  }

  .hero-name {
    white-space: normal;
  }

  .hero-subtitle {
    font-size: 15px;
    line-height: 1.45;
  }

  .hero-desc {
    font-size: 12px;
  }

  .hero-actions,
  .contact-actions {
    flex-direction: column;
    align-items: stretch;
  }

  .premium-btn {
    width: 100%;
    min-width: 0;
  }

  .floating-techs {
    gap: 6px;
  }

  .floating-techs span {
    padding: 7px 9px;
    font-size: 9px;
  }

  .hero-lottie {
    width: min(100%, 320px);
    height: min(78vw, 300px);
    min-height: 230px;
  }

  .lottie-caption {
    width: calc(100% - 14px);
    margin-top: 7px;
    padding: 9px 10px;
    border-radius: 13px;
  }

  .lottie-caption strong {
    font-size: 11px;
  }

  .lottie-caption small {
    font-size: 9px;
  }

  .showcase-card {
    padding: 10px 11px;
    border-radius: 13px;
  }

  .showcase-card strong {
    font-size: 12px;
  }

  .showcase-card span {
    font-size: 9px;
  }

  .mobile-menu-panel {
    padding: 17px;
  }
}

@media (max-width: 460px) {
  .mobile-actions {
    grid-template-columns: 1fr;
  }

  .mobile-actions .switch-btn {
    height: 43px;
  }
}

@media (max-width: 390px) {
  .container {
    width: calc(100% - 14px);
  }

  .app {
    padding-top: 74px;
  }

  .nav-shell {
    padding-inline: 6px;
    border-radius: 16px;
  }

  .brand {
    gap: 7px;
  }

  .brand-mark {
    width: 32px;
    height: 32px;
  }

  .brand-copy strong {
    font-size: 10.5px;
  }

  .brand-role {
    font-size: 8px;
  }

  .hero-lottie {
    min-height: 210px;
    height: 67vw;
  }

  .lottie-caption {
    max-width: 300px;
  }
}

@media (max-width: 360px) {
  .brand-role {
    display: none;
  }

  .mobile-menu-panel {
    padding: 14px;
  }

  .mobile-nav-links a {
    min-height: 48px;
    font-size: 14px;
  }
}

@media (max-height: 650px) and (max-width: 760px) {
  .mobile-menu-header {
    padding-bottom: 10px;
  }

  .mobile-nav-links {
    padding-block: 10px;
    gap: 4px;
  }

  .mobile-nav-links a {
    min-height: 44px;
    padding-block: 10px;
  }
}

/* =========================================================
   REDUCED MOTION
========================================================= */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
  }

  .reveal {
    opacity: 1 !important;
    transform: none !important;
  }

  .code-mark-path {
    animation: none !important;
    stroke-dashoffset: 0;
    opacity: 1;
  }
}
</style>
