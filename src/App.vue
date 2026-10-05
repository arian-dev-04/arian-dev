<template>
  <div
    class="app"
    :class="[
      theme,
      { 'lang-fa': isFa, 'lang-en': !isFa, 'is-loading': isInitialLoading },
    ]"
    :dir="isFa ? 'rtl' : 'ltr'"
    :lang="isFa ? 'fa' : 'en'"
  >
    <Transition name="initial-loader" @after-leave="destroyLoaderLottie">
      <div
        v-if="isInitialLoading"
        class="initial-loader"
        role="status"
        aria-live="polite"
        :aria-label="isFa ? 'در حال آماده‌سازی سایت' : 'Initializing website'"
      >
        <div class="initial-loader-grid" aria-hidden="true"></div>

        <div class="initial-loader-atmosphere" aria-hidden="true">
          <span v-for="n in 18" :key="n" :style="{ '--i': n }"></span>
        </div>

        <div class="initial-loader-center">
          <div class="initial-loader-animation" aria-hidden="true">
            <span class="initial-loader-ring"></span>
            <span class="initial-loader-ring initial-loader-ring-2"></span>
            <span class="initial-loader-dot initial-loader-dot-1"></span>
            <span class="initial-loader-dot initial-loader-dot-2"></span>
            <span class="initial-loader-dot initial-loader-dot-3"></span>

            <div
              ref="loaderLottieContainer"
              class="initial-loader-lottie"
            ></div>
          </div>

          <h2>{{ loaderTitle }}</h2>

          <div class="initial-loader-progress-row">
            <div
              class="initial-loader-progress"
              role="progressbar"
              aria-valuemin="0"
              aria-valuemax="100"
              aria-valuenow="0"
              ref="loaderProgressEl"
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

    <div class="mouse-trail-layer" aria-hidden="true">
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
    <div class="bg-vignette" aria-hidden="true"></div>
    <div class="bg-orb bg-orb-1" aria-hidden="true"></div>
    <div class="bg-orb bg-orb-2" aria-hidden="true"></div>

    <!-- =========================================================
         NAVBAR
    ========================================================== -->
    <header
      class="navbar"
      :style="{
        transform: isNavVisible ? 'translateY(0)' : 'translateY(-110%)',
      }"
    >
      <div class="container nav-shell">
        <div class="nav-col nav-col-start">
          <a href="#home" class="brand" @click="menuOpen = false">
            <div class="brand-mark brand-code-mark" aria-hidden="true">
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

                <!-- drifting aurora + code rain (depth layers) -->
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

                <!-- "compiled" pulse -->
                <circle class="code-mark-pulse" cx="32" cy="32" r="8" />

                <!-- < / > -->
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

                <!-- terminal cursor -->
                <rect
                  class="code-mark-cursor"
                  x="25"
                  y="52"
                  width="14"
                  height="3"
                  rx="1.5"
                />

                <!-- live LED -->
                <g class="code-mark-led">
                  <circle class="led-ring" cx="53" cy="11" r="2.4" />
                  <circle class="led-dot" cx="53" cy="11" r="2.4" />
                </g>

                <circle class="code-mark-spark" cx="11" cy="53" r="1.3" />

                <!-- glass shine -->
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
            <a href="#home">{{ t.nav.home }}</a>
            <a href="#skills">{{ t.nav.skills }}</a>
            <a href="#services">{{ t.nav.services }}</a>
            <a href="#about">{{ t.nav.about }}</a>
            <a href="#projects">{{ t.nav.projects }}</a>
            <a href="#contact">{{ t.nav.contact }}</a>
          </nav>
        </div>

        <div class="nav-col nav-col-end">
          <div class="nav-actions">
            <button
              class="switch-btn"
              @click="toggleLang"
              type="button"
              :aria-label="isFa ? 'Switch to English' : 'تغییر به فارسی'"
            >
              <span class="switch-bg"></span>

              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">🌐</span>

                <span class="switch-text" :key="lang">
                  {{ isFa ? "English" : "فارسی" }}
                </span>
              </span>
            </button>

            <button
              class="switch-btn"
              @click="toggleTheme"
              type="button"
              :aria-label="
                theme === 'dark'
                  ? 'Switch to light mode'
                  : 'Switch to dark mode'
              "
            >
              <span class="switch-bg"></span>

              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">
                  {{ theme === "dark" ? "☀️" : "🌙" }}
                </span>

                <span class="switch-text" :key="theme + '-text'">
                  {{ theme === "dark" ? t.actions.light : t.actions.dark }}
                </span>
              </span>
            </button>
          </div>

          <button
            class="hamburger-btn"
            :class="{ active: menuOpen }"
            @click.stop="toggleMenu"
            type="button"
            :aria-expanded="menuOpen"
            aria-controls="mobile-navigation"
            aria-label="Toggle navigation"
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
              @click="menuOpen = false"
              aria-label="Close menu"
            >
              ×
            </button>
          </div>

          <nav
            id="mobile-navigation"
            class="mobile-nav-links"
            aria-label="Mobile navigation"
          >
            <a href="#home" @click="menuOpen = false">
              {{ t.nav.home }}
            </a>

            <a href="#skills" @click="menuOpen = false">
              {{ t.nav.skills }}
            </a>

            <a href="#services" @click="menuOpen = false">
              {{ t.nav.services }}
            </a>

            <a href="#about" @click="menuOpen = false">
              {{ t.nav.about }}
            </a>

            <a href="#projects" @click="menuOpen = false">
              {{ t.nav.projects }}
            </a>

            <a href="#contact" @click="menuOpen = false">
              {{ t.nav.contact }}
            </a>
          </nav>

          <div class="mobile-actions">
            <button class="switch-btn" @click="toggleLang" type="button">
              <span class="switch-bg"></span>

              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">🌐</span>

                <span class="switch-text">
                  {{ isFa ? "English" : "فارسی" }}
                </span>
              </span>
            </button>

            <button class="switch-btn" @click="toggleTheme" type="button">
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
      <!-- =========================================================
           HERO
      ========================================================== -->
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

            <p class="hero-desc">
              {{ t.hero.description }}
            </p>

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
              <span v-for="item in t.hero.techs" :key="item">
                {{ item }}
              </span>
            </div>
          </div>

          <div class="hero-right reveal reveal-right">
            <div class="glass-stage lottie-stage">
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

        <a href="#skills" class="scroll-indicator" aria-label="Scroll down">
          <span></span>
        </a>
      </section>

      <!-- =========================================================
           SKILLS
      ========================================================== -->
      <section
        id="skills"
        class="section skills-section flush-section section-auto"
      >
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">
              {{ t.skillsSection.tag }}
            </span>

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

      <!-- =========================================================
           SERVICES
      ========================================================== -->
      <section id="services" class="section services-section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">
              {{ t.services.tag }}
            </span>

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
                v-for="(service, index) in t.services.items"
                :key="service.title"
                class="service-card"
                :style="{ '--delay': `${index * 90}ms` }"
              >
                <div class="service-number">0{{ index + 1 }}</div>

                <div class="service-icon" aria-hidden="true">
                  {{ service.icon }}
                </div>

                <h3>{{ service.title }}</h3>

                <p>{{ service.desc }}</p>

                <div class="service-tags">
                  <span v-for="tag in service.tags" :key="tag">
                    {{ tag }}
                  </span>
                </div>
              </article>
            </div>
          </div>
        </div>
      </section>

      <!-- =========================================================
           SHOWCASE
      ========================================================== -->
      <section class="section showcase-section section-auto">
        <div class="container showcase-grid">
          <div class="showcase-media reveal reveal-left">
            <div class="showcase-image-frame">
              <img
                :src="codingWorkspaceImage"
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
            <span class="section-badge">
              {{ t.showcase.tag }}
            </span>

            <h2>{{ t.showcase.title }}</h2>

            <p>{{ t.showcase.desc }}</p>

            <div class="showcase-points">
              <div>
                <span class="showcase-point-icon">01</span>

                <div>
                  <strong>{{ t.showcase.point1Title }}</strong>
                  <p>{{ t.showcase.point1Desc }}</p>
                </div>
              </div>

              <div>
                <span class="showcase-point-icon">02</span>

                <div>
                  <strong>{{ t.showcase.point2Title }}</strong>
                  <p>{{ t.showcase.point2Desc }}</p>
                </div>
              </div>

              <div>
                <span class="showcase-point-icon">03</span>

                <div>
                  <strong>{{ t.showcase.point3Title }}</strong>
                  <p>{{ t.showcase.point3Desc }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- =========================================================
           ABOUT
      ========================================================== -->
      <section
        id="about"
        class="section flush-section soft-surface section-auto"
      >
        <div class="container about-grid">
          <div class="about-main reveal reveal-left">
            <span class="section-badge">
              {{ t.about.tag }}
            </span>

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

      <!-- =========================================================
           PROJECTS
      ========================================================== -->
      <section id="projects" class="section flush-section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">
              {{ t.projectsSection.tag }}
            </span>

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
                    :src="project.image"
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
                    <strong class="pc-preview-title">
                      {{ project.previewTitle }}
                    </strong>

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

      <!-- =========================================================
           PROCESS
      ========================================================== -->
      <section class="section process-section section-auto">
        <div class="container">
          <div class="section-heading reveal">
            <span class="section-badge">
              {{ t.process.tag }}
            </span>

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

      <!-- =========================================================
           CONTACT
      ========================================================== -->
      <section
        id="contact"
        class="section flush-section soft-surface section-auto"
      >
        <div class="container">
          <div class="contact-shell reveal">
            <div class="contact-light" aria-hidden="true"></div>

            <div class="contact-content">
              <span class="section-badge">
                {{ t.contact.tag }}
              </span>

              <h2>{{ t.contact.title }}</h2>

              <p>{{ t.contact.desc }}</p>

              <div class="contact-actions">
                <a
                  href="https://mail.google.com/mail/?view=cm&fs=1&to=arian.kalantari@yamil.com"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="premium-btn primary"
                >
                  <span class="btn-liquid" aria-hidden="true"></span>

                  <span class="btn-content">
                    {{ t.contact.emailBtn }}
                  </span>

                  <span class="btn-arrow" aria-hidden="true"> ↗ </span>
                </a>

                <a href="#about" class="premium-btn ghost">
                  <span class="btn-content">
                    {{ t.contact.resumeBtn }}
                  </span>

                  <span class="btn-arrow" aria-hidden="true"> → </span>
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

    <!-- =========================================================
         FOOTER
    ========================================================== -->
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
import lottie from "lottie-web";
import pollwonAnimation from "./assets/animations/pollwon.json";
import birdAnimation from "./assets/animations/bird (1).json";
import windowLayoutAnimation from "./assets/animations/Window layout.json";
import codingWorkspaceImage from "./assets/images/photo-programming.avif";
import techInterfaceImage from "./assets/images/photo-programming.avif";
import modernOfficeImage from "./assets/images/photo-programming.avif";

const lang = ref("fa");
const theme = ref("dark");

const isFa = computed(() => lang.value === "fa");

const currentYear = new Date().getFullYear();

const menuOpen = ref(false);
const isNavVisible = ref(true);

const isInitialLoading = ref(true);
const INITIAL_LOADING_MS = 2400;
let initialLoadingTimeout = null;

/* ---------- loader copy + progress ---------- */
const loaderBarEl = ref(null);
const loaderPercentEl = ref(null);
const loaderProgressEl = ref(null);

let loaderFrame = 0;
let loaderLoadHandler = null;

const loaderTitle = computed(() =>
  isFa.value ? "ایده‌ها را به کد تبدیل می‌کنیم" : "Where ideas meet code",
);

const finishInitialLoading = () => {
  isInitialLoading.value = false;
};

const startInitialLoader = () => {
  const startedAt = performance.now();

  const tick = (now) => {
    const raw = Math.min(
      1,
      Math.max(0, (now - startedAt) / INITIAL_LOADING_MS),
    );
    const eased = 0.5 - Math.cos(Math.PI * raw) / 2;

    // direct DOM writes: no full re-render on every frame
    const percent = Math.round(eased * 100);

    if (loaderBarEl.value) {
      loaderBarEl.value.style.transform = `scaleX(${eased})`;
    }

    if (loaderPercentEl.value) {
      loaderPercentEl.value.textContent = `${percent}%`;
    }

    if (loaderProgressEl.value) {
      loaderProgressEl.value.setAttribute("aria-valuenow", String(percent));
    }

    if (raw < 1) {
      loaderFrame = requestAnimationFrame(tick);

      return;
    }

    // animation done: wait for the page itself (max 3s) before revealing
    if (document.readyState === "complete") {
      finishInitialLoading();

      return;
    }

    loaderLoadHandler = finishInitialLoading;

    window.addEventListener("load", loaderLoadHandler, { once: true });

    initialLoadingTimeout = window.setTimeout(finishInitialLoading, 3000);
  };

  loaderFrame = requestAnimationFrame(tick);
};

const lottieContainer = ref(null);
const loaderLottieContainer = ref(null);
const mobileLottieContainer = ref(null);
const servicesLottieContainer = ref(null);
const contactLottieContainer = ref(null);

let loaderLottieInstance = null;

let lastScrollY = 0;
let typingTimeout = null;
let revealObserver = null;

let mouseMoveHandler = null;
let mouseOutHandler = null;

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value;
};

