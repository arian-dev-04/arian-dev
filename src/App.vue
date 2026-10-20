<template>
  <div
    class="app"
    :class="[
      theme,
      isFa ? 'lang-fa' : 'lang-en',
      {
        'is-loading': isInitialLoading || isRevealing,
        'is-revealing': isRevealing,
      },
    ]"
    :dir="isFa ? 'rtl' : 'ltr'"
    :lang="isFa ? 'fa' : 'en'"
  >
    <!-- =========================================================
         INITIAL LOADER (lightweight geometric frame + one lazy Lottie)
    ========================================================== -->
    <Transition name="initial-loader" @after-leave="destroyLoaderLottie">
      <div
        v-if="isInitialLoading"
        class="initial-loader"
        role="status"
        aria-live="polite"
        :aria-label="isFa ? 'در حال آماده‌سازی سایت' : 'Initializing website'"
      >
        <div class="initial-loader-aurora" aria-hidden="true">
          <span class="il-blob il-blob-1"></span>
          <span class="il-blob il-blob-2"></span>
          <span class="il-blob il-blob-3"></span>
        </div>

        <div class="initial-loader-center">
          <div class="initial-loader-animation" aria-hidden="true">
            <span class="initial-loader-frame initial-loader-frame-1"></span>

            <span class="initial-loader-corner corner-tl"></span>
            <span class="initial-loader-corner corner-tr"></span>
            <span class="initial-loader-corner corner-bl"></span>
            <span class="initial-loader-corner corner-br"></span>
            <span class="initial-loader-scan"></span>

            <div
              ref="loaderLottieContainer"
              class="initial-loader-lottie"
            ></div>
          </div>

          <h2 class="initial-loader-title">{{ loaderTitle }}</h2>
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

          <div class="initial-loader-chips" aria-hidden="true">
            <span v-for="item in TECHS" :key="item">{{ item }}</span>
          </div>
        </div>
      </div>
    </Transition>

    <!-- =========================================================
         OPENING REVEAL (runs right after the loader finishes)
    ========================================================== -->
    <div v-if="isRevealing" class="portal" aria-hidden="true">
      <span class="portal-door portal-door-top"></span>
      <span class="portal-door portal-door-bottom"></span>
      <span class="portal-flare"></span>
      <span class="portal-line"></span>
    </div>

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
    <a class="skip-link" href="#main-content">{{
      isFa ? "پرش به محتوا" : "Skip to content"
    }}</a>
    <header class="navbar" :class="{ scrolled: isScrolled }">
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
            <a
              v-for="item in NAV_ITEMS"
              :key="item"
              :href="`#${item}`"
              :class="{ active: activeSection === item }"
              :aria-current="activeSection === item ? 'true' : undefined"
            >
              {{ t.nav[item] }}
            </a>
          </nav>
        </div>

        <div class="nav-col nav-col-end">
          <div class="nav-actions">
            <button
              class="lang-toggle"
              :class="{ fa: isFa }"
              type="button"
              :aria-label="isFa ? 'Switch to English' : 'تغییر به فارسی'"
              @click="toggleLang"
            >
              <span class="lang-thumb" aria-hidden="true"></span>
              <span class="lang-opt" :class="{ on: !isFa }">EN</span>
              <span class="lang-opt" :class="{ on: isFa }">فا</span>
            </button>

            <button
              class="theme-toggle"
              :class="{ light: theme === 'light' }"
              type="button"
              :aria-label="
                theme === 'dark'
                  ? 'Switch to light mode'
                  : 'Switch to dark mode'
              "
              @click="toggleTheme"
            >
              <svg
                class="theme-sun"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                aria-hidden="true"
              >
                <circle cx="12" cy="12" r="4" />
                <path
                  d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"
                />
              </svg>
              <svg
                class="theme-moon"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                aria-hidden="true"
              >
                <path
                  d="M21 12.8A8.5 8.5 0 1 1 11.2 3a6.6 6.6 0 0 0 9.8 9.8Z"
                />
              </svg>
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
        <span class="nav-progress" aria-hidden="true"
          ><i ref="navProgressEl"></i
        ></span>
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
              :class="{ active: activeSection === item }"
              @click="menuOpen = false"
            >
              {{ t.nav[item] }}
            </a>
          </nav>

          <div class="mobile-actions">
            <button
              v-for="action in navActions"
              :key="action.key"
              class="switch-btn"
              type="button"
              :aria-label="action.aria"
              @click="action.onClick"
            >
              <span class="switch-inner">
                <span class="switch-icon-wrap" aria-hidden="true">
                  <svg
                    v-if="action.key === 'lang'"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <circle cx="12" cy="12" r="9" />
                    <path
                      d="M3 12h18M12 3c2.6 2.6 3.9 5.6 3.9 9s-1.3 6.4-3.9 9c-2.6-2.6-3.9-5.6-3.9-9S9.4 5.6 12 3Z"
                    />
                  </svg>
                  <svg
                    v-else-if="theme === 'dark'"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <circle cx="12" cy="12" r="4" />
                    <path
                      d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"
                    />
                  </svg>
                  <svg
                    v-else
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path
                      d="M21 12.8A8.5 8.5 0 1 1 11.2 3a6.6 6.6 0 0 0 9.8 9.8Z"
                    />
                  </svg>
                </span>
                <span class="switch-text" :key="action.label">{{
                  action.label
                }}</span>
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
                    <strong>{{ t.brand }}</strong>
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
              class="skill-panel spot reveal"
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
                class="service-card spot"
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
              class="about-card spot reveal"
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
                class="pc spot"
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
              class="process-card spot reveal"
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
                  :href="CONTACT_TELEGRAM_URL"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="contact-link contact-link-telegram"
                  :aria-label="t.contact.telegramBtn"
                >
                  <span class="contact-link-icon" aria-hidden="true">
                    <svg viewBox="0 0 24 24" focusable="false">
                      <path
                        d="M21.45 4.05 18.3 19.1c-.24 1.08-.87 1.35-1.76.84l-4.86-3.58-2.34 2.25c-.26.26-.48.48-.98.48l.35-4.95 9.02-8.15c.39-.35-.09-.54-.6-.19L6.02 12.94 1.2 11.43c-1.05-.33-1.07-1.05.22-1.57L20.28 2.5c.88-.32 1.65.21 1.17 1.55Z"
                        fill="currentColor"
                      />
                    </svg>
                  </span>

                  <span class="contact-link-text">
                    <small>{{ t.contact.telegramHint }}</small>
                    <strong dir="ltr">{{ CONTACT_TELEGRAM_HANDLE }}</strong>
                  </span>

                  <span class="contact-link-arrow" aria-hidden="true">
                    <svg viewBox="0 0 24 24" fill="none" focusable="false">
                      <path
                        d="M7 17 17 7M9 7h8v8"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                    </svg>
                  </span>
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

    <button
      class="to-top"
      :class="{ show: showTop }"
      type="button"
      :aria-label="isFa ? 'بازگشت به بالا' : 'Back to top'"
      :tabindex="showTop ? 0 : -1"
      @click="scrollToTop"
    >
      <svg class="to-top-ring" viewBox="0 0 44 44" aria-hidden="true">
        <circle class="ring-track" cx="22" cy="22" r="20" />
        <circle ref="topRingEl" class="ring-bar" cx="22" cy="22" r="20" />
      </svg>
      <svg
        class="to-top-arrow"
        viewBox="0 0 24 24"
        fill="none"
        aria-hidden="true"
      >
        <path
          d="M12 19V5M5 12l7-7 7 7"
          stroke="currentColor"
          stroke-width="2.2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </button>
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
// Replace these two values with your real Telegram link and username.
const CONTACT_TELEGRAM_URL = "https://t.me/Ariankdev";
const CONTACT_TELEGRAM_HANDLE = "@Ariankdev";
const SKILL_META = [
  { name: "Vue.js", icon: "Vu" },
  { name: "Flutter", icon: "Fl" },
  { name: "JavaScript", icon: "JS" },
  { name: "Dart", icon: "Dt" },
  { name: "Go", icon: "Go" },
  { name: "UI / UX", icon: "UX" },
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

/* Opening reveal uses one GPU-friendly blue curtain instead of many bubbles. */

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
      title1: "توسعه‌دهنده‌ی",
      name: "محصولات دیجیتال",
      title2: "با تمرکز بر طراحی و تجربه کاربری",
      typingPrefix: "مسلط به ",
      description:
        "من روی ساخت تجربه‌های دیجیتال شیک، سریع، روان و کاربرمحور تمرکز دارم؛ از رابط‌های مدرن وب با Vue گرفته تا اپلیکیشن‌های Flutter، منطق با JavaScript و Dart، کار با Go و طراحی حرفه‌ای UI/UX.",
      primaryBtn: "مشاهده پروژه‌ها",
      secondaryBtn: "ارتباط با من",
      role: "Frontend, Flutter, Go Developer",
    },
    skillsSection: {
      tag: "زبان‌ها و مهارت‌ها",
      title: "ابزارهایی برای تبدیل ایده به واقعیت",
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
      title: "از طراحی تا خلق تجربه‌های دیجیتال",
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
      title: "مهارت‌هایی برای خلق تجربه‌های دیجیتال",
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
      telegramBtn: "تلگرام",
      telegramHint: "پیام مستقیم در تلگرام",
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
      title1: "Digital Product",
      name: "Developer",
      title2: "Focused on Design & User Experience",
      typingPrefix: "Skilled in ",
      description:
        "I focus on crafting elegant, fast, smooth and user-centered digital experiences — from modern web interfaces with Vue to Flutter apps, logic with JavaScript and Dart, working with Go, and professional UI/UX design.",
      primaryBtn: "View Projects",
      secondaryBtn: "Contact Me",
      role: "Frontend, Flutter, Go & UI/UX Developer",
    },
    skillsSection: {
      tag: "Languages & Skills",
      title: "Tools for Turning Ideas into Reality",
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
      title: "From Design to Digital Experiences",
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
      title: "Skills for Crafting Digital Experiences",
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
      telegramBtn: "Telegram",
      telegramHint: "Message me on Telegram",
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
const isRevealing = ref(false);
const activeFilter = ref("all");
const displayedWord = ref("");
const trailEnabled = ref(false);
const isScrolled = ref(false);
const showTop = ref(false);
const activeSection = ref("home");

const isFa = computed(() => lang.value === "fa");
const t = computed(() => translations[lang.value]);
const loaderTitle = computed(() => t.value.loader);
const currentYear = new Date().getFullYear();

const toggleLang = () => {
  lang.value = isFa.value ? "en" : "fa";
};
const applyTheme = () => {
  theme.value = theme.value === "dark" ? "light" : "dark";
};

/* =========================================================
   THEME SWITCH
   · Desktop  – circular reveal that expands from the clicked button.
   · Phones / touch / very large screens – quiet cross-dissolve.
   Both use the View Transitions API and animate only clip-path / opacity;
   browsers without it (or with reduced motion) switch instantly.
========================================================= */
const REVEAL_MS = 950;
const REVEAL_EASING = "cubic-bezier(0.22, 1, 0.36, 1)";
/* keep in sync with `theme-fade-in` in the unscoped <style> */
const FADE_MS = 620;
/* Beyond this viewport area (CSS px², ≈ 2560×1000) repainting a full-screen
   clip-path every frame costs too much, so the cheaper cross-dissolve is used. */
const REVEAL_MAX_AREA = 2_500_000;

const softThemeQuery =
  typeof window !== "undefined"
    ? window.matchMedia("(hover: none), (pointer: coarse), (max-width: 760px)")
    : null;

let themeTransitioning = false;

/* Freeze / resume every running animation (CSS + Lottie). The Lottie inside
   the mobile menu keeps playing. */
const setMotionFrozen = (frozen) => {
  document.documentElement.classList.toggle("theme-switching", frozen);

  lottieEntries.forEach((entry) => {
    if (frozen) {
      if (entry.el !== mobileLottieEl) entry.instance?.pause();
    } else if (!prefersReducedMotion && entry.visible) {
      entry.instance?.play();
    }
  });
};

const nextFrames = (count = 2) =>
  new Promise((resolve) => {
    const step = (left) =>
      requestAnimationFrame(() => (left <= 1 ? resolve() : step(left - 1)));

    step(count);
  });

const toggleTheme = async (event) => {
  /* a click during a running transition just flips the theme right away */
  if (
    themeTransitioning ||
    !document.startViewTransition ||
    prefersReducedMotion
  ) {
    const quiet = themeTransitioning === false;

    if (quiet) setMotionFrozen(true);
    applyTheme();

    if (quiet) {
      await nextFrames(2);
      setMotionFrozen(false);
    }

    return;
  }

  const root = document.documentElement;
  const soft =
    softThemeQuery?.matches ||
    window.innerWidth * window.innerHeight > REVEAL_MAX_AREA;

  /* reveal origin: read before the transition starts */
  let x = 0;
  let y = 0;
  let radius = 0;

  if (!soft) {
    const rect = event?.currentTarget?.getBoundingClientRect?.();

    x = rect ? rect.left + rect.width / 2 : window.innerWidth / 2;
    y = rect ? rect.top + rect.height / 2 : 0;
    radius = Math.hypot(
      Math.max(x, window.innerWidth - x),
      Math.max(y, window.innerHeight - y),
    );
  }

  themeTransitioning = true;

  let resumeTimer = 0;

  try {
    if (soft) {
      root.classList.add("theme-fade");
      setMotionFrozen(true);
    }

    const transition = document.startViewTransition(async () => {
      applyTheme();
      await nextTick();
    });

    await transition.ready;

    if (soft) {
      /* resume page animations a little before the fade ends */
      resumeTimer = window.setTimeout(
        () => setMotionFrozen(false),
        FADE_MS * 0.7,
      );
    } else {
      root.animate(
        {
          clipPath: [
            `circle(0px at ${x}px ${y}px)`,
            `circle(${radius}px at ${x}px ${y}px)`,
          ],
        },
        {
          duration: REVEAL_MS,
          easing: REVEAL_EASING,
          pseudoElement: "::view-transition-new(root)",
        },
      );
    }

    await transition.finished;
  } catch {
    /* skipped or interrupted: the theme is already applied */
  } finally {
    clearTimeout(resumeTimer);
    root.classList.remove("theme-fade");
    if (soft) setMotionFrozen(false);
    themeTransitioning = false;
  }
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

/* language + theme buttons, rendered in the navbar and the mobile menu */
const navActions = computed(() => [
  {
    key: "lang",
    label: isFa.value ? "English" : "فارسی",
    aria: isFa.value ? "Switch to English" : "تغییر به فارسی",
    onClick: toggleLang,
  },
  {
    key: "theme",
    label:
      theme.value === "dark" ? t.value.actions.light : t.value.actions.dark,
    aria:
      theme.value === "dark" ? "Switch to light mode" : "Switch to dark mode",
    onClick: toggleTheme,
  },
]);

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
const navProgressEl = ref(null);
const topRingEl = ref(null);

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

const AUTHOR = "Arian Kalantari";

const STRUCTURED_DATA = {
  "@context": "https://schema.org",
  "@type": "ProfilePage",
  mainEntity: {
    "@type": "Person",
    name: AUTHOR,
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
};

const metaName = (name, content) => [`meta[name="${name}"]`, { name, content }];
const metaProp = (property, content) => [
  `meta[property="${property}"]`,
  { property, content },
];

const updateSeo = () => {
  const { title, description, locale } = t.value.seo;

  document.title = title;

  [
    metaName("description", description),
    metaName("robots", ROBOTS),
    metaName("googlebot", ROBOTS),
    metaName("author", AUTHOR),
    metaProp("og:title", title),
    metaProp("og:description", description),
    metaProp("og:type", "website"),
    metaProp("og:locale", locale),
    metaProp("og:site_name", AUTHOR),
    metaName("twitter:card", "summary_large_image"),
    metaName("twitter:title", title),
    metaName("twitter:description", description),
  ].forEach(([selector, attributes]) => setMeta(selector, attributes));

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
    script.textContent = JSON.stringify(STRUCTURED_DATA);
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
   MOUSE TRAIL (soft glowing stars)
   Desktop / fine pointer only, so touch devices pay nothing.
========================================================= */
const stars = ref([]);

const PALETTES = {
  dark: [
    { color: "#ffffff", glow: "rgba(255,255,255,0.90)" },
    { color: "#62e8ff", glow: "rgba(98,232,255,0.72)" },
    { color: "#8f7cff", glow: "rgba(143,124,255,0.68)" },
    { color: "#73f4cf", glow: "rgba(115,244,207,0.62)" },
    { color: "#ff8eea", glow: "rgba(255,142,234,0.54)" },
    { color: "#ffd36b", glow: "rgba(255,211,107,0.48)" },
    { color: "#75a9ff", glow: "rgba(117,169,255,0.56)" },
  ],
  light: [
    { color: "#ffffff", glow: "rgba(255,255,255,0.92)" },
    { color: "#7c3aed", glow: "rgba(124,58,237,0.34)" },
    { color: "#0ea5e9", glow: "rgba(14,165,233,0.34)" },
    { color: "#14b8a6", glow: "rgba(20,184,166,0.30)" },
    { color: "#db5cff", glow: "rgba(219,92,255,0.27)" },
    { color: "#f59e0b", glow: "rgba(245,158,11,0.24)" },
  ],
};

let starId = 0;
let lastEmit = 0;
let lastMove = { x: 0, y: 0, time: 0 };

const pick = (arr) => arr[Math.floor(Math.random() * arr.length)];

const createSoftStar = (x, y, spread = 10) => {
  const tone = pick(PALETTES[theme.value]);
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

const hasFinePointer = () =>
  !prefersReducedMotion &&
  window.matchMedia("(hover: hover) and (pointer: fine)").matches;

const setupTrail = () => {
  if (!hasFinePointer()) return;

  trailEnabled.value = true;

  window.addEventListener("mousemove", handleMouseMove, { passive: true });
  window.addEventListener("mouseout", handleMouseOut);
  document.documentElement.addEventListener("mouseleave", clearTrail);
};

const teardownTrail = () => {
  window.removeEventListener("mousemove", handleMouseMove);
  window.removeEventListener("mouseout", handleMouseOut);
  document.documentElement.removeEventListener("mouseleave", clearTrail);
};

/* =========================================================
   CARD SPOTLIGHT
   A single delegated, rAF-throttled listener feeds --mx / --my
   to whichever .spot card is under the pointer.
========================================================= */
let spotFrame = 0;
let spotEvent = null;

const handleSpot = (e) => {
  spotEvent = e;

  if (spotFrame) return;

  spotFrame = requestAnimationFrame(() => {
    spotFrame = 0;

    const card = spotEvent?.target?.closest?.(".spot");

    if (!card) return;

    const rect = card.getBoundingClientRect();

    card.style.setProperty("--mx", `${spotEvent.clientX - rect.left}px`);
    card.style.setProperty("--my", `${spotEvent.clientY - rect.top}px`);
  });
};

const setupSpotlight = () => {
  if (hasFinePointer())
    window.addEventListener("pointermove", handleSpot, { passive: true });
};

/* =========================================================
   SCROLL UI: navbar state, progress line, back-to-top ring, scrollspy
   One passive listener, one rAF per frame, no reactive churn.
========================================================= */
const RING_LENGTH = 125.66;

let scrollQueued = false;
let spyObserver = null;

const updateScrollUi = () => {
  scrollQueued = false;

  const y = window.scrollY;
  const max = document.documentElement.scrollHeight - window.innerHeight;
  const progress = max > 0 ? Math.min(1, y / max) : 0;

  if (navProgressEl.value)
    navProgressEl.value.style.transform = `scaleX(${progress})`;
  if (topRingEl.value)
    topRingEl.value.style.strokeDashoffset = String(
      RING_LENGTH * (1 - progress),
    );

  isScrolled.value = y > 12;
  showTop.value = y > 640;
};

const onScroll = () => {
  if (scrollQueued) return;

  scrollQueued = true;
  requestAnimationFrame(updateScrollUi);
};

const scrollToTop = () =>
  window.scrollTo({
    top: 0,
    behavior: prefersReducedMotion ? "auto" : "smooth",
  });

const setupScrollUi = () => {
  window.addEventListener("scroll", onScroll, { passive: true });
  window.addEventListener("resize", onScroll, { passive: true });
  updateScrollUi();

  spyObserver = new IntersectionObserver(
    (records) => {
      records.forEach(({ target, isIntersecting }) => {
        if (isIntersecting) activeSection.value = target.id;
      });
    },
    { rootMargin: "-45% 0px -50% 0px" },
  );

  NAV_ITEMS.forEach((id) => {
    const el = document.getElementById(id);

    if (el) spyObserver.observe(el);
  });
};

const teardownScrollUi = () => {
  cancelAnimationFrame(spotFrame);
  window.removeEventListener("pointermove", handleSpot);
  window.removeEventListener("scroll", onScroll);
  window.removeEventListener("resize", onScroll);
  spyObserver?.disconnect();
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

/* Open the page with a light-split reveal after the loader fades. */
const REVEAL_PAGE_START_MS = 850;
const REVEAL_TOTAL_MS = 1950;
let revealTimers = [];

const finishInitialLoading = () => {
  if (!isInitialLoading.value) return;

  if (prefersReducedMotion) {
    isInitialLoading.value = false;
    nextTick(startPage);

    return;
  }

  isRevealing.value = true;
  isInitialLoading.value = false;
  window.scrollTo(0, 0);

  revealTimers = [
    window.setTimeout(startPage, REVEAL_PAGE_START_MS),
    window.setTimeout(() => {
      isRevealing.value = false;
    }, REVEAL_TOTAL_MS),
  ];
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
  setupSpotlight();
  setupScrollUi();
});

onBeforeUnmount(() => {
  clearTimeout(typingTimeout);
  cancelAnimationFrame(loaderFrame);
  revealTimers.forEach((id) => clearTimeout(id));

  destroyLoaderLottie();

  lottieEntries.forEach((entry) => {
    entry.disposed = true;
    entry.instance?.destroy();
  });
  lottieEntries.clear();
  lottieObserver?.disconnect();
  revealObserver?.disconnect();

  teardownTrail();
  teardownScrollUi();
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

/* Theme switch. Desktop: circular reveal, the animation itself runs from JS.
   Phones: cross-dissolve (class "theme-fade"), opacity only. */
::view-transition-old(root),
::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;
}

::view-transition-old(root) {
  z-index: 1;
}

::view-transition-new(root) {
  z-index: 2;
}

:root.theme-fade::view-transition-group(root) {
  animation: none;
}

:root.theme-fade::view-transition-new(root) {
  animation: theme-fade-in 0.62s cubic-bezier(0.4, 0, 0.2, 1) both;
}

@keyframes theme-fade-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

/* While the theme fades, freeze every page animation and transition so
   the browser only has to animate the veil itself. */
.theme-switching *,
.theme-switching *::before,
.theme-switching *::after {
  transition: none !important;
  animation-play-state: paused !important;
}

/* touch / small screens: blur layers are the costliest thing to re-capture */
@media (hover: none), (max-width: 760px) {
  .theme-switching *,
  .theme-switching *::before,
  .theme-switching *::after {
    backdrop-filter: none !important;
    -webkit-backdrop-filter: none !important;
  }
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
  --primary-3: #ff8ad8;
  --accent: #6ff0c8;
  --grad: linear-gradient(
    120deg,
    var(--primary) 0%,
    var(--primary-2) 58%,
    var(--primary-3) 125%
  );
  --spot-rgb: 110, 190, 255;
  --progress: linear-gradient(90deg, #2f6fb0, #5b9bd5);
  --progress-solid: #4a8fd0;
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
  --primary-3: #c2398f;
  --accent: #0c9a7e;
  --spot-rgb: 6, 120, 170;
  --progress: linear-gradient(90deg, #1f4f86, #3b78b8);
  --progress-solid: #2a65a3;
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

/* =========================================================
   INITIAL LOADER (v2)
========================================================= */
.initial-loader {
  --il-bg: #050b14;
  --il-text: #f3f8ff;
  --il-soft: #9fb2cc;
  --il-c1: #43cfff;
  --il-c2: #287cf0;
  --il-c3: #a2efff;
  --il-c4: #65b9ff;
  --il-c5: #d9f7ff;
  --il-track: rgba(120, 146, 176, 0.16);
  --il-grid: rgba(88, 216, 255, 0.06);
  --il-chip-bg: rgba(255, 255, 255, 0.05);
  --il-chip-border: rgba(255, 255, 255, 0.1);

  position: fixed;
  inset: 0;
  z-index: 10000;
  display: grid;
  place-items: center;
  overflow: hidden;
  isolation: isolate;
  color: var(--il-text);
  background: var(--il-bg);
}

.app.light .initial-loader {
  --il-bg: #f4f7fc;
  --il-text: #0e1626;
  --il-soft: #51627a;
  --il-c1: #0b8fc2;
  --il-c2: #126ed0;
  --il-c3: #69d4f0;
  --il-c4: #58aaf0;
  --il-c5: #8ce3ff;
  --il-track: rgba(15, 23, 42, 0.1);
  --il-grid: rgba(6, 120, 170, 0.07);
  --il-chip-bg: rgba(255, 255, 255, 0.8);
  --il-chip-border: rgba(15, 23, 42, 0.09);
}

.initial-loader-aurora {
  position: absolute;
  inset: 0;
  z-index: -3;
  overflow: hidden;
}

.il-blob {
  position: absolute;
  width: 62vmax;
  height: 62vmax;
  border-radius: 50%;
  opacity: 0.55;
  will-change: transform;
}

.il-blob-1 {
  top: -28vmax;
  left: -20vmax;
  background: radial-gradient(
    circle,
    color-mix(in srgb, var(--il-c1) 38%, transparent),
    transparent 66%
  );
  animation: ilBlob 14s ease-in-out infinite alternate;
}

.il-blob-2 {
  right: -24vmax;
  bottom: -30vmax;
  background: radial-gradient(
    circle,
    color-mix(in srgb, var(--il-c2) 40%, transparent),
    transparent 66%
  );
  animation: ilBlob 17s ease-in-out infinite alternate-reverse;
}

.il-blob-3 {
  top: 28%;
  left: 38%;
  width: 40vmax;
  height: 40vmax;
  opacity: 0.32;
  background: radial-gradient(
    circle,
    color-mix(in srgb, var(--il-c3) 34%, transparent),
    transparent 66%
  );
  animation: ilBlob 20s ease-in-out infinite alternate;
}

.app.light .il-blob {
  opacity: 0.4;
}

.initial-loader-center {
  position: relative;
  z-index: 2;
  width: min(760px, calc(100vw - 32px));
  padding: clamp(18px, 4vw, 34px);
  text-align: center;
}

.initial-loader-animation {
  --il-scan-distance: clamp(62px, 16vw, 92px);

  position: relative;
  width: clamp(146px, 31vw, 198px);
  height: clamp(146px, 31vw, 198px);
  margin: clamp(28px, 4.5vh, 42px) auto clamp(34px, 5.5vh, 50px);
  display: grid;
  place-items: center;
  isolation: isolate;
}

/* Layered glass frames replace the busy rotating rings. */
.initial-loader-frame {
  position: absolute;
  pointer-events: none;
}

.initial-loader-frame-1 {
  inset: 8px;
  border: 1px solid color-mix(in srgb, var(--il-c1) 38%, transparent);
  border-radius: 30px;
  background: linear-gradient(
    145deg,
    color-mix(in srgb, var(--il-c1) 8%, transparent),
    transparent 52%,
    color-mix(in srgb, var(--il-c2) 7%, transparent)
  );
  box-shadow:
    inset 0 0 28px color-mix(in srgb, var(--il-c1) 5%, transparent),
    0 0 34px color-mix(in srgb, var(--il-c1) 8%, transparent);
  animation: ilFrameFloat 4.6s ease-in-out infinite;
}

.initial-loader-corner {
  position: absolute;
  z-index: 2;
  width: 19px;
  height: 19px;
  border-color: var(--il-c1);
  border-style: solid;
  opacity: 0.9;
  filter: drop-shadow(
    0 0 5px color-mix(in srgb, var(--il-c1) 35%, transparent)
  );
  animation: ilCornerPulse 3.2s ease-in-out infinite;
}

.corner-tl {
  top: 2px;
  left: 2px;
  border-width: 2px 0 0 2px;
  border-radius: 7px 0 0;
}

.corner-tr {
  top: 2px;
  right: 2px;
  border-width: 2px 2px 0 0;
  border-radius: 0 7px 0 0;
  animation-delay: 0.8s;
}

.corner-bl {
  bottom: 2px;
  left: 2px;
  border-width: 0 0 2px 2px;
  border-radius: 0 0 0 7px;
  animation-delay: 1.6s;
}

.corner-br {
  right: 2px;
  bottom: 2px;
  border-width: 0 2px 2px 0;
  border-radius: 0 0 7px;
  animation-delay: 2.4s;
}

.initial-loader-scan {
  position: absolute;
  z-index: 1;
  top: 16%;
  left: 13%;
  width: 74%;
  height: 1px;
  pointer-events: none;
  opacity: 0;
  background: linear-gradient(
    90deg,
    transparent,
    color-mix(in srgb, var(--il-c1) 85%, white),
    transparent
  );
  box-shadow: 0 0 12px color-mix(in srgb, var(--il-c1) 55%, transparent);
  animation: ilScan 3.2s ease-in-out infinite;
}

.initial-loader-lottie {
  position: relative;
  z-index: 2;
  width: 68%;
  height: 68%;
}

.initial-loader-title {
  margin: 0;
  font-size: clamp(18px, 2.4vw, 25px);
  line-height: 1.4;
  font-weight: 850;
  letter-spacing: 0.02em;
  color: transparent;
  background: linear-gradient(
    100deg,
    var(--il-text) 0%,
    var(--il-c1) 30%,
    var(--il-c2) 55%,
    var(--il-c3) 75%,
    var(--il-text) 100%
  );
  background-size: 220% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  animation: ilTitleFlow 5s linear infinite;
}

.app.lang-fa .initial-loader-title {
  letter-spacing: 0;
  line-height: 1.6;
}

.initial-loader-progress-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  align-items: center;
  gap: 12px;
  width: min(440px, 100%);
  margin: 18px auto;
}

.initial-loader-progress {
  direction: ltr;
  position: relative;
  width: 100%;
  height: 8px;
  overflow: hidden;
  border-radius: 999px;
  background: var(--il-track);
  border: 1px solid color-mix(in srgb, var(--il-c1) 16%, transparent);
}

.initial-loader-progress span {
  position: relative;
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: inherit;
  background: linear-gradient(90deg, #176fce 0%, #258fe6 52%, #62c5f5 100%);
  background-size: 100% 100%;
  box-shadow: 0 0 12px rgba(37, 143, 230, 0.38);
  transform: scaleX(0);
  transform-origin: left center;
  will-change: transform;
}

.initial-loader-progress span::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(
    100deg,
    transparent 30%,
    rgba(255, 255, 255, 0.55) 50%,
    transparent 70%
  );
  transform: translateX(-100%);
  animation: ilShine 1.6s ease-in-out infinite;
}

.initial-loader-percent {
  min-width: 46px;
  color: var(--il-text);
  font-family: var(--font-mono);
  font-size: 13px;
  font-weight: 700;
  font-variant-numeric: tabular-nums;
  letter-spacing: 0.04em;
  text-align: end;
}

.initial-loader-chips {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
  max-width: 460px;
  margin: 0 auto;
}

.initial-loader-chips span {
  padding: 6px 12px;
  border: 1px solid var(--il-chip-border);
  border-radius: 999px;
  background: var(--il-chip-bg);
  color: var(--il-soft);
  font-size: 11px;
  font-weight: 700;
  opacity: 0;
  transform: translateY(8px);
  animation: ilChipIn 0.6s cubic-bezier(0.22, 1, 0.36, 1) forwards;
}

.initial-loader-chips span:nth-child(1) {
  animation-delay: 0.3s;
}
.initial-loader-chips span:nth-child(2) {
  animation-delay: 0.45s;
}
.initial-loader-chips span:nth-child(3) {
  animation-delay: 0.6s;
}
.initial-loader-chips span:nth-child(4) {
  animation-delay: 0.75s;
}
.initial-loader-chips span:nth-child(5) {
  animation-delay: 0.9s;
}
.initial-loader-chips span:nth-child(6) {
  animation-delay: 1.05s;
}

.initial-loader-leave-active {
  transition:
    opacity 0.55s ease,
    transform 0.55s cubic-bezier(0.22, 1, 0.36, 1),
    filter 0.55s ease;
}

.initial-loader-leave-to {
  opacity: 0;
  transform: scale(1.04);
  filter: blur(6px);
}

@keyframes ilBlob {
  to {
    transform: translate(6vmax, 5vmax) scale(1.12);
  }
}

@keyframes ilFrameFloat {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-3px);
  }
}

@keyframes ilCornerPulse {
  0%,
  100% {
    opacity: 0.62;
  }
  50% {
    opacity: 1;
  }
}

@keyframes ilScan {
  0% {
    opacity: 0;
    transform: translateY(0);
  }
  22% {
    opacity: 0.75;
  }
  78% {
    opacity: 0.42;
    transform: translateY(var(--il-scan-distance));
  }
  100% {
    opacity: 0;
    transform: translateY(var(--il-scan-distance));
  }
}

@keyframes ilTitleFlow {
  to {
    background-position: -220% 0;
  }
}

@keyframes ilShine {
  to {
    transform: translateX(100%);
  }
}

@keyframes ilChipIn {
  to {
    opacity: 1;
    transform: none;
  }
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
  .initial-loader-chips span {
    font-size: 10px;
    padding: 5px 9px;
  }
}

@media (max-height: 640px) {
  .initial-loader-animation {
    width: 100px;
    height: 100px;
    margin: 24px auto 34px;
  }

  .initial-loader-corner {
    width: 14px;
    height: 14px;
  }
  .initial-loader-chips {
    display: none;
  }
  .initial-loader-center {
    padding: 14px 22px;
  }
}

@media (max-width: 760px) {
  .il-blob-3 {
    display: none;
  }
}

@media (prefers-reduced-motion: reduce) {
  .initial-loader-chips span {
    opacity: 1;
    transform: none;
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
      60% 50% at 12% 0%,
      rgba(80, 210, 255, 0.1),
      transparent 70%
    ),
    radial-gradient(
      50% 45% at 92% 18%,
      rgba(132, 121, 255, 0.11),
      transparent 70%
    ),
    radial-gradient(
      55% 45% at 50% 100%,
      rgba(255, 138, 216, 0.06),
      transparent 70%
    ),
    linear-gradient(180deg, #07111f 0%, #081422 52%, #07101c 100%);
}

/* faint dotted grid that fades out away from the top */
.bg-base::after {
  content: "";
  position: absolute;
  inset: 0;
  opacity: 0.6;
  background-image: radial-gradient(
    rgba(255, 255, 255, 0.09) 1px,
    transparent 1.2px
  );
  background-size: 30px 30px;
  -webkit-mask-image: radial-gradient(
    ellipse 80% 60% at 50% 18%,
    #000 0%,
    transparent 75%
  );
  mask-image: radial-gradient(
    ellipse 80% 60% at 50% 18%,
    #000 0%,
    transparent 75%
  );
}

.app.light .bg-base {
  background:
    radial-gradient(
      60% 50% at 12% 0%,
      rgba(40, 160, 230, 0.1),
      transparent 70%
    ),
    radial-gradient(
      50% 45% at 92% 18%,
      rgba(110, 90, 230, 0.08),
      transparent 70%
    ),
    radial-gradient(
      55% 45% at 50% 100%,
      rgba(230, 90, 170, 0.05),
      transparent 70%
    ),
    linear-gradient(180deg, #fbfcfe 0%, #f7f9fc 52%, #f1f4f8 100%);
}

.app.light .bg-base::after {
  background-image: radial-gradient(
    rgba(15, 23, 42, 0.1) 1px,
    transparent 1.2px
  );
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
  gap: 9px;
  padding: 8px 16px;
  margin-bottom: 16px;
  border-radius: 999px;
  background:
    linear-gradient(
      135deg,
      rgba(var(--spot-rgb), 0.12),
      rgba(132, 121, 255, 0.07) 60%,
      rgba(255, 138, 216, 0.05)
    ),
    var(--surface);
  border: 1px solid var(--border-strong);
  color: var(--text);
  font-size: 13px;
  font-weight: 800;
  letter-spacing: 0.02em;
  backdrop-filter: blur(10px);
}

.section-badge::before {
  content: "";
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--grad);
  box-shadow: 0 0 0 3px rgba(var(--spot-rgb), 0.16);
}

.section-heading h2,
.about-main h2,
.showcase-content h2,
.contact-shell h2 {
  margin: 0 0 16px;
  color: var(--text);
  font-size: clamp(34px, 4vw, 48px);
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
    #07111f;
  box-shadow:
    0 18px 45px rgba(0, 0, 0, 0.13),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
}

.app.light .nav-shell {
  background: linear-gradient(135deg, #ffffff, #f5f8fc);
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
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2px;
  padding: 4px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.03);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.04);
}

.app.light .nav-links {
  background: rgba(15, 23, 42, 0.03);
}

.nav-links a {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 34px;
  padding: 0 14px;
  border-radius: 999px;
  color: var(--text-soft);
  font-size: 12.5px;
  font-weight: 700;
  white-space: nowrap;
  transition:
    color 0.25s ease,
    background 0.25s ease,
    box-shadow 0.25s ease;
}

.nav-links a:hover {
  color: var(--text);
  background: var(--surface-strong);
}

.nav-links a.active {
  color: var(--text);
  background: linear-gradient(
    135deg,
    rgba(var(--spot-rgb), 0.2),
    rgba(132, 121, 255, 0.17)
  );
  box-shadow: inset 0 0 0 1px rgba(var(--spot-rgb), 0.3);
}

.nav-links a.active::after {
  content: "";
  position: absolute;
  bottom: 3px;
  left: 50%;
  width: 14px;
  height: 2px;
  margin-left: -7px;
  border-radius: 2px;
  background: var(--grad);
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 8px;
  direction: ltr;
}

/* segmented EN | فا switch with a sliding thumb */
.lang-toggle {
  position: relative;
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 78px;
  height: 38px;
  padding: 3px;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.03);
  color: var(--muted);
  font-family: inherit;
  cursor: pointer;
  isolation: isolate;
  transition: border-color 0.25s ease;
}

.app.light .lang-toggle,
.app.light .theme-toggle {
  background: rgba(15, 23, 42, 0.03);
}

.lang-toggle:hover {
  border-color: var(--border-strong);
}

.lang-thumb {
  position: absolute;
  z-index: -1;
  top: 3px;
  bottom: 3px;
  left: 3px;
  width: calc((100% - 6px) / 2);
  border-radius: 999px;
  background: linear-gradient(135deg, var(--primary), var(--primary-2));
  box-shadow: 0 4px 14px rgba(var(--spot-rgb), 0.3);
  transition: transform 0.4s cubic-bezier(0.34, 1.3, 0.5, 1);
}

.lang-toggle.fa .lang-thumb {
  transform: translateX(100%);
}

.lang-opt {
  display: grid;
  place-items: center;
  font-size: 11.5px;
  font-weight: 800;
  letter-spacing: 0.04em;
  line-height: 1;
  transition: color 0.3s ease;
}

.lang-opt.on {
  color: var(--on-primary);
}

/* round button, sun and moon cross-fade with a small rotation */
.theme-toggle {
  position: relative;
  display: grid;
  place-items: center;
  width: 38px;
  height: 38px;
  padding: 0;
  overflow: hidden;
  border: 1px solid var(--border);
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.03);
  color: var(--text);
  cursor: pointer;
  transition:
    transform 0.25s ease,
    border-color 0.25s ease,
    background 0.25s ease;
}

.theme-toggle:hover {
  transform: translateY(-1px);
  border-color: var(--border-strong);
  background: var(--surface-strong);
}

.theme-toggle:active {
  transform: scale(0.94);
}

.theme-toggle svg {
  grid-area: 1 / 1;
  width: 18px;
  height: 18px;
  transition:
    transform 0.55s cubic-bezier(0.2, 0.8, 0.2, 1),
    opacity 0.35s ease;
}

.theme-sun {
  color: #f5b942;
}

.theme-moon {
  color: var(--primary-2);
  opacity: 0;
  transform: rotate(-80deg) scale(0.4);
}

.theme-toggle.light .theme-sun {
  opacity: 0;
  transform: rotate(80deg) scale(0.4);
}

.theme-toggle.light .theme-moon {
  opacity: 1;
  transform: none;
}

/* labelled buttons used inside the mobile menu */
.switch-btn {
  width: 100%;
  height: 40px;
  padding: 0 12px;
  border: 1px solid var(--border);
  border-radius: 12px;
  background: var(--surface);
  color: var(--text);
  font-family: inherit;
  cursor: pointer;
  transition:
    transform 0.2s ease,
    border-color 0.25s ease,
    background 0.25s ease;
}

.switch-btn:hover {
  border-color: var(--border-strong);
  background: var(--surface-strong);
}

.switch-btn:active {
  transform: scale(0.97);
}

.switch-inner {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.switch-icon-wrap {
  width: 18px;
  height: 18px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
  color: var(--primary);
}

.switch-icon-wrap svg {
  width: 18px;
  height: 18px;
}

.switch-text {
  font-size: 12.5px;
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
   MOBILE MENU (opens from the hamburger side: EN right, FA left)
========================================================= */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  z-index: 70;
  display: flex;
  align-items: flex-start;
  /* physical sides: the overlay is always LTR so flex-end = right edge */
  justify-content: flex-end;
  direction: ltr;
  padding: 72px 12px 12px;
  background: rgba(4, 10, 18, 0.5);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}

/* FA (RTL): the hamburger sits on the left, so the menu opens from the left */
.app.lang-fa .mobile-menu-overlay {
  justify-content: flex-start;
}

.app.light .mobile-menu-overlay {
  background: rgba(226, 234, 243, 0.5);
}

.mobile-menu-panel {
  width: min(280px, 100%);
  height: auto;
  max-height: calc(100dvh - 84px);
  padding: 14px;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  direction: ltr;
  border: 1px solid var(--border);
  border-radius: 22px;
  background: rgba(12, 22, 37, 0.97);
  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.38);
  transform-origin: top right;
}

.app.lang-fa .mobile-menu-panel {
  direction: rtl;
  transform-origin: top left;
}

.app.light .mobile-menu-panel {
  background: rgba(255, 255, 255, 0.97);
  box-shadow: 0 24px 60px rgba(55, 75, 105, 0.18);
}

.mobile-menu-header {
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
}

.mobile-menu-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.mobile-menu-mark {
  width: 36px;
  height: 36px;
  display: grid;
  place-items: center;
  flex-shrink: 0;
}

.mobile-lottie {
  width: 36px;
  height: 36px;
}

.mobile-menu-brand strong {
  display: block;
  color: var(--text);
  font-size: 13px;
}

.mobile-menu-brand span {
  display: block;
  margin-top: 2px;
  color: var(--muted);
  font-size: 9px;
}

.mobile-close {
  width: 36px;
  height: 36px;
  flex-shrink: 0;
  display: grid;
  place-items: center;
  border: 1px solid var(--border);
  border-radius: 11px;
  background: var(--surface);
  color: var(--text);
  font-size: 20px;
  cursor: pointer;
}

.mobile-nav-links {
  flex: 0 1 auto;
  min-height: 0;
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 10px 0;
  overflow-y: auto;
  overscroll-behavior: contain;
}

.mobile-nav-links a {
  position: relative;
  min-height: 42px;
  display: flex;
  align-items: center;
  padding: 9px 12px;
  border: 1px solid transparent;
  border-radius: 12px;
  color: var(--text);
  font-size: 14px;
  font-weight: 700;
  transition:
    background 0.22s ease,
    border-color 0.22s ease,
    color 0.22s ease;
}

.mobile-nav-links a::after {
  content: "";
  position: absolute;
  inset-inline-end: 12px;
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
  gap: 6px;
  padding-top: 12px;
  border-top: 1px solid var(--border);
}

.mobile-actions .switch-btn {
  width: 100%;
  height: 38px;
}

.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: opacity 0.25s ease;
}

.mobile-menu-enter-active .mobile-menu-panel,
.mobile-menu-leave-active .mobile-menu-panel {
  transition:
    transform 0.32s cubic-bezier(0.22, 1, 0.36, 1),
    opacity 0.25s ease;
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
}

/* EN: slides in from the right (same side as the hamburger) */
.mobile-menu-enter-from .mobile-menu-panel,
.mobile-menu-leave-to .mobile-menu-panel {
  opacity: 0;
  transform: translateX(28px) scale(0.96);
}

/* FA: slides in from the left (same side as the hamburger) */
.app.lang-fa .mobile-menu-enter-from .mobile-menu-panel,
.app.lang-fa .mobile-menu-leave-to .mobile-menu-panel {
  transform: translateX(-28px) scale(0.96);
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
  margin: 0 0 18px;
  color: var(--text);
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
  background-image: linear-gradient(
    100deg,
    #eaf6ff 0%,
    var(--primary) 28%,
    var(--primary-2) 56%,
    var(--primary-3) 80%,
    #eaf6ff 100%
  );
  background-size: 220% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  animation: nameFlow 9s linear infinite;
}

.app.light .hero-name {
  background-image: linear-gradient(
    100deg,
    #0e1626 0%,
    var(--primary) 30%,
    var(--primary-2) 58%,
    var(--primary-3) 82%,
    #0e1626 100%
  );
}

.app.lang-fa .hero-title {
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
  min-width: 176px;
  min-height: 52px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 0 22px;
  overflow: hidden;
  isolation: isolate;
  border: 1px solid transparent;
  border-radius: 16px;
  transition:
    transform 0.3s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.3s ease,
    border-color 0.3s ease,
    background-color 0.3s ease,
    background-position 0.6s ease;
}

.premium-btn:hover {
  transform: translateY(-3px);
}

.premium-btn:active {
  transform: translateY(-1px) scale(0.97);
}

.premium-btn.primary {
  color: var(--on-primary);
  background: var(--grad);
  background-size: 180% 100%;
  background-position: 0 0;
  box-shadow:
    0 14px 34px rgba(var(--spot-rgb), 0.22),
    inset 0 1px 0 rgba(255, 255, 255, 0.32);
}

.premium-btn.primary:hover {
  background-position: 100% 0;
  box-shadow:
    0 18px 42px rgba(var(--spot-rgb), 0.34),
    inset 0 1px 0 rgba(255, 255, 255, 0.4);
}

.premium-btn.ghost {
  color: var(--text);
  border-color: var(--border);
  background: var(--surface);
  backdrop-filter: blur(12px);
}

.premium-btn.ghost:hover {
  border-color: var(--border-strong);
  background: var(--surface-strong);
}

/* diagonal sheen that sweeps across on hover */
.btn-liquid {
  position: absolute;
  top: -20%;
  bottom: -20%;
  left: 0;
  z-index: 1;
  width: 38%;
  background: linear-gradient(
    100deg,
    transparent,
    rgba(255, 255, 255, 0.45),
    transparent
  );
  transform: translateX(-160%) skewX(-18deg);
  transition: transform 0.75s cubic-bezier(0.2, 0.8, 0.2, 1);
  pointer-events: none;
}

.premium-btn:hover .btn-liquid {
  transform: translateX(380%) skewX(-18deg);
}

.btn-content,
.btn-arrow {
  position: relative;
  z-index: 2;
  font-size: 14px;
}

.btn-content {
  font-weight: 800;
}

.btn-arrow {
  width: 24px;
  height: 24px;
  display: grid;
  place-items: center;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.22);
  font-size: 13px;
  transition:
    transform 0.3s ease,
    background 0.3s ease;
}

.premium-btn.ghost .btn-arrow {
  background: var(--surface-strong);
}

.premium-btn:hover .btn-arrow {
  transform: translate(2px, -2px);
}

/* hero visual */
.glass-stage {
  position: relative;
  min-height: 620px;
  display: grid;
  place-items: center;
}

.stage-glow {
  position: absolute;
  width: 500px;
  height: 500px;
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
  width: min(100%, 530px);
  min-height: 560px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.hero-lottie {
  position: relative;
  z-index: 2;
  width: min(100%, 470px);
  height: 470px;
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
  font-family: var(--font-display);
  font-size: 17px;
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
  background: var(--grad);
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
  background: var(--grad);
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
  background: var(--grad);
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
  display: grid;
  grid-template-columns: minmax(0, 1fr);
  gap: 14px;
  max-width: 400px;
  margin-top: 30px;
}

.contact-link {
  --lk-rgb: 88, 216, 255;

  position: relative;
  min-width: 0;
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 13px 14px;
  overflow: hidden;
  isolation: isolate;
  text-align: start;
  color: var(--text);
  border: 1px solid var(--border);
  border-radius: 22px;
  background:
    linear-gradient(
      135deg,
      rgba(var(--lk-rgb), 0.1),
      rgba(var(--lk-rgb), 0.015) 62%
    ),
    var(--surface);
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    border-color 0.3s ease,
    box-shadow 0.35s ease;
}

.contact-link-telegram {
  --lk-rgb: 42, 171, 238;
}

.contact-link::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  background: linear-gradient(
    105deg,
    transparent 30%,
    rgba(var(--lk-rgb), 0.2) 50%,
    transparent 70%
  );
  transform: translateX(-110%);
  transition: transform 0.75s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.contact-link:hover,
.contact-link:focus-visible {
  transform: translateY(-4px);
  border-color: rgba(var(--lk-rgb), 0.55);
  box-shadow:
    0 18px 38px rgba(var(--lk-rgb), 0.18),
    0 0 0 1px rgba(var(--lk-rgb), 0.12);
}

.contact-link:hover::before,
.contact-link:focus-visible::before {
  transform: translateX(110%);
}

.contact-link-icon {
  width: 50px;
  height: 50px;
  flex: 0 0 50px;
  display: grid;
  place-items: center;
  color: #fff;
  border-radius: 16px;
  background: linear-gradient(
    145deg,
    rgb(var(--lk-rgb)),
    color-mix(in srgb, rgb(var(--lk-rgb)) 55%, #4f46e5)
  );
  box-shadow:
    0 10px 22px rgba(var(--lk-rgb), 0.32),
    inset 0 1px 0 rgba(255, 255, 255, 0.35);
  transition: transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.contact-link-icon svg {
  width: 23px;
  height: 23px;
}

.contact-link:hover .contact-link-icon,
.contact-link:focus-visible .contact-link-icon {
  transform: scale(1.06) rotate(-6deg);
}

.contact-link-text {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.contact-link-text small {
  color: var(--muted);
  font-size: 11px;
  font-weight: 700;
  line-height: 1.4;
}

.contact-link-text strong {
  overflow: hidden;
  color: var(--text);
  font-family: var(--font-display);
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.01em;
  line-height: 1.3;
  text-align: start;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.contact-link-arrow {
  width: 36px;
  height: 36px;
  flex: 0 0 36px;
  display: grid;
  place-items: center;
  color: var(--text-soft);
  border: 1px solid var(--border);
  border-radius: 50%;
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    background 0.3s ease,
    color 0.3s ease,
    border-color 0.3s ease;
}

.contact-link-arrow svg {
  width: 16px;
  height: 16px;
}

.contact-link:hover .contact-link-arrow,
.contact-link:focus-visible .contact-link-arrow {
  color: #fff;
  border-color: transparent;
  background: rgb(var(--lk-rgb));
  transform: rotate(45deg);
}

.app.light .contact-link:hover,
.app.light .contact-link:focus-visible {
  box-shadow:
    0 18px 38px rgba(var(--lk-rgb), 0.22),
    0 0 0 1px rgba(var(--lk-rgb), 0.14);
}

@media (max-width: 760px) {
  .contact-link:hover,
  .contact-link:focus-visible {
    transform: none;
  }
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
  transform: translateY(28px) scale(0.985);
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
   OPENING REVEAL — "light split"
   After the loader dissolves, a thin line of light draws across
   the centre, flares, and the screen opens along it like a pair
   of doors while the hero settles into place.
   Only transform / opacity are animated (GPU friendly).
========================================================= */
.portal {
  position: fixed;
  inset: 0;
  z-index: 9999;
  overflow: hidden;
  pointer-events: none;
  isolation: isolate;
}

.portal-door {
  position: absolute;
  left: 0;
  right: 0;
  height: calc(50% + 1px);
  will-change: transform;
  backface-visibility: hidden;
}

.portal-door-top {
  top: 0;
  background:
    radial-gradient(
      ellipse at 50% 100%,
      rgba(40, 160, 235, 0.14),
      transparent 62%
    ),
    linear-gradient(180deg, #050b14 0%, #07162a 100%);
  animation: portalDoorTop 1s cubic-bezier(0.76, 0, 0.18, 1) 0.62s forwards;
}

.portal-door-bottom {
  bottom: 0;
  background:
    radial-gradient(
      ellipse at 50% 0%,
      rgba(40, 160, 235, 0.14),
      transparent 62%
    ),
    linear-gradient(0deg, #050b14 0%, #07162a 100%);
  animation: portalDoorBottom 1s cubic-bezier(0.76, 0, 0.18, 1) 0.62s forwards;
}

/* soft glow hugging the inner edge of each door */
.portal-door::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  height: clamp(48px, 11vh, 110px);
  opacity: 0;
  animation: portalEdge 0.5s ease 0.3s forwards;
}

.portal-door-top::after {
  bottom: 0;
  background: linear-gradient(0deg, rgba(88, 208, 255, 0.2), transparent);
}

.portal-door-bottom::after {
  top: 0;
  background: linear-gradient(180deg, rgba(88, 208, 255, 0.2), transparent);
}

.portal-flare {
  position: absolute;
  left: 50%;
  top: 50%;
  width: min(96vmin, 760px);
  height: min(96vmin, 760px);
  translate: -50% -50%;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(92, 212, 255, 0.34),
    rgba(120, 104, 255, 0.12) 46%,
    transparent 70%
  );
  opacity: 0;
  transform: scale(0.2);
  will-change: transform, opacity;
  animation: portalFlare 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.52s forwards;
}

.portal-line {
  position: absolute;
  left: 6%;
  right: 6%;
  top: 50%;
  height: 2px;
  margin-top: -1px;
  border-radius: 2px;
  background: linear-gradient(
    90deg,
    transparent,
    #7fe3ff 18%,
    #ffffff 50%,
    #7fe3ff 82%,
    transparent
  );
  box-shadow:
    0 0 16px rgba(65, 190, 255, 0.85),
    0 0 52px rgba(34, 141, 244, 0.5);
  opacity: 0;
  transform: scaleX(0);
  will-change: transform, opacity;
  animation: portalLine 1.3s cubic-bezier(0.22, 1, 0.36, 1) 0.3s forwards;
}

.app.light .portal-door-top {
  background:
    radial-gradient(
      ellipse at 50% 100%,
      rgba(49, 166, 223, 0.14),
      transparent 62%
    ),
    linear-gradient(180deg, #f4f7fc 0%, #e6f2fc 100%);
}

.app.light .portal-door-bottom {
  background:
    radial-gradient(
      ellipse at 50% 0%,
      rgba(49, 166, 223, 0.14),
      transparent 62%
    ),
    linear-gradient(0deg, #f4f7fc 0%, #e6f2fc 100%);
}

.app.light .portal-door-top::after {
  background: linear-gradient(0deg, rgba(18, 131, 204, 0.16), transparent);
}

.app.light .portal-door-bottom::after {
  background: linear-gradient(180deg, rgba(18, 131, 204, 0.16), transparent);
}

.app.light .portal-flare {
  background: radial-gradient(
    circle,
    rgba(18, 131, 204, 0.26),
    rgba(90, 73, 214, 0.1) 46%,
    transparent 70%
  );
}

.app.light .portal-line {
  background: linear-gradient(
    90deg,
    transparent,
    #3aa6e0 18%,
    #ffffff 50%,
    #3aa6e0 82%,
    transparent
  );
  box-shadow:
    0 0 16px rgba(18, 131, 204, 0.7),
    0 0 52px rgba(18, 131, 204, 0.38);
}

/* the hero eases in from slightly zoomed while the doors open */
.app.is-revealing .hero {
  transform-origin: 50% 42%;
  animation: heroSettle 1.3s cubic-bezier(0.22, 1, 0.36, 1) 0.6s backwards;
}

.app.is-revealing .navbar {
  animation: navDrop 0.95s cubic-bezier(0.22, 1, 0.36, 1) 1.05s backwards;
}

/* Hero content rises in as the light sweep reveals the page. */
.hero-left.in-view > * {
  animation: heroRise 0.95s cubic-bezier(0.22, 1, 0.36, 1) backwards;
}

.hero-left.in-view > :nth-child(1) {
  animation-delay: 0.05s;
}
.hero-left.in-view > :nth-child(2) {
  animation-delay: 0.16s;
}
.hero-left.in-view > :nth-child(3) {
  animation-delay: 0.27s;
}
.hero-left.in-view > :nth-child(4) {
  animation-delay: 0.38s;
}
.hero-left.in-view > :nth-child(5) {
  animation-delay: 0.49s;
}
.hero-left.in-view > :nth-child(6) {
  animation-delay: 0.6s;
}

.hero-right.in-view .lottie-card {
  animation:
    heroPop 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.2s backwards,
    floatY 7s ease-in-out 1.6s infinite;
}

/* =========================================================
   ENHANCEMENTS: skills badge, spotlight cards, nav extras, back-to-top
========================================================= */
.skill-icon {
  color: var(--primary);
  font: 800 15px / 1 var(--font-mono);
  letter-spacing: 0.02em;
  background: linear-gradient(
    135deg,
    rgba(var(--spot-rgb), 0.15),
    rgba(132, 121, 255, 0.12)
  );
}

.spot {
  --mx: 50%;
  --my: 50%;

  position: relative;
  isolation: isolate;
}

.pc.spot {
  --spot-rgb: var(--pc-rgb);
}

.spot::before,
.spot::after {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: inherit;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.35s ease;
}

/* soft glow under the content */
.spot::before {
  z-index: -1;
  background: radial-gradient(
    380px circle at var(--mx) var(--my),
    rgba(var(--spot-rgb), 0.14),
    transparent 62%
  );
}

/* glowing border that follows the cursor */
.spot::after {
  z-index: 1;
  padding: 1px;
  background: radial-gradient(
    240px circle at var(--mx) var(--my),
    rgba(var(--spot-rgb), 0.8),
    rgba(132, 121, 255, 0.35) 45%,
    transparent 70%
  );
  -webkit-mask:
    linear-gradient(#000 0 0) content-box,
    linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  mask:
    linear-gradient(#000 0 0) content-box,
    linear-gradient(#000 0 0);
  mask-composite: exclude;
}

.spot:hover::before,
.spot:hover::after {
  opacity: 1;
}

@media (hover: none) {
  .spot::before,
  .spot::after {
    display: none;
  }
}

.skip-link {
  position: fixed;
  top: 10px;
  inset-inline-start: 12px;
  z-index: 11000;
  padding: 10px 16px;
  border-radius: 12px;
  background: var(--grad);
  color: var(--on-primary);
  font-size: 13px;
  font-weight: 800;
  transform: translateY(-160%);
  transition: transform 0.25s ease;
}

.skip-link:focus {
  transform: none;
}

.nav-shell {
  transition:
    border-color 0.3s ease,
    box-shadow 0.3s ease;
}

.navbar.scrolled .nav-shell {
  border-color: var(--border-strong);
  box-shadow:
    0 18px 50px rgba(0, 0, 0, 0.26),
    inset 0 1px 0 rgba(255, 255, 255, 0.06);
}

.app.light .navbar.scrolled .nav-shell {
  box-shadow:
    0 18px 50px rgba(56, 77, 105, 0.16),
    inset 0 1px 0 rgba(255, 255, 255, 0.92);
}

.nav-progress {
  position: absolute;
  inset-inline: 22px;
  bottom: -1px;
  height: 2px;
  overflow: hidden;
  border-radius: 2px;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
}

.navbar.scrolled .nav-progress {
  opacity: 1;
}

.nav-progress i {
  display: block;
  width: 100%;
  height: 100%;
  background: var(--progress);
  transform: scaleX(0);
  transform-origin: left center;
  will-change: transform;
}

.app.lang-fa .nav-progress i {
  transform-origin: right center;
}

.mobile-nav-links a.active {
  color: var(--primary);
  border-color: var(--border);
  background: linear-gradient(
    90deg,
    rgba(var(--spot-rgb), 0.1),
    rgba(132, 121, 255, 0.06)
  );
}

.mobile-nav-links a.active::after {
  opacity: 1;
  transform: scale(1);
}

.to-top {
  position: fixed;
  inset-inline-end: 18px;
  bottom: 18px;
  z-index: 55;
  width: 46px;
  height: 46px;
  display: grid;
  place-items: center;
  padding: 0;
  border: 0;
  border-radius: 50%;
  background: var(--nav-bg);
  color: var(--text);
  box-shadow: var(--shadow);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  cursor: pointer;
  opacity: 0;
  visibility: hidden;
  transform: translateY(14px) scale(0.9);
  transition:
    opacity 0.3s ease,
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    color 0.25s ease,
    visibility 0s linear 0.35s;
}

.to-top.show {
  opacity: 1;
  visibility: visible;
  transform: none;
  transition-delay: 0s;
}

.to-top.show:hover {
  color: var(--primary);
  transform: translateY(-3px);
}

.to-top-ring {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}

.to-top-ring circle {
  fill: none;
  stroke-width: 2.2;
}

.ring-track {
  stroke: var(--border);
}

.ring-bar {
  stroke: var(--progress-solid);
  stroke-linecap: round;
  stroke-dasharray: 125.66;
  stroke-dashoffset: 125.66;
}

.to-top-arrow {
  position: relative;
  width: 18px;
  height: 18px;
}

/* animated name and sheen on the hero caption card */
.lottie-caption {
  overflow: hidden;
}

.lottie-caption::after {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  width: 40%;
  background: linear-gradient(
    100deg,
    transparent,
    rgba(255, 255, 255, 0.14),
    transparent
  );
  transform: translateX(-160%) skewX(-18deg);
  animation: captionSheen 6s ease-in-out 2s infinite;
  pointer-events: none;
}

.lottie-caption strong,
.app.light .lottie-caption strong {
  position: relative;
  width: fit-content;
  color: transparent;
  font-size: 16px;
  letter-spacing: 0.01em;
  background-repeat: no-repeat;
  background-size:
    260% 100%,
    100% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  animation: nameShine 5s ease-in-out infinite;
}

/* sober steel-blue palette + a faint band that sweeps across the name */
.lottie-caption strong {
  background-image:
    linear-gradient(
      105deg,
      transparent 42%,
      rgba(255, 255, 255, 0.8) 50%,
      transparent 58%
    ),
    linear-gradient(90deg, #e8eef7 0%, #b9c9de 40%, #8fa9cc 75%, #c3d1e6 100%);
}

.app.light .lottie-caption strong {
  background-image:
    linear-gradient(
      105deg,
      transparent 42%,
      rgba(255, 255, 255, 0.75) 50%,
      transparent 58%
    ),
    linear-gradient(90deg, #0f1f35 0%, #1f4f86 45%, #2f5f9c 75%, #16335a 100%);
}

/* thin accent line under the name */
.lottie-caption strong::after {
  content: "";
  display: block;
  width: 30px;
  height: 2px;
  margin-top: 4px;
  border-radius: 2px;
  background: var(--progress);
  transform-origin: left center;
  animation: nameLine 3.6s ease-in-out infinite;
}

.app.lang-fa .lottie-caption strong::after {
  transform-origin: right center;
}

.app.light .lottie-caption::after {
  background: linear-gradient(
    100deg,
    transparent,
    rgba(255, 255, 255, 0.7),
    transparent
  );
}

.hamburger-btn {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  transition: border-color 0.25s ease;
}

.hamburger-btn:hover {
  border-color: var(--border-strong);
}

/* =========================================================
   KEYFRAMES
========================================================= */
@keyframes portalDoorTop {
  to {
    transform: translate3d(0, -101%, 0);
  }
}

@keyframes portalDoorBottom {
  to {
    transform: translate3d(0, 101%, 0);
  }
}

@keyframes portalEdge {
  to {
    opacity: 1;
  }
}

@keyframes portalLine {
  0% {
    opacity: 0;
    transform: scaleX(0) scaleY(1);
  }
  10% {
    opacity: 1;
  }
  32% {
    opacity: 1;
    transform: scaleX(1) scaleY(1);
  }
  48% {
    opacity: 1;
    transform: scaleX(1) scaleY(2.6);
  }
  100% {
    opacity: 0;
    transform: scaleX(1.15) scaleY(0.5);
  }
}

@keyframes portalFlare {
  0% {
    opacity: 0;
    transform: scale(0.2);
  }
  35% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: scale(1.9);
  }
}

@keyframes heroSettle {
  from {
    opacity: 0.15;
    transform: scale(1.07);
  }
}

@keyframes nameShine {
  0%,
  30% {
    background-position:
      160% 0,
      0 0;
  }
  70%,
  100% {
    background-position:
      -60% 0,
      0 0;
  }
}

@keyframes nameLine {
  0%,
  100% {
    opacity: 0.6;
    transform: scaleX(0.35);
  }
  50% {
    opacity: 1;
    transform: scaleX(1);
  }
}

@keyframes captionSheen {
  0%,
  55% {
    transform: translateX(-160%) skewX(-18deg);
  }
  85%,
  100% {
    transform: translateX(380%) skewX(-18deg);
  }
}

@keyframes nameFlow {
  to {
    background-position: 183.333% 0;
  }
}

@keyframes floatY {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-9px);
  }
}

@keyframes navDrop {
  from {
    opacity: 0;
    transform: translateY(-26px);
  }
}

@keyframes heroRise {
  from {
    opacity: 0;
    transform: translateY(26px);
  }
}

@keyframes heroPop {
  from {
    opacity: 0;
    transform: translateY(22px) scale(0.95);
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
    min-height: 560px;
  }

  .lottie-card {
    min-height: 510px;
  }

  .hero-lottie {
    width: min(100%, 425px);
    height: 425px;
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
    min-height: 540px;
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
    margin-inline: auto;
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
    width: min(100%, 420px);
    height: min(88vw, 400px);
    min-height: 290px;
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
    background: #07111f;
  }

  .app.light .nav-shell {
    background: #ffffff;
  }

  .nav-shell,
  .mobile-menu-overlay,
  .section-badge,
  .hero-chip,
  .floating-techs span,
  .premium-btn.ghost,
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
    width: min(100%, 360px);
    height: min(86vw, 340px);
    min-height: 260px;
  }

  .lottie-caption {
    width: calc(100% - 14px);
    margin-top: 7px;
    padding: 9px 10px;
    border-radius: 13px;
  }

  .lottie-caption strong {
    font-size: 13.5px;
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
    min-height: 240px;
    height: 78vw;
  }

  .lottie-caption {
    max-width: 300px;
  }
}

@media (max-width: 360px) {
  .brand-role {
    display: none;
  }
}

@media (max-height: 650px) and (max-width: 760px) {
  .mobile-nav-links a {
    min-height: 38px;
    padding-block: 6px;
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