const toggleTheme = () => {
  theme.value = theme.value === "dark" ? "light" : "dark";
};

const toggleLang = () => {
  lang.value = lang.value === "fa" ? "en" : "fa";
};

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

    actions: {
      light: "روشن",
      dark: "دارک",
    },

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

    services: {
      tag: "خدمات",

      title: "چیزهایی که برای ساختن یک محصول دیجیتال انجام می‌دهم",

      desc: "از ایده و رابط کاربری تا توسعه و جزئیات نهایی، تمرکزم روی ساخت تجربه‌ای حرفه‌ای و قابل استفاده است.",

      items: [
        {
          title: "Web Development",
          icon: "◈",

          desc: "ساخت وب‌سایت‌ها و وب‌اپلیکیشن‌های مدرن، سریع و ریسپانسیو.",

          tags: ["Vue.js", "JavaScript", "Responsive"],
        },

        {
          title: "Mobile Development",
          icon: "⌁",

          desc: "طراحی و توسعه رابط‌های موبایل با تمرکز روی تجربه کاربری روان.",

          tags: ["Flutter", "Dart", "Mobile"],
        },

        {
          title: "UI / UX",
          icon: "✦",

          desc: "طراحی رابط و تجربه کاربری با تمرکز روی جزئیات، وضوح و رفتار کاربر.",

          tags: ["UI", "UX", "Design"],
        },

        {
          title: "Interactive Experience",
          icon: "✺",

          desc: "ساخت تعاملات و انیمیشن‌های سبک برای ایجاد تجربه‌ای زنده و متفاوت.",

          tags: ["Animation", "Motion", "Lottie"],
        },
      ],
    },

    showcase: {
      tag: "رویکرد من",

      title: "طراحی دقیق، کدنویسی تمیز و تجربه‌ای که حس کیفیت می‌دهد",

      desc: "هر پروژه را به‌عنوان یک محصول کامل می‌بینم؛ از ساختار و سرعت گرفته تا جزئیات بصری، تعاملات و کیفیت تجربه کاربر.",

      point1Title: "سرعت در اولویت",

      point1Desc:
        "کد سبک، انیمیشن کنترل‌شده و بارگذاری تصاویر با lazy loading.",

      point2Title: "ریسپانسیو از پایه",

      point2Desc: "چیدمان از ابتدا برای موبایل، تبلت و دسکتاپ طراحی می‌شود.",

      point3Title: "رابط کاربری دقیق",

      point3Desc:
        "فاصله‌ها، رنگ‌ها، تایپوگرافی و تعاملات با یک سیستم بصری یکپارچه.",

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
        {
          title: "Vue.js",
          desc: "برای ساخت رابط‌های مدرن، سریع و تعاملی",
        },

        {
          title: "Flutter",
          desc: "برای اپلیکیشن‌های موبایل زیبا و روان",
        },

        {
          title: "Go",
          desc: "برای درک و پیاده‌سازی منطق سریع و سبک",
        },

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
      categoryLabel: "دسته‌بندی",

      viewLabel: "مشاهده پروژه",

      unavailableLabel: "لینک پروژه ثبت نشده",

      readyLabel: "پروژه آنلاین",
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

    words: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
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

    actions: {
      light: "Light",
      dark: "Dark",
    },

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

    services: {
      tag: "SERVICES",

      title: "What I build for digital products",

      desc: "From interface design to development and final polish, I focus on creating digital experiences that feel refined and usable.",

      items: [
        {
          title: "Web Development",
          icon: "◈",

          desc: "Modern, fast and responsive websites and web applications.",

          tags: ["Vue.js", "JavaScript", "Responsive"],
        },

        {
          title: "Mobile Development",
          icon: "⌁",

          desc: "Smooth mobile interfaces with a strong focus on interaction and usability.",

          tags: ["Flutter", "Dart", "Mobile"],
        },

        {
          title: "UI / UX",
          icon: "✦",

          desc: "User interfaces and experiences designed around clarity, detail and behavior.",

          tags: ["UI", "UX", "Design"],
        },

        {
          title: "Interactive Experience",
          icon: "✺",

          desc: "Lightweight interactions and animations that make interfaces feel alive and distinctive.",

          tags: ["Animation", "Motion", "Lottie"],
        },
      ],
    },

    showcase: {
      tag: "APPROACH",

      title: "Precise design, clean code and experiences that feel premium",

      desc: "I treat every project as a complete product — from structure and performance to visual details, interactions and user experience.",

      point1Title: "Performance First",

      point1Desc: "Lean code, controlled animation and lazy-loaded imagery.",

      point2Title: "Responsive by Design",

      point2Desc:
        "Layouts are designed from the start for mobile, tablet and desktop.",

      point3Title: "Polished UI",

      point3Desc:
        "Spacing, color, typography and interaction work as one visual system.",

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
      categoryLabel: "CATEGORY",

      viewLabel: "VIEW PROJECT",

      unavailableLabel: "LINK NOT AVAILABLE",

      readyLabel: "Live project",
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

    words: ["Vue.js", "Flutter", "JavaScript", "Dart", "Go", "UI/UX"],
  },
};

const t = computed(() => translations[lang.value]);

/* =========================================================
   SEO
========================================================= */
const updateSeo = () => {
  if (typeof document === "undefined") return;

  const seo = isFa.value
    ? {
        title: "آرین کلانتری | توسعه‌دهنده Frontend، Flutter، Go و UI/UX",

        description:
          "پورتفولیوی آرین کلانتری؛ توسعه‌دهنده Frontend با Vue.js، توسعه‌دهنده Flutter و طراح UI/UX با تمرکز بر ساخت تجربه‌های دیجیتال سریع، مدرن و حرفه‌ای.",

        locale: "fa_IR",
        language: "fa",
      }
    : {
        title: "Arian Kalantari | Frontend, Flutter, Go & UI/UX Developer",

        description:
          "Arian Kalantari's portfolio — Frontend developer specializing in Vue.js, Flutter, Go and UI/UX, focused on building modern, fast and polished digital experiences.",

        locale: "en_US",
        language: "en",
      };

  document.title = seo.title;

  const setMeta = (selector, attributes) => {
    let element = document.head.querySelector(selector);

    if (!element) {
      element = document.createElement("meta");
      document.head.appendChild(element);
    }

    Object.entries(attributes).forEach(([key, value]) => {
      element.setAttribute(key, value);
    });
  };

  setMeta('meta[name="description"]', {
    name: "description",
    content: seo.description,
  });

  setMeta('meta[name="robots"]', {
    name: "robots",
    content:
      "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1",
  });

  setMeta('meta[name="googlebot"]', {
    name: "googlebot",
    content:
      "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1",
  });

  setMeta('meta[name="author"]', {
    name: "author",
    content: "Arian Kalantari",
  });

  setMeta('meta[name="theme-color"]', {
    name: "theme-color",
    content: theme.value === "dark" ? "#07111f" : "#f6f8fb",
  });

  setMeta('meta[property="og:title"]', {
    property: "og:title",
    content: seo.title,
  });

  setMeta('meta[property="og:description"]', {
    property: "og:description",
    content: seo.description,
  });

  setMeta('meta[property="og:type"]', {
    property: "og:type",
    content: "website",
  });

  setMeta('meta[property="og:locale"]', {
    property: "og:locale",
    content: seo.locale,
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
    content: seo.title,
  });

  setMeta('meta[name="twitter:description"]', {
    name: "twitter:description",
    content: seo.description,
  });

  let canonical = document.head.querySelector('link[rel="canonical"]');

  if (!canonical) {
    canonical = document.createElement("link");
    canonical.rel = "canonical";
    document.head.appendChild(canonical);
  }

  canonical.href = window.location.origin + window.location.pathname;

  document.documentElement.setAttribute("lang", seo.language);

  document.documentElement.setAttribute(
    "dir",
    seo.language === "fa" ? "rtl" : "ltr",
  );

  let structuredData = document.head.querySelector(
    "#portfolio-structured-data",
  );

  if (!structuredData) {
    structuredData = document.createElement("script");
    structuredData.type = "application/ld+json";
    structuredData.id = "portfolio-structured-data";

    document.head.appendChild(structuredData);
  }

  structuredData.textContent = JSON.stringify({
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
};

/* =========================================================
   SKILLS
========================================================= */
const localizedSkills = computed(() =>
  lang.value === "fa"
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
      ],
);

/* =========================================================
   PROJECT IMAGES
========================================================= */
const projectImages = [
  codingWorkspaceImage,
  techInterfaceImage,
  modernOfficeImage,
];

/* =========================================================
   PROJECT DATA
========================================================= */
const localizedProjects = computed(() =>
  lang.value === "fa"
    ? [
        {
          title: "Modern Business Website",

          type: "Website",

          category: "Corporate / Landing",

          previewTitle: "Modern Digital Experience",

          urlLabel: "preview.local / business",

          role: "Design + Frontend",

          description:
            "یک وب‌سایت مدرن با تمرکز روی ساختار حرفه‌ای، انیمیشن‌های نرم، ریسپانسیو بودن و تجربه کاربری روان.",

          tags: ["Vue.js", "Responsive", "UI/UX"],

          image: projectImages[0],

          url: "",
        },

        {
          title: "Mobile Application",

          type: "Mobile",

          category: "Flutter Application",

          previewTitle: "Smooth Mobile Interface",

          urlLabel: "app.preview / mobile",

          role: "UI + Flutter",

          description:
            "طراحی و توسعه رابط یک اپلیکیشن موبایل با تمرکز روی سرعت، تعاملات نرم و تجربه کاربری مدرن.",

          tags: ["Flutter", "Dart", "UX"],

          image: projectImages[1],

          url: "",
        },

        {
          title: "Admin Dashboard",

          type: "Dashboard",

          category: "Web Application",

          previewTitle: "Data Management System",

          urlLabel: "dashboard.local / admin",

          role: "Frontend Development",

          description:
            "یک داشبورد مدیریتی با ساختار تمیز، کامپوننت‌های قابل توسعه و رابط کاربری مناسب برای داده‌های زیاد.",

          tags: ["Vue.js", "JavaScript", "Admin UI"],

          image: projectImages[2],

          url: "",
        },
      ]
    : [
        {
          title: "Modern Business Website",

          type: "Website",

          category: "Corporate / Landing",

          previewTitle: "Modern Digital Experience",

          urlLabel: "preview.local / business",

          role: "Design + Frontend",

          description:
            "A polished business website focused on strong visual hierarchy, smooth animation, responsive layout and refined UX.",

          tags: ["Vue.js", "Responsive", "UI/UX"],

          image: projectImages[0],

          url: "",
        },

        {
          title: "Mobile Application",

          type: "Mobile",

          category: "Flutter Application",

          previewTitle: "Smooth Mobile Interface",

          urlLabel: "app.preview / mobile",

          role: "UI + Flutter",

          description:
            "A mobile experience designed around fluid interactions, clean interfaces and scalable Flutter architecture.",

          tags: ["Flutter", "Dart", "UX"],

          image: projectImages[1],

          url: "",
        },

        {
          title: "Admin Dashboard",

          type: "Dashboard",

          category: "Web Application",

          previewTitle: "Data Management System",

          urlLabel: "dashboard.local / admin",

          role: "Frontend Development",

          description:
            "A modern admin interface with clean structure, reusable components and a scalable visual system.",

          tags: ["Vue.js", "JavaScript", "Admin UI"],

          image: projectImages[2],

          url: "",
        },
      ],
);

/* ---------- project filters + per-card styling ---------- */
const activeFilter = ref("all");

const projectMeta = [
  { key: "web", number: "01", rgb: "88, 216, 255", hue: 0, focus: "50% 35%" },
  {
    key: "mobile",
    number: "02",
    rgb: "111, 240, 200",
    hue: -22,
    focus: "18% 60%",
  },
  {
    key: "dashboard",
    number: "03",
    rgb: "132, 121, 255",
    hue: 28,
    focus: "82% 40%",
  },
];

const projectsView = computed(() =>
  localizedProjects.value.map((project, index) => ({
    ...project,
    ...projectMeta[index],
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
    { key: "all", label: labels.all, count: projectsView.value.length },
    ...projectMeta.map(({ key }) => ({
      key,
      label: labels[key],
      count: projectsView.value.filter((project) => project.key === key).length,
    })),
  ];
});

/* =========================================================
   FONTS
========================================================= */
const loadFonts = () => {
  if (typeof document === "undefined" || document.getElementById("ak-fonts")) {
    return;
  }

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
const displayedWord = ref("");

let wordIndex = 0;
let charIndex = 0;
let deleting = false;

const typeEffect = () => {
  const words = t.value.words;

  if (!words.length) return;

  const currentWord = words[wordIndex];

  if (!deleting) {
    displayedWord.value = currentWord.slice(0, charIndex + 1);

    charIndex += 1;

    if (charIndex === currentWord.length) {
      deleting = true;

      typingTimeout = setTimeout(typeEffect, 1200);

      return;
    }
  } else {
    displayedWord.value = currentWord.slice(0, charIndex - 1);

    charIndex -= 1;

    if (charIndex === 0) {
      deleting = false;

      wordIndex = (wordIndex + 1) % words.length;
    }
  }

  typingTimeout = setTimeout(typeEffect, deleting ? 42 : 78);
};

const resetTyping = () => {
  if (typingTimeout) {
    clearTimeout(typingTimeout);
  }

  displayedWord.value = "";
  wordIndex = 0;
  charIndex = 0;
  deleting = false;

  typeEffect();
};

/* =========================================================
   LOTTIE
========================================================= */
const createLottie = (container, options = {}) => {
  if (!container) return null;

  const { animationData = pollwonAnimation, ...lottieOptions } = options;

  return lottie.loadAnimation({
    container,

    renderer: "svg",

    loop: true,

    autoplay: true,

    animationData,

    rendererSettings: {
      preserveAspectRatio: "xMidYMid meet",

      progressiveLoad: true,
    },

    ...lottieOptions,
  });
};

const initLoaderLottie = () => {
  loaderLottieInstance?.destroy();

  loaderLottieInstance = createLottie(loaderLottieContainer.value, {
    animationData: birdAnimation,
  });

  loaderLottieInstance?.setSpeed(0.9);
};

const destroyLoaderLottie = () => {
  loaderLottieInstance?.destroy();

  loaderLottieInstance = null;
};

/*
  Lazy lotties: created only when near the viewport,
  paused when off-screen, destroyed on unmount.
*/
const lottieEntries = new Map();

let lottieObserver = null;
let mobileLottieEl = null;

const setupLottieObserver = () => {
  if (!("IntersectionObserver" in window)) return;

  lottieObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach(({ target, isIntersecting }) => {
        const entry = lottieEntries.get(target);

        if (!entry) return;

        if (isIntersecting) {
          if (entry.instance) {
            entry.instance.play();
          } else {
            entry.instance = entry.create();
          }
        } else {
          entry.instance?.pause();
        }
      });
    },
    { rootMargin: "160px 0px" },
  );
};

const registerLottie = (el, create) => {
  if (!el) return;

  const entry = { create, instance: null };

  lottieEntries.set(el, entry);

  if (lottieObserver) {
    lottieObserver.observe(el);
  } else {
    entry.instance = create();
  }
};

const unregisterLottie = (el) => {
  const entry = lottieEntries.get(el);

  if (!entry) return;

  entry.instance?.destroy();

  lottieObserver?.unobserve(el);

  lottieEntries.delete(el);
};

const initLotties = () => {
  registerLottie(lottieContainer.value, () =>
    createLottie(lottieContainer.value),
  );

  registerLottie(servicesLottieContainer.value, () => {
    const instance = createLottie(servicesLottieContainer.value, {
      animationData: windowLayoutAnimation,
    });

    instance?.setSpeed(0.8);

    return instance;
  });

  registerLottie(contactLottieContainer.value, () => {
    const instance = createLottie(contactLottieContainer.value);

    instance?.setSpeed(0.75);

    return instance;
  });
};

// the mobile menu is rendered with v-if, so its lottie is created on open
const releaseMobileLottie = () => {
  if (mobileLottieEl) {
    unregisterLottie(mobileLottieEl);

    mobileLottieEl = null;
  }
};

watch(menuOpen, async (open) => {
  if (!open) return;

  await nextTick();

  const el = mobileLottieContainer.value;

  if (!el || el === mobileLottieEl) return;

  mobileLottieEl = el;

  registerLottie(el, () => {
    const instance = createLottie(el);

    instance?.setSpeed(0.7);

    return instance;
  });
});

/* =========================================================
   MOUSE TRAIL
========================================================= */
const stars = ref([]);

let starId = 0;
let lastEmit = 0;

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

  const item = {
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
  };

  if (stars.value.length >= 22) {
    stars.value.shift();
  }

  stars.value.push(item);

  window.setTimeout(() => {
    const index = stars.value.findIndex((star) => star.id === id);
    if (index !== -1) stars.value.splice(index, 1);
  }, duration + 80);
};

const clearTrail = () => {
  stars.value = [];
};

const handleGlobalMouseMove = (e) => {
  const now = performance.now();
  const deltaTime = Math.max(8, now - (handleGlobalMouseMove.lastTime || now));
  const dx = e.clientX - (handleGlobalMouseMove.lastX || e.clientX);
  const dy = e.clientY - (handleGlobalMouseMove.lastY || e.clientY);
  const speed = Math.hypot(dx, dy) / deltaTime;

  handleGlobalMouseMove.lastX = e.clientX;
  handleGlobalMouseMove.lastY = e.clientY;
  handleGlobalMouseMove.lastTime = now;

  const emitGap = speed > 0.85 ? 16 : speed > 0.35 ? 22 : 30;
  if (now - lastEmit < emitGap) return;

  lastEmit = now;

  createSoftStar(e.clientX, e.clientY, speed > 0.85 ? 10 : 7);

  if (speed > 0.4) {
    createSoftStar(e.clientX, e.clientY, 12);
  }

  if (speed > 0.9 && Math.random() > 0.35) {
    createSoftStar(e.clientX, e.clientY, 17);
  }

  if (speed > 1.35 && Math.random() > 0.55) {
    createSoftStar(e.clientX, e.clientY, 21);
  }
};

const handleGlobalMouseLeave = () => {
  clearTrail();
};

/* =========================================================
   SCROLL
========================================================= */
let scrollTicking = false;

const updateNavVisibility = () => {
  scrollTicking = false;

  if (menuOpen.value) {
    isNavVisible.value = true;

    return;
  }

  const currentScrollY = window.scrollY;

  isNavVisible.value = !(currentScrollY > lastScrollY && currentScrollY > 90);

  lastScrollY = currentScrollY;
};

const handleScroll = () => {
  if (scrollTicking) return;

  scrollTicking = true;

  requestAnimationFrame(updateNavVisibility);
};

/* =========================================================
   REVEAL
========================================================= */
const setupReveal = () => {
  const pending = document.querySelectorAll(".reveal:not(.in-view)");

  if (!("IntersectionObserver" in window)) {
    pending.forEach((el) => el.classList.add("in-view"));

    return;
  }

  if (!revealObserver) {
    revealObserver = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add("in-view");

            revealObserver?.unobserve(entry.target);
          }
        });
      },
      {
        threshold: 0.12,

        rootMargin: "0px 0px -40px 0px",
      },
    );
  }

  // safe to call again: observing the same element twice is a no-op
  pending.forEach((element) => revealObserver.observe(element));
};

/* =========================================================
   MOUNT
========================================================= */
onMounted(() => {
  loadFonts();

  initLoaderLottie();

  startInitialLoader();

  const savedTheme = localStorage.getItem("ak-theme");

  const savedLang = localStorage.getItem("ak-lang");

  if (savedTheme === "dark" || savedTheme === "light") {
    theme.value = savedTheme;
  }

  if (savedLang === "fa" || savedLang === "en") {
    lang.value = savedLang;
  }

  updateSeo();

  typeEffect();

  setupLottieObserver();

  requestAnimationFrame(() => {
    initLotties();
    setupReveal();
  });

  mouseMoveHandler = (e) => handleGlobalMouseMove(e);

  mouseOutHandler = (e) => {
    if (!e.relatedTarget && !e.toElement) {
      handleGlobalMouseLeave();
    }
  };

  if (window.matchMedia("(hover: hover) and (pointer: fine)").matches) {
    window.addEventListener("mousemove", mouseMoveHandler, {
      passive: true,
    });

    window.addEventListener("mouseout", mouseOutHandler);
    document.documentElement.addEventListener(
      "mouseleave",
      handleGlobalMouseLeave,
    );
  }

  window.addEventListener("scroll", handleScroll, {
    passive: true,
  });
});

/* =========================================================
   UNMOUNT
========================================================= */
onBeforeUnmount(() => {
  if (initialLoadingTimeout) {
    clearTimeout(initialLoadingTimeout);
  }

  if (typingTimeout) {
    clearTimeout(typingTimeout);
  }

  cancelAnimationFrame(loaderFrame);

  if (loaderLoadHandler) {
    window.removeEventListener("load", loaderLoadHandler);
  }

  loaderLottieInstance?.destroy();

  lottieEntries.forEach((entry) => entry.instance?.destroy());
  lottieEntries.clear();
  lottieObserver?.disconnect();

  if (mouseMoveHandler) {
    window.removeEventListener("mousemove", mouseMoveHandler);
  }

  if (mouseOutHandler) {
    window.removeEventListener("mouseout", mouseOutHandler);
    document.documentElement.removeEventListener(
      "mouseleave",
      handleGlobalMouseLeave,
    );
  }

  window.removeEventListener("scroll", handleScroll);

  revealObserver?.disconnect();
});

/* =========================================================
   WATCHERS
========================================================= */
watch(theme, (value) => {
  localStorage.setItem("ak-theme", value);

  updateSeo();
});

watch(lang, (value) => {
  localStorage.setItem("ak-lang", value);

  menuOpen.value = false;

  resetTyping();

  updateSeo();

  // re-scan after the new language has been rendered
  nextTick(setupReveal);
});
</script>

<style scoped>
:global(*),
:global(*::before),
:global(*::after) {
  box-sizing: border-box;
}

:global(html) {
  scroll-behavior: smooth;
  scroll-padding-top: 84px;
  background: #07111f;
}

:global(body) {
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
  display: block;
}

/* =========================================================
   ROOT
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
  --font-sans:
    "Plus Jakarta Sans", "Vazirmatn", system-ui, -apple-system, "Segoe UI",
    sans-serif;
  --font-display:
    "Space Grotesk", "Vazirmatn", "Plus Jakarta Sans", system-ui, sans-serif;
  --font-mono:
    "JetBrains Mono", "Fira Code", ui-monospace, SFMono-Regular, Menlo,
    monospace;

  font-family: var(--font-sans);

  position: relative;
  min-height: 100vh;
  overflow-x: clip;
  color: var(--text);
  background: var(--bg);
  isolation: isolate;
  padding-top: 74px;
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
}

/* =========================================================
   SKIP LINK
========================================================= */
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
  background: #050b14;
  color: #f5f8fd;
  isolation: isolate;
}

.app.light .initial-loader {
  background: #f4f7fb;
  color: #0f172a;
}

.app.is-loading {
  max-height: 100vh;
  overflow: hidden;
}

.initial-loader-grid {
  position: absolute;
  inset: 0;
  opacity: 0.34;
  background-image:
    linear-gradient(rgba(88, 216, 255, 0.055) 1px, transparent 1px),
    linear-gradient(90deg, rgba(88, 216, 255, 0.055) 1px, transparent 1px);
  background-size: 42px 42px;
  animation: loaderGridDrift 20s linear infinite;
  mask-image: radial-gradient(
    circle at center,
    black 0%,
    rgba(0, 0, 0, 0.35) 44%,
    transparent 82%
  );
}

.initial-loader-atmosphere {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  overflow: hidden;
  background: transparent;
}

.initial-loader-atmosphere span {
  position: absolute;
  width: clamp(2px, 0.4vw, 5px);
  height: clamp(2px, 0.4vw, 5px);
  left: calc((var(--i) * 13%) - 4%);
  top: calc((var(--i) * 17%) + 7%);
  border-radius: 50%;
  background: hsl(calc(178 + var(--i) * 28), 100%, 78%);
  box-shadow:
    0 0 10px currentColor,
    0 0 22px currentColor;
  opacity: 0.34;
  animation: loaderParticleFloat calc(8s + var(--i) * 0.55s) ease-in-out
    infinite;
  animation-delay: calc(var(--i) * -0.55s);
}

.initial-loader-center {
  position: relative;
  z-index: 2;
  width: min(760px, calc(100vw - 32px));
  padding: clamp(18px, 4vw, 34px);
  text-align: center;
  border: 0;
  border-radius: 0;
  background: transparent;
  box-shadow: none;
  backdrop-filter: none;
  -webkit-backdrop-filter: none;
}

.app.light .initial-loader-center {
  border: 0;
  background: transparent;
  box-shadow: none;
}

.initial-loader-animation {
  position: relative;
  width: clamp(146px, 31vw, 198px);
  height: clamp(146px, 31vw, 198px);
  margin: 0 auto 26px;
  display: grid;
  place-items: center;
  isolation: isolate;
}

.initial-loader-animation::before {
  content: "";
  position: absolute;
  inset: 4%;
  border-radius: 50%;
  border: 1px dashed rgba(133, 226, 255, 0.2);
  border-top-color: rgba(88, 216, 255, 0.72);
  border-right-color: rgba(116, 105, 255, 0.56);
  border-bottom-color: rgba(114, 242, 203, 0.44);
  transform: rotate(-18deg);
  animation: loaderTechnicalOrbit 9s linear infinite;
}

.initial-loader-ring {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 1px solid rgba(157, 225, 255, 0.11);
  border-top-color: #58d8ff;
  border-right-color: rgba(88, 216, 255, 0.75);
  border-bottom-color: rgba(114, 242, 203, 0.24);
  box-shadow: 0 0 9px rgba(88, 216, 255, 0.07);
  animation: loaderRingSpin 4.8s cubic-bezier(0.65, 0, 0.35, 1) infinite;
}

.initial-loader-ring-2 {
  inset: 15px;
  border-color: rgba(116, 105, 255, 0.1);
  border-bottom-color: #7469ff;
  border-left-color: rgba(116, 105, 255, 0.62);
  box-shadow: 0 0 10px rgba(116, 105, 255, 0.07);
  animation-duration: 6.6s;
  animation-direction: reverse;
}

.initial-loader-dot {
  position: absolute;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #dffcff;
  box-shadow: 0 0 12px rgba(88, 216, 255, 0.8);
  z-index: 3;
  transform-origin: calc(50% - 0px) calc(50% + 0px);
}

.initial-loader-dot-1 {
  top: 6%;
  left: 50%;
  animation: loaderDotFloat 5.8s ease-in-out infinite;
}

.initial-loader-dot-2 {
  right: 11%;
  bottom: 21%;
  background: #b5aaff;
  box-shadow: 0 0 12px rgba(116, 105, 255, 0.75);
  animation: loaderDotFloat 6.4s ease-in-out infinite -0.9s;
}

.initial-loader-dot-3 {
  left: 12%;
  bottom: 22%;
  background: #baffeb;
  box-shadow: 0 0 12px rgba(114, 242, 203, 0.72);
  animation: loaderDotFloat 7.2s ease-in-out infinite -1.5s;
}

.initial-loader-animation::after {
  content: "";
  position: absolute;
  inset: -8%;
  border-radius: 50%;
  pointer-events: none;
  background:
    radial-gradient(
      circle at 50% 3%,
      rgba(88, 216, 255, 0.95) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 84% 23%,
      rgba(116, 105, 255, 0.9) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 93% 66%,
      rgba(114, 242, 203, 0.9) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 65% 94%,
      rgba(255, 142, 234, 0.85) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 17% 82%,
      rgba(255, 211, 107, 0.85) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 7% 38%,
      rgba(117, 169, 255, 0.9) 0 2px,
      transparent 3px
    ),
    radial-gradient(
      circle at 31% 8%,
      rgba(255, 255, 255, 0.95) 0 1.5px,
      transparent 3px
    ),
    radial-gradient(
      circle at 39% 97%,
      rgba(143, 124, 255, 0.85) 0 1.5px,
      transparent 3px
    ),
    radial-gradient(
      circle at 96% 38%,
      rgba(98, 232, 255, 0.85) 0 1.5px,
      transparent 3px
    ),
    radial-gradient(
      circle at 11% 61%,
      rgba(115, 244, 207, 0.85) 0 1.5px,
      transparent 3px
    );
  opacity: 0.62;
  animation: loaderOrbitDots 18s linear infinite;
}

.initial-loader-lottie {
  position: relative;
  z-index: 2;
  width: 68%;
  height: 68%;
  filter: drop-shadow(0 0 7px rgba(88, 216, 255, 0.09));
}

/* Persian letters must stay connected */
.app.lang-fa .initial-loader-center h2 {
  letter-spacing: 0;
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

  .initial-loader-progress {
    margin: 14px auto 10px;
  }
}

.initial-loader-lottie svg {
  width: 100% !important;
  height: 100% !important;
  display: block;
}

.initial-loader-center h2 {
  margin: 0;
  color: inherit;
  font-size: clamp(16px, 2vw, 21px);
  line-height: 1.4;
  font-weight: 850;
  letter-spacing: 0.02em;
}

.initial-loader-center p {
  margin: 9px 0 0;
  color: #8fa3bd;
  font-size: 13px;
  line-height: 1.7;
}

.app.light .initial-loader-center p {
  color: #66748a;
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
  margin: 0;
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
  box-shadow: 0 0 22px rgba(88, 216, 255, 0.34);
  transform: scaleX(0);
  transform-origin: left center;
  will-change: transform;
}

.initial-loader-percent {
  min-width: 44px;
  color: #eefbff;
  font-size: 13px;
  font-weight: 900;
  font-variant-numeric: tabular-nums;
  letter-spacing: 0.04em;
  text-align: end;
}

.app.light .initial-loader-percent {
  color: #17243a;
}

.initial-loader-enter-active,
.initial-loader-leave-active {
  transition:
    opacity 0.42s ease,
    visibility 0.42s ease;
}

.initial-loader-enter-from,
.initial-loader-leave-to {
  opacity: 0;
  visibility: hidden;
}

@keyframes loaderAtmosphereSpin {
  to {
    transform: rotate(360deg) scale(1.04);
  }
}

@keyframes loaderGridDrift {
  0% {
    background-position:
      0 0,
      0 0;
    transform: scale(1);
  }
  50% {
    background-position:
      18px 10px,
      -12px 16px;
    transform: scale(1.025);
  }
  100% {
    background-position:
      36px 20px,
      -24px 32px;
    transform: scale(1.05);
  }
}

@keyframes loaderParticleFloat {
  0%,
  100% {
    transform: translate3d(0, 0, 0) scale(0.72);
    opacity: 0.18;
  }
  50% {
    transform: translate3d(0, -18px, 0) scale(1.25);
    opacity: 0.72;
  }
}

@keyframes loaderRingSpin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes loaderTechnicalOrbit {
  0%,
  100% {
    transform: scale(0.88);
    opacity: 0.62;
  }
  50% {
    transform: scale(1.06);
    opacity: 1;
  }
}

@keyframes loaderCore {
  0%,
  100% {
    transform: scale(0.94);
    opacity: 0.42;
  }
  50% {
    transform: scale(1.04);
    opacity: 0.86;
  }
}

@keyframes loaderDotFloat {
  0%,
  100% {
    transform: translate3d(0, 0, 0) scale(0.85);
    opacity: 0.46;
  }
  50% {
    transform: translate3d(0, -5px, 0) scale(1.12);
    opacity: 1;
  }
}

@keyframes loaderOrbitDots {
  0% {
    transform: rotate(0deg) scale(0.98);
  }
  50% {
    transform: rotate(180deg) scale(1.015);
  }
  100% {
    transform: rotate(360deg) scale(0.98);
  }
}

/* =========================================================
   BACKGROUND
========================================================= */
.bg-base,
.bg-vignette,
.bg-orb {
  position: fixed;
  inset: 0;
  pointer-events: none;
}

.bg-base {
  z-index: -20;

  background: linear-gradient(180deg, #07111f 0%, #081422 52%, #07101c 100%);
}

.app.light .bg-base {
  background: linear-gradient(180deg, #fbfcfe 0%, #f7f9fc 52%, #f1f4f8 100%);
}

.bg-vignette {
  z-index: -19;

  background: radial-gradient(
    circle at 50% 28%,
    rgba(74, 194, 250, 0.045),
    transparent 44%
  );
}

.app.light .bg-vignette {
  background: radial-gradient(
    circle at 50% 28%,
    rgba(60, 140, 220, 0.035),
    transparent 44%
  );
}

.bg-orb {
  z-index: -18;
  inset: auto;
  width: 280px;
  height: 280px;
  border-radius: 50%;
  filter: blur(70px);
  opacity: 0.16;
}

.bg-orb-1 {
  top: 6%;
  left: -120px;
  background: rgba(87, 212, 255, 0.12);
}

.bg-orb-2 {
  top: 24%;
  right: -120px;
  background: rgba(116, 105, 255, 0.1);
}

.app.light .bg-orb {
  opacity: 0.09;
}

/* =========================================================
   MOUSE TRAIL
========================================================= */
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
  width: min(1240px, calc(100% - 40px));
  margin-inline: auto;
}

.section {
  position: relative;
  z-index: 2;
  padding: 118px 0;
}

.section-auto {
  content-visibility: auto;
  contain-intrinsic-size: 700px;
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

  direction: inherit;

  padding: 8px 0;

  background: transparent;

  border: 0;

  transition: transform 0.34s ease;
}

.app.lang-fa .nav-shell,
.app.lang-en .nav-shell {
  animation: navLanguageFlip 0.38s cubic-bezier(0.22, 0.8, 0.2, 1);
}

.app.light .navbar {
  background: transparent;
}

.app.lang-en .navbar {
  direction: ltr;
}

.app.lang-fa .navbar {
  direction: rtl;
}

.app.lang-en .nav-links {
  direction: ltr;
}

.app.lang-fa .nav-links {
  direction: rtl;
}

.app.lang-fa .brand {
  direction: rtl;
  text-align: right;
}

.app.lang-en .brand {
  direction: ltr;
  text-align: left;
}

.nav-shell {
  min-height: 58px;

  display: grid;

  grid-template-columns:
    minmax(0, 1fr)
    auto
    minmax(0, 1fr);

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
    rgba(7, 17, 31, 0.78);

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

/* -----------------------------------------------------------------
   NOTE: grid columns are placed logically so that RTL/LTR mirrors
   happen automatically through `direction`, not through duplicated
   grid-template-areas overrides.
----------------------------------------------------------------- */
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

  color: var(--text);

  direction: ltr;
  text-align: left;

  min-width: 0;
}

.brand-mark {
  position: relative;

  width: 40px;
  height: 40px;

  display: grid;
  place-items: center;

  flex-shrink: 0;

  overflow: hidden;

  border-radius: 11px;

  background:
    radial-gradient(
      circle at 30% 20%,
      rgba(88, 216, 255, 0.2),
      transparent 50%
    ),
    linear-gradient(145deg, #1b2432, #0d141e);

  border: 1px solid rgba(117, 188, 255, 0.16);

  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    0 8px 22px rgba(0, 0, 0, 0.18);
}

/* ---------- custom coding animation (navbar logo) ---------- */
.brand-code-mark {
  isolation: isolate;

  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.35s ease;

  padding: 0;

  border: 0;
  border-radius: 12px;

  background: transparent;

  box-shadow: 0 8px 22px rgba(0, 0, 0, 0.2);
}

.brand-code-mark::before {
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

.brand-code-mark::after {
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

.app.light .brand-code-mark {
  box-shadow: 0 8px 20px rgba(56, 77, 105, 0.14);
}

.app.light .brand-code-mark::after {
  background:
    radial-gradient(
      circle at 28% 16%,
      rgba(11, 158, 215, 0.14),
      transparent 56%
    ),
    linear-gradient(145deg, #ffffff, #e9f0f8);
}

.code-mark-svg {
  position: relative;

  width: 80%;
  height: 80%;

  overflow: visible;

  filter: drop-shadow(0 0 5px rgba(88, 216, 255, 0.4));
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

.code-mark-shift.shift-left {
  --shift: -1.6px;
}

.code-mark-shift.shift-right {
  --shift: 1.6px;
}

.brand:hover .brand-code-mark,
.brand:focus-visible .brand-code-mark {
  transform: translateY(-2px) rotate(-4deg);

  box-shadow: 0 12px 28px rgba(88, 216, 255, 0.22);
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

.mobile-lottie svg,
.services-lottie svg,
.contact-lottie svg {
  width: 100% !important;
  height: 100% !important;
  display: block;
}

.brand-copy {
  width: 168px;
  min-width: 0;

  display: flex;
  flex-direction: column;

  gap: 3px;
}

.brand-copy strong {
  color: var(--text);

  font-size: 14px;
  line-height: 1.1;

  white-space: nowrap;
}

.brand-copy strong {
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

  color: transparent;

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

  direction: ltr;

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
  display: none;

  width: 29px;
  height: 22px;

  padding: 0;

  border: 0;

  background: transparent;

  cursor: pointer;

  flex-direction: column;
  justify-content: space-between;
}

.hamburger-btn span {
  display: block;

  width: 100%;
  height: 3px;

  border-radius: 999px;

  background: var(--text);

  transition:
    transform 0.3s ease,
    opacity 0.2s ease;
}

.hamburger-btn.active span:nth-child(1) {
  transform: translateY(9px) rotate(45deg);
}

.hamburger-btn.active span:nth-child(2) {
  opacity: 0;
}

.hamburger-btn.active span:nth-child(3) {
  transform: translateY(-9px) rotate(-45deg);
}

/* =========================================================
   MOBILE MENU
========================================================= */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;

  z-index: 70;

  display: flex;
  align-items: stretch;
  justify-content: flex-start;

  direction: ltr;

  padding: 0;

  background: rgba(2, 7, 15, 0.72);

  backdrop-filter: blur(18px);
}

.app.lang-fa .mobile-menu-overlay {
  justify-content: flex-end;
}

.app.light .mobile-menu-overlay {
  background: rgba(229, 236, 245, 0.7);
}

.mobile-menu-panel {
  width: min(390px, 88vw);
  height: 100dvh;
  min-height: 100%;
  padding: max(20px, env(safe-area-inset-top)) 21px
    max(21px, env(safe-area-inset-bottom));
  overflow-y: auto;
  border: 1px solid var(--border);
  border-top: 0;
  border-bottom: 0;
  border-left: 0;
  border-radius: 0 26px 26px 0;
  background: rgba(12, 22, 37, 0.97);
  box-shadow: 25px 0 70px rgba(0, 0, 0, 0.32);
  direction: ltr;
}

.app.lang-fa .mobile-menu-panel {
  border-left: 1px solid var(--border);
  border-right: 0;
  border-radius: 26px 0 0 26px;
  box-shadow: -25px 0 70px rgba(0, 0, 0, 0.32);
  direction: rtl;
}

.app.light .mobile-menu-panel {
  background: rgba(255, 255, 255, 0.96);

  box-shadow: 25px 0 70px rgba(55, 75, 105, 0.12);
}

.app.light.lang-fa .mobile-menu-panel {
  box-shadow: -25px 0 70px rgba(55, 75, 105, 0.12);
}

.mobile-menu-header {
  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 12px;

  padding-bottom: 17px;

  border-bottom: 1px solid var(--border);
}

.mobile-menu-brand {
  display: flex;
  align-items: center;

  gap: 10px;

  min-width: 0;
}

.mobile-menu-mark {
  width: 40px;
  height: 40px;

  display: grid;
  place-items: center;

  overflow: hidden;

  border-radius: 12px;

  background: linear-gradient(135deg, var(--primary), var(--primary-2));
}

.mobile-lottie {
  width: 32px;
  height: 32px;
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
  width: 36px;
  height: 36px;

  display: grid;
  place-items: center;

  border: 1px solid var(--border);

  border-radius: 11px;

  background: var(--surface);

  color: var(--text);

  font-size: 23px;

  cursor: pointer;
}

.mobile-nav-links {
  display: flex;
  flex-direction: column;

  gap: 4px;

  padding: 17px 0;
}

.mobile-nav-links a {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 13px 11px;

  border-radius: 12px;

  color: var(--text);

  font-size: 15px;
  font-weight: 700;

  transition:
    background 0.22s ease,
    color 0.22s ease,
    transform 0.22s ease;
}

.mobile-nav-links a:hover {
  background: var(--surface);

  color: var(--primary);

  transform: translateX(-2px);
}

.mobile-actions {
  display: grid;

  grid-template-columns: 1fr 1fr;

  gap: 9px;
}

.mobile-actions .switch-btn {
  width: 100%;
  height: 42px;
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
    rgba(88, 216, 255, 0.045),
    transparent 72%
  );

  filter: blur(15px);

  pointer-events: none;
}

.hero-grid {
  display: grid;

  grid-template-columns:
    minmax(0, 1.05fr)
    minmax(370px, 0.95fr);

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

  isolation: isolate;

  width: fit-content;
  max-width: 100%;

  min-height: 40px;

  transform: translateZ(0);

  backface-visibility: hidden;

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

  background: linear-gradient(
    120deg,
    #fff 0%,
    #a8ecff 34%,
    #968aff 67%,
    #ddd8ff 100%
  );

  -webkit-background-clip: text;
  background-clip: text;

  color: transparent;
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

  color: transparent;
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

.floating-techs span,
.skill-cloud span {
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

.floating-techs span:hover,
.skill-cloud span:hover {
  transform: translateY(-2px);

  border-color: var(--border-strong);

  color: var(--primary);
}

.premium-btn {
  position: relative;

  display: inline-flex;
  align-items: center;
  justify-content: center;

  gap: 9px;

  min-width: 178px;
  min-height: 54px;

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
}

.btn-content {
  font-size: 14px;
  font-weight: 750;
}

.btn-arrow {
  font-size: 14px;

  transition: transform 0.25s ease;
}

.premium-btn:hover .btn-arrow {
  transform: translate(2px, -2px);
}

/* =========================================================
   HERO LOTTIE
========================================================= */
.glass-stage {
  position: relative;

  min-height: 560px;

  display: grid;
  place-items: center;
}

.stage-glow {
  position: absolute;

  width: 380px;
  height: 380px;

  border-radius: 50%;

  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.075),
    rgba(116, 105, 255, 0.03) 42%,
    transparent 72%
  );

  filter: blur(25px);

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

  padding: 0;

  border: 0 !important;
  border-radius: 0 !important;

  background: transparent !important;

  box-shadow: none !important;

  backdrop-filter: none !important;
  -webkit-backdrop-filter: none !important;

  overflow: visible;
}

.lottie-card::before,
.lottie-card::after {
  content: none !important;
  display: none !important;
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

.hero-lottie svg {
  width: 100% !important;
  height: 100% !important;

  display: block;
}

/* =========================================================
   LOTTIE CAPTION
========================================================= */
.lottie-caption {
  margin-top: 40px;

  position: relative;
  z-index: 3;

  width: min(100%, 350px);

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

  box-shadow:
    0 0 0 5px rgba(114, 244, 202, 0.06),
    0 0 18px rgba(114, 244, 202, 0.32);

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

  overflow: visible;

  border: 0;

  border-radius: 28px;

  background: transparent;

  box-shadow: none;
}

.skill-panel {
  padding: 29px;

  min-height: 224px;

  background: var(--surface);

  border: 1px solid var(--border);

  border-radius: 24px;

  box-shadow: none;

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

  background: var(--surface-strong);

  border: 1px solid var(--border);

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

  grid-template-columns:
    0.72fr
    1.28fr;

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

  width: 260px;
  height: 260px;

  border-radius: 50%;

  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.11),
    transparent 70%
  );

  filter: blur(25px);
}

.services-lottie-card {
  position: relative;

  width: min(100%, 390px);

  aspect-ratio: 1;

  border-radius: 32px;

  border: 1px solid var(--border);

  background: var(--surface);

  box-shadow: var(--shadow);

  display: grid;
  place-items: center;

  overflow: hidden;

  min-height: 0;
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

  border: 1px solid var(--border);

  border-radius: 24px;

  background: var(--surface);

  overflow: hidden;

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
  right: 20px;

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

  background: rgba(88, 216, 255, 0.08);

  border: 1px solid var(--border);

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
  padding-top: 105px;
  padding-bottom: 105px;
}

.showcase-grid {
  display: grid;

  grid-template-columns:
    1fr
    0.9fr;

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

  box-shadow: 0 18px 35px rgba(0, 0, 0, 0.22);

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
  left: -16px;
  bottom: 35px;
}

.showcase-card-2 {
  right: -16px;
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

  grid-template-columns:
    1.08fr
    0.92fr;

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

  display: grid;
  place-items: center;

  flex-shrink: 0;

  border-radius: 50%;

  background: rgba(89, 216, 255, 0.09);

  color: var(--primary);

  font-size: 11px;
}

.about-point p {
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
  display: inline-flex;
  align-items: center;

  gap: 8px;

  min-height: 40px;

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

  display: flex;
  flex-direction: column;

  min-width: 0;

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

  background: #0b1421;

  isolation: isolate;
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

  display: flex;
  align-items: center;

  gap: 10px;

  height: 30px;

  padding: 0 11px;

  border: 1px solid rgba(255, 255, 255, 0.13);
  border-radius: 10px;

  background: rgba(7, 17, 31, 0.58);

  backdrop-filter: blur(8px);

  direction: ltr;
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
  display: inline-flex;
  align-items: center;

  gap: 8px;

  min-height: 40px;

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
  grid-template-columns:
    minmax(0, 1.1fr)
    minmax(0, 0.9fr);
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

  width: 420px;
  height: 420px;

  margin-top: -210px;

  border-radius: 50%;

  background: radial-gradient(
    circle,
    rgba(88, 216, 255, 0.13),
    transparent 68%
  );

  filter: blur(14px);

  pointer-events: none;

  animation: stageGlow 8s ease-in-out infinite;
}

.contact-content {
  position: relative;
  z-index: 2;

  min-width: 0;

  text-align: start;
}

.contact-shell h2 {
  margin: 0 0 16px;

  font-size: clamp(30px, 3.4vw, 46px);
  line-height: 1.2;
}

.contact-shell p {
  max-width: 520px;

  margin: 0;
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

    margin: 0 0 0 -210px;
  }
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
  transform: translateX(-25px) translateY(8px);
}

.reveal-right {
  transform: translateX(25px) translateY(8px);
}

.reveal.in-view {
  opacity: 1;

  transform: translate3d(0, 0, 0);
}

/* =========================================================
   ANIMATIONS
========================================================= */
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

@keyframes scrollDot {
  0% {
    transform: translateY(0);

    opacity: 1;
  }

  60% {
    transform: translateY(15px);

    opacity: 0.35;
  }

  100% {
    transform: translateY(0);

    opacity: 1;
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
   HERO FRAME REMOVAL
========================================================= */
.lottie-card,
.lottie-card::before,
.lottie-card::after {
  border: 0 !important;

  background: transparent !important;

  box-shadow: none !important;

  backdrop-filter: none !important;

  -webkit-backdrop-filter: none !important;
}

/* =========================================================
   MOBILE HERO
========================================================= */
@media (max-width: 760px) {
  .container {
    width: calc(100% - 22px);
  }

  .hero-grid {
    grid-template-columns: 1fr;
    gap: 48px;
  }

  .hero-left {
    text-align: center;
  }

  .hero-chip {
    margin-inline: auto;

    justify-content: center;
  }

  .hero-title {
    justify-content: center;

    font-size: clamp(30px, 7.7vw, 42px);
  }

  .hero-subtitle {
    min-height: auto;

    font-size: 18px;
  }

  .hero-desc {
    max-width: 620px;

    margin-inline: auto;

    font-size: 13.5px;
  }

  .hero-actions,
  .floating-techs {
    justify-content: center;
  }

  .hero-right {
    width: calc(100vw - 24px);

    margin-inline: calc((100% - (100vw - 24px)) / 2);
  }

  .lottie-stage {
    width: 100%;

    min-height: 430px;

    padding: 8px 0;
  }

  .lottie-card {
    width: 100%;

    min-height: 410px;

    padding: 16px 12px;

    border-radius: 28px;
  }

  .hero-lottie {
    width: min(100%, 380px);

    height: 340px;
  }

  .lottie-caption {
    width: min(calc(100% - 28px), 360px);
  }

  .skills-grid,
  .projects-showcase {
    grid-template-columns: 1fr;
  }

  .about-side {
    grid-template-columns: 1fr;
  }

  .reveal-left,
  .reveal-right {
    transform: translateY(22px);
  }

  .reveal.in-view {
    transform: translateY(0);
  }

  .services-layout {
    grid-template-columns: 1fr;
  }

  .services-lottie-wrap {
    min-height: 300px;
  }

  .services-grid {
    grid-template-columns: 1fr;
  }

  .process-grid {
    grid-template-columns: 1fr;
  }
}

/* =========================================================
   640
========================================================= */
@media (max-width: 640px) {
  .app {
    padding-top: 64px;
  }

  .container {
    width: calc(100% - 16px);
  }

  .navbar {
    backdrop-filter: blur(12px);

    -webkit-backdrop-filter: blur(12px);
  }

  .brand {
    gap: 9px;
  }

  .brand-mark {
    width: 34px;
    height: 34px;
  }

  .brand-copy {
    max-width: 145px;
  }

  .brand-copy strong {
    font-size: 14px;
  }

  .brand-role {
    font-size: 9px;
  }

  .hero {
    padding-top: 25px;
  }

  .hero-title {
    font-size: 29px;
  }

  .hero-subtitle {
    font-size: 16px;
  }

  .hero-desc {
    font-size: 12.8px;
  }

  .hero-actions,
  .contact-actions {
    flex-direction: column;

    align-items: center;
  }

  .premium-btn {
    width: min(100%, 290px);
  }

  .floating-techs span {
    padding: 7px 10px;

    font-size: 10px;
  }

  .lottie-stage {
    min-height: 405px;

    padding: 6px 0;
  }

  .lottie-card {
    width: 100%;

    min-height: 390px;

    padding: 14px 10px;

    border-radius: 24px;
  }

  .hero-lottie {
    width: min(100%, 345px);

    height: 320px;
  }

  .lottie-caption {
    width: min(calc(100% - 24px), 340px);

    padding: 9px 11px;
  }

  .section {
    padding: 68px 0;
  }

  .section-heading {
    margin-bottom: 35px;
  }

  .section-heading h2,
  .about-main h2,
  .showcase-content h2,
  .contact-shell h2 {
    font-size: 25px;
  }

  .section-heading p,
  .about-main p,
  .showcase-content > p,
  .contact-shell p {
    font-size: 12.5px;
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

  .showcase-points strong {
    font-size: 12px;
  }

  .showcase-card {
    padding: 11px 13px;
  }

  .showcase-card strong {
    font-size: 14px;
  }

  .showcase-card span {
    font-size: 10px;
  }

  .mobile-actions {
    grid-template-columns: 1fr;
  }

  .mobile-menu-panel {
    padding: 18px;
  }

  .services-lottie-wrap {
    min-height: 260px;
  }

  .services-lottie-card {
    width: 220px;
  }

  .service-card {
    min-height: 205px;

    padding: 19px;
  }

  .site-footer-inner {
    min-height: 90px;

    flex-direction: column;
    justify-content: center;

    text-align: center;
  }

  .site-footer nav {
    gap: 12px;
  }
}

/* =========================================================
   520
========================================================= */
@media (max-width: 520px) {
  .brand-copy {
    max-width: 115px;
  }

  .brand-copy strong {
    font-size: 12px;
  }

  .brand-role {
    font-size: 9px;
  }

  .hero-chip {
    font-size: 10px;

    padding: 7px 10px;

    max-width: calc(100vw - 30px);

    line-height: 1.45;
  }

  .hero-title {
    font-size: 25px;
  }

  .hero-subtitle {
    font-size: 15px;
  }

  .hero-desc {
    font-size: 12px;
  }

  .hero-right {
    width: calc(100vw - 28px);

    margin-inline: calc((100% - (100vw - 28px)) / 2);
  }

  .lottie-stage {
    min-height: 400px;
  }

  .lottie-card {
    min-height: 382px;
  }

  .hero-lottie {
    width: min(100%, 315px);

    height: 295px;
  }

  .lottie-caption {
    width: calc(100% - 20px);

    max-width: 320px;

    padding: 8px 10px;

    border-radius: 13px;
  }

  .lottie-caption strong {
    font-size: 10px;
  }

  .lottie-caption small {
    font-size: 10px;
  }

  .lottie-caption-dot {
    width: 7px;
    height: 7px;
  }

  .showcase-media {
    min-height: 280px;
  }

  .showcase-image-frame img {
    height: 280px;
  }

  .nav-shell {
    border-radius: 16px;
  }

  .brand-mark {
    width: 34px;
    height: 34px;
  }

  .brand-copy {
    max-width: calc(100vw - 94px);
  }

  .brand-copy strong {
    font-size: 12px;
  }

  .brand-role {
    font-size: 9px;
  }
}

/* =========================================================
   420
========================================================= */
@media (max-width: 420px) {
  .nav-shell {
    padding-inline: 0;
  }

  .brand {
    gap: 8px;
  }

  .brand-mark {
    width: 34px;
    height: 34px;
  }

  .brand-copy {
    max-width: calc(100vw - 112px);
  }

  .brand-copy strong {
    font-size: 11px;
  }

  .brand-role {
    font-size: 9px;
  }

  .hamburger-btn {
    width: 27px;
    height: 21px;
  }

  .hero-title {
    font-size: 22px;
  }

  .hero-subtitle {
    font-size: 14px;
  }

  .hero-desc {
    font-size: 11.5px;
  }

  .showcase-image-frame img {
    height: 245px;
  }

  .showcase-media {
    min-height: 245px;
  }

  .showcase-card {
    padding: 9px 10px;
  }

  .showcase-card strong {
    font-size: 12px;
  }

  .showcase-card span {
    font-size: 10px;
  }
}

/* =========================================================
   RESPONSIVE NAVBAR
========================================================= */

.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: opacity 0.28s ease;
}

.mobile-menu-enter-active .mobile-menu-panel,
.mobile-menu-leave-active .mobile-menu-panel {
  transition:
    transform 0.38s cubic-bezier(0.22, 1, 0.36, 1),
    opacity 0.28s ease;
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
}

.mobile-menu-enter-from .mobile-menu-panel,
.mobile-menu-leave-to .mobile-menu-panel {
  transform: translateX(-100%);
  opacity: 0;
}

.app.lang-fa .mobile-menu-enter-from .mobile-menu-panel,
.app.lang-fa .mobile-menu-leave-to .mobile-menu-panel {
  transform: translateX(100%);
}

@media (max-width: 1140px) {
  .nav-shell {
    grid-template-columns:
      minmax(0, 1fr)
      auto;

    min-height: 58px;

    gap: 10px;
  }

  .nav-col-center,
  .nav-actions {
    display: none;
  }

  .nav-col-start {
    grid-column: 1;
  }

  .nav-col-end {
    grid-column: 2;
    flex: 0 0 auto;
  }

  .hamburger-btn {
    display: flex;
  }

  .brand-copy {
    width: min(190px, 28vw);
  }

  .app {
    padding-top: 74px;
  }

  .full-screen-section,
  .hero {
    min-height: calc(100dvh - 74px);
  }

  .projects-showcase {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 760px) {
  .app {
    padding-top: 84px;
  }

  .full-screen-section,
  .hero {
    min-height: calc(100dvh - 84px);
  }

  .navbar {
    padding: 8px 0;
  }

  .nav-shell {
    min-height: 64px;

    padding: 7px 8px;

    border-radius: 18px;
  }

  .brand {
    gap: 9px;
  }

  .brand-mark {
    width: 36px;
    height: 36px;
  }

  .brand-copy {
    width: auto;

    max-width: calc(100vw - 105px);
  }

  .brand-copy strong {
    font-size: 13px;
  }

  .brand-role {
    font-size: 9px;
  }

  .projects-showcase {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 400px) {
  .brand-copy {
    max-width: calc(100vw - 88px);
  }

  .brand-copy strong {
    font-size: 11px;
  }

  .brand-role {
    font-size: 9px;
  }
}

/* =========================================================
   FINAL MOBILE HERO SPACING
========================================================= */
@media (max-width: 760px) {
  .scroll-indicator {
    display: none !important;
  }

  .hero-right {
    width: calc(100vw - 24px);

    margin-inline: calc((100% - (100vw - 24px)) / 2);
  }

  .lottie-stage {
    width: 100%;

    min-height: 430px;
  }

  .lottie-card {
    width: 100%;

    min-height: 410px;
  }

  .lottie-caption {
    margin-top: 18px;
  }
}

/* =========================================================
   TABLET
========================================================= */
@media (min-width: 761px) and (max-width: 980px) {
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

  .section {
    padding: 92px 0;
  }
}

/* =========================================================
   FINAL RESPONSIVE SYSTEM
   Keeps mobile/tablet geometry predictable after the legacy breakpoints above.
========================================================= */
.app {
  min-width: 0;
  overflow-x: clip;
}

img,
svg,
video,
canvas {
  max-width: 100%;
}

.container {
  width: min(1240px, calc(100% - 32px));
}

.section {
  padding-block: clamp(76px, 9vw, 118px);
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

  .skills-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .process-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .showcase-grid,
  .about-grid,
  .services-layout {
    grid-template-columns: 1fr;
  }

  .showcase-content {
    max-width: none;
  }

  .showcase-media {
    min-height: 400px;
  }

  .showcase-image-frame img {
    height: min(58vw, 420px);
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

  .hero {
    padding: 28px 0 42px;
  }

  .hero-grid {
    grid-template-columns: 1fr;
    gap: 34px;
  }

  .hero-left {
    text-align: center;
  }

  .hero-chip {
    margin-inline: auto;
    max-width: min(100%, 420px);
    justify-content: center;
  }

  .hero-title {
    justify-content: center;
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
    margin-inline: auto;
    font-size: 13px;
    line-height: 1.95;
  }

  .hero-actions,
  .floating-techs {
    justify-content: center;
  }

  .hero-actions {
    gap: 10px;
  }

  .premium-btn {
    min-height: 50px;
  }

  .hero-right {
    width: 100%;
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
    padding: 0;
    gap: 0;
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

  .skills-grid {
    grid-template-columns: 1fr;
  }

  .skill-panel,
  .about-card,
  .process-card,
  .service-card {
    border-radius: 20px;
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
    left: 10px;
    bottom: 12px;
  }

  .showcase-card-2 {
    right: 10px;
    top: 12px;
  }

  .about-side {
    grid-template-columns: 1fr;
  }

  .services-layout {
    gap: 26px;
  }

  .services-grid,
  .process-grid {
    grid-template-columns: 1fr;
  }

  .services-lottie-wrap {
    min-height: 240px;
  }

  .services-lottie-card {
    width: min(230px, 70vw);
  }

  .contact-shell {
    text-align: center;
  }

  .contact-content {
    text-align: center;
  }

  .contact-actions {
    justify-content: center;
  }

  .mobile-menu-overlay {
    align-items: flex-end;
    padding: 10px;
  }

  .mobile-menu-panel {
    width: 100%;
    max-height: min(82dvh, 720px);
    overflow-y: auto;
    border-radius: 24px 24px 18px 18px;
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

  .hero {
    padding-top: 22px;
  }

  .hero-chip {
    width: 100%;
    font-size: 10px;
    padding: 8px 10px;
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

  .hero-actions {
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

  .floating-techs span,
  .skill-cloud span {
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

  .site-footer-inner {
    padding-block: 20px;
    gap: 13px;
  }

  .site-footer nav {
    flex-wrap: wrap;
    justify-content: center;
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
    min-height: 58px;
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

  .brand-copy {
    max-width: calc(100vw - 98px);
  }

  .brand-copy strong {
    font-size: 10.5px;
  }

  .brand-role {
    font-size: 8px;
  }

  .hamburger-btn {
    width: 28px;
    height: 28px;
  }

  .hero-lottie {
    min-height: 210px;
    height: 67vw;
  }

  .lottie-caption {
    max-width: 300px;
  }
}

@media (hover: none), (pointer: coarse) {
  .mouse-trail-layer {
    display: none;
  }
}

/* =========================================================
   ACCESSIBILITY / TOUCH
========================================================= */

.app h2,
.app h3 {
  font-family: var(--font-display);
}

.app :focus-visible {
  outline: 2px solid var(--primary);
  outline-offset: 3px;
  border-radius: 8px;
}

.hamburger-btn {
  position: relative;
}

.hamburger-btn::after {
  content: "";
  position: absolute;
  inset: -12px;
}

.mobile-close {
  width: 44px;
  height: 44px;
}

.app.lang-fa .hero-title {
  line-height: 1.32;
}

.app.lang-fa .hero-name {
  padding-bottom: 0.24em;
}

.app.lang-fa .hero-title,
.app.lang-fa .hero-name,
.app.lang-fa .section-heading h2,
.app.lang-fa .about-main h2,
.app.lang-fa .showcase-content h2,
.app.lang-fa .contact-shell h2 {
  letter-spacing: 0;
}

/* =========================================================
   TOUCH / REDUCED MOTION
========================================================= */
@media (hover: none) and (pointer: coarse) {
  .mouse-trail-layer {
    display: none;
  }

  .premium-btn:hover,
  .skill-panel:hover,
  .service-card:hover,
  .about-card:hover,
  .process-card:hover,
  .showcase-points > div:hover,
  .pc:hover {
    transform: none;
  }
}

/* =========================================================
   FINAL MOBILE DRAWER DIRECTION OVERRIDE
   EN opens from the left, FA opens from the right.
========================================================= */
@media (max-width: 760px) {
  .mobile-menu-overlay {
    align-items: stretch !important;
    padding: 0 !important;
  }

  .app.lang-en .mobile-menu-overlay {
    justify-content: flex-start !important;
  }

  .app.lang-fa .mobile-menu-overlay {
    justify-content: flex-end !important;
  }

  .mobile-menu-panel {
    width: min(390px, 88vw) !important;
    height: 100dvh !important;
    min-height: 100dvh !important;
    max-height: none !important;
    margin: 0 !important;
    overflow-y: auto !important;
    border-top: 0 !important;
    border-bottom: 0 !important;
    border-left: 0 !important;
    border-right: 1px solid var(--border) !important;
    border-radius: 0 26px 26px 0 !important;
    box-shadow: 25px 0 70px rgba(0, 0, 0, 0.32) !important;
  }

  .app.lang-fa .mobile-menu-panel {
    border-left: 1px solid var(--border) !important;
    border-right: 0 !important;
    border-radius: 26px 0 0 26px !important;
    box-shadow: -25px 0 70px rgba(0, 0, 0, 0.32) !important;
  }

  .app.light .mobile-menu-panel {
    box-shadow: 25px 0 70px rgba(55, 75, 105, 0.12) !important;
  }

  .app.light.lang-fa .mobile-menu-panel {
    box-shadow: -25px 0 70px rgba(55, 75, 105, 0.12) !important;
  }
}

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

  .reveal {
    opacity: 1 !important;

    transform: none !important;
  }

  .mouse-trail-layer {
    display: none !important;
  }

  .code-mark-path {
    animation: none !important;

    stroke-dashoffset: 0;

    opacity: 1;
  }

  .code-mark-shift,
  .code-mark-aurora,
  .code-mark-rain line,
  .code-mark-pulse,
  .code-mark-shine,
  .led-ring {
    animation: none !important;
  }
}

/* =========================================================
   FINAL POLISH — MOBILE MENU / LOADER / POINTER TRAIL
========================================================= */
@media (max-width: 1140px) {
  .nav-shell {
    width: min(100%, 1240px);
  }
}

@media (max-width: 760px) {
  .navbar {
    padding: 7px 0;
  }

  .nav-shell {
    width: calc(100% - 16px);
    min-height: 58px;
    padding: 6px 8px;
    border-radius: 17px;
    gap: 8px;
  }

  .nav-col-start {
    min-width: 0;
  }

  .brand {
    max-width: 100%;
    gap: 8px;
  }

  .brand-mark {
    width: 35px;
    height: 35px;
    border-radius: 10px;
  }

  .brand-copy {
    width: auto;
    max-width: min(62vw, 260px);
  }

  .brand-copy strong {
    font-size: clamp(11px, 3.1vw, 13px);
  }

  .brand-role {
    max-width: 100%;
    font-size: 8px;
  }

  .hamburger-btn {
    width: 36px;
    height: 36px;
    padding: 8px;
    justify-content: center;
    gap: 4px;
    border: 1px solid var(--border);
    border-radius: 10px;
    background: var(--surface);
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.05);
  }

  .hamburger-btn span {
    width: 16px;
    height: 2px;
  }

  .hamburger-btn.active span:nth-child(1) {
    transform: translateY(6px) rotate(45deg);
  }

  .hamburger-btn.active span:nth-child(3) {
    transform: translateY(-6px) rotate(-45deg);
  }

  .mobile-menu-overlay {
    align-items: stretch;
    padding: 8px;
    background: rgba(4, 10, 18, 0.58);
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
  }

  .app.light .mobile-menu-overlay {
    background: rgba(226, 234, 243, 0.58);
  }

  .mobile-menu-panel {
    width: min(410px, calc(100vw - 16px));
    height: calc(100dvh - 16px);
    max-height: none;
    margin: 0;
    padding: 18px;
    border-radius: 24px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    box-shadow:
      0 25px 80px rgba(0, 0, 0, 0.32),
      inset 0 1px 0 rgba(255, 255, 255, 0.06);
  }

  .app.lang-en .mobile-menu-overlay {
    justify-content: flex-end;
  }

  .app.lang-fa .mobile-menu-overlay {
    justify-content: flex-start;
  }

  .mobile-menu-header {
    flex: 0 0 auto;
    padding-bottom: 15px;
  }

  .mobile-menu-brand {
    min-width: 0;
  }

  .mobile-menu-mark {
    width: 42px;
    height: 42px;
    border-radius: 13px;
    background:
      radial-gradient(
        circle at 30% 20%,
        rgba(255, 255, 255, 0.18),
        transparent 52%
      ),
      linear-gradient(135deg, var(--primary), var(--primary-2));
    box-shadow: 0 10px 24px rgba(88, 216, 255, 0.12);
  }

  .mobile-lottie {
    width: 34px;
    height: 34px;
  }

  .mobile-menu-brand strong {
    font-size: 14px;
  }

  .mobile-menu-brand span {
    font-size: 9px;
  }

  .mobile-close {
    width: 40px;
    height: 40px;
    border-radius: 12px;
  }

  .mobile-nav-links {
    flex: 1 1 auto;
    min-height: 0;
    overflow-y: auto;
    overscroll-behavior: contain;
    padding: 16px 2px;
    gap: 7px;
  }

  .mobile-nav-links::-webkit-scrollbar {
    width: 4px;
  }

  .mobile-nav-links::-webkit-scrollbar-thumb {
    border-radius: 99px;
    background: rgba(116, 105, 255, 0.24);
  }

  .mobile-nav-links a {
    position: relative;
    min-height: 52px;
    display: flex;
    align-items: center;
    padding: 13px 14px;
    border: 1px solid transparent;
    border-radius: 15px;
    background: transparent;
    font-size: 15px;
  }

  .mobile-nav-links a::after {
    content: "";
    position: absolute;
    inset-inline-end: 14px;
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--primary);
    box-shadow: 0 0 10px rgba(88, 216, 255, 0.45);
    opacity: 0;
    transform: scale(0.5);
    transition: 0.22s ease;
  }

  .mobile-nav-links a:hover,
  .mobile-nav-links a:focus-visible {
    border-color: var(--border);
    background: linear-gradient(
      90deg,
      rgba(88, 216, 255, 0.08),
      rgba(116, 105, 255, 0.05)
    );
    transform: none;
  }

  .mobile-nav-links a:hover::after,
  .mobile-nav-links a:focus-visible::after {
    opacity: 1;
    transform: scale(1);
  }

  .mobile-actions {
    flex: 0 0 auto;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    padding-top: 13px;
    border-top: 1px solid var(--border);
  }

  .mobile-actions .switch-btn {
    height: 44px;
  }
}

@media (max-width: 460px) {
  .mobile-menu-panel {
    width: 100%;
    height: 100%;
    border-radius: 20px;
    padding: 16px;
  }

  .mobile-actions {
    grid-template-columns: 1fr;
  }

  .mobile-actions .switch-btn {
    height: 43px;
  }
}

@media (max-width: 360px) {
  .brand-copy {
    max-width: calc(100vw - 96px);
  }

  .brand-role {
    display: none;
  }

  .mobile-menu-panel {
    padding: 14px;
  }

  .mobile-menu-header {
    padding-bottom: 12px;
  }

  .mobile-nav-links {
    padding-block: 12px;
  }

  .mobile-nav-links a {
    min-height: 48px;
    font-size: 14px;
  }
}

@media (max-height: 650px) and (max-width: 760px) {
  .mobile-menu-panel {
    height: 100dvh;
    min-height: 100dvh;
    border-radius: 0 18px 18px 0;
  }

  .app.lang-fa .mobile-menu-panel {
    border-radius: 18px 0 0 18px;
  }

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

@media (prefers-reduced-motion: reduce) {
  .trail-star,
  .initial-loader-animation::after,
  .initial-loader-animation::before,
  .initial-loader-dot,
  .initial-loader-atmosphere span {
    animation: none !important;
    transition: none !important;
  }
}

@keyframes navLanguageFlip {
  0% {
    opacity: 0.88;
    transform: translateX(0) scaleX(0.985);
  }
  55% {
    opacity: 1;
    transform: translateX(0) scaleX(1.012);
  }
  100% {
    opacity: 1;
    transform: translateX(0) scaleX(1);
  }
}
</style>
