<template>
  <div>
    <header>
      <div class="header-container">
        <div class="logo">
          <img :src="logo" alt="СофтАрт" />
        </div>
        <nav>
          <a href="#">Услуги</a>
          <a href="#">Проекты</a>
          <a href="#">Продукты</a>
          <a href="#">Технологии</a>
          <a href="#">Контакты</a>
        </nav>
      </div>
    </header>

    <section class="hero">
      <h1>
        Разработка, размещение, поддержка и<br />
        продвижение интернет-проектов
      </h1>
      <p>
        SoftArt - полный цикл: лендинги, интернет-магазины,
        корпоративные порталы на TALL stack.
        Каждая заявка сразу в вашу CRM.
      </p>
      <div class="hero-buttons">
        <button class="btn-primary" @click="openModal">
          Подать заявку
        </button>
        <button class="btn-secondary">
          Наши сервисы
        </button>
      </div>
    </section>

    <section class="services">
      <h2>Что мы делаем</h2>
      <div class="cards">
        <div class="card">
          <div class="card-icon">
            <img :src="landingIcon" alt="" />
          </div>
          <h3>Лендинги</h3>
          <p>
            Высокая конверсия, скорость
            загрузки, анимация.
            Идеальны для рекламы.
          </p>
        </div>

        <div class="card">
          <div class="card-icon">
            <img :src="corporateIcon" alt="" />
          </div>
          <h3>
            Корпоративные<br />
            сайты
          </h3>
          <p>
            CRM, SiteSet CMS, 1С
            интеграция, личные кабинеты.
          </p>
        </div>

        <div class="card">
          <div class="card-icon">
            <img :src="shopIcon" alt="" />
          </div>
          <h3>
            Интернет-<br />
            магазины
          </h3>
          <p>
            Каталоги, фильтры, оплата,
            доставка и API.
          </p>
        </div>

        <div class="card">
          <div class="card-icon">
            <img :src="seoIcon" alt="" />
          </div>
          <h3>Поддержка & SEO</h3>
          <p>
            Продвижение в ТОП,
            хостинг из реестра РФ,
            SLA.
          </p>
        </div>
      </div>
    </section>

    <section class="projects">
      <h2>Реализованные проекты</h2>
      <div class="projects-grid">
        <a href="#" class="project-tag">magicfire.ru</a>
        <a href="#" class="project-tag">япошик.рф</a>
        <a href="#" class="project-tag">rostplast-nn.ru</a>
        <a href="#" class="project-tag">rskit.ru</a>
        <a href="#" class="project-tag">onlinepizza.ru</a>
        <a href="#" class="project-tag">jadever.ru</a>

        <a href="#" class="project-tag">protex-carter.ru</a>
        <a href="#" class="project-tag">termal.biz</a>
        <a href="#" class="project-tag">avtocenter-nn.ru</a>
        <a href="#" class="project-tag">spa-loren.ru</a>
        <a href="#" class="project-tag">pdc.nnov.ru</a>
        <a href="#" class="project-tag">двери-дома.рф</a>
      </div>
    </section>

    <!-- Модальное окно -->
    <RequestModal :isOpen="isModalOpen" @close="closeModal" />

    <footer>
      <div class="footer-container">
        <p>© 2025 Интернет-компания «СофтАрт»</p>
        <p>Разработка сайтов, CRM и интернет-проектов</p>
      </div>
    </footer>
  </div>
</template>

<script>
import RequestModal from './components/RequestModal.vue'
import logo from './components/assets/logo.svg'
import landingIcon from './components/assets/icons/landing.svg'
import corporateIcon from './components/assets/icons/corporate.svg'
import shopIcon from './components/assets/icons/shop.svg'
import seoIcon from './components/assets/icons/seo.svg'

export default {
  name: 'App',
  components: {
    RequestModal
  },
  data() {
    return {
      isModalOpen: false,
      logo,
      landingIcon,
      corporateIcon,
      shopIcon,
      seoIcon
    }
  },
  methods: {
    openModal() {
      this.isModalOpen = true
    },
    closeModal() {
      this.isModalOpen = false
    }
  }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: #f8f8f8;
    color: #1d2338;
    min-width: 320px;
}

/* ===== АДАПТИВНАЯ ШАПКА ===== */
header {
    height: 90px;
    background: #ffffff;
    padding: 0 20px;
}

.header-container {
    max-width: 1420px;
    height: 100%;
    margin: 0 auto;
    padding: 0 20px;

    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
}

.logo img {
    height: 42px;
    width: auto;
    display: block;
}

nav {
    display: flex;
    gap: 32px;
    flex-wrap: wrap;
}

nav a {
    text-decoration: none;
    color: #163f65;
    font-size: 16px;
    font-weight: 500;
    transition: .2s;
    white-space: nowrap;
}

nav a:hover {
    opacity: .7;
}

/* ===== ГЕРОЙ ===== */
.hero {
    background: #dcecf8;
    padding: 80px 20px;
    text-align: center;
}

.hero h1 {
    font-size: clamp(32px, 5vw, 62px);
    font-weight: 700;
    line-height: 1.3;
    color: #1e4f78;
}

.hero p {
    max-width: 800px;
    margin: 30px auto 0;
    font-size: clamp(16px, 1.5vw, 22px);
    line-height: 1.6;
    color: #606060;
}

.hero-buttons {
    margin-top: 45px;
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
}

.btn-primary,
.btn-secondary {
    min-width: 200px;
    padding: 0 30px;
    height: 54px;
    border-radius: 50px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: .2s;
    border: none;
}

.btn-primary {
    background: #0b87cf;
    color: #ffffff;
    box-shadow: 0 8px 20px rgba(11, 135, 207, .25);
}

.btn-primary:hover {
    transform: translateY(-2px);
}

.btn-secondary {
    background: transparent;
    border: 1px solid #8f8f8f;
    color: #4a4a4a;
}

.btn-secondary:hover {
    background: rgba(255, 255, 255, .5);
}

/* ===== УСЛУГИ ===== */
.services {
    padding: 60px 20px 80px;
    max-width: 1400px;
    margin: 0 auto;
}

.services h2 {
    text-align: center;
    font-size: clamp(28px, 3vw, 36px);
    font-weight: 700;
    color: #1e4f78;
    margin-bottom: 60px;
}

.cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 30px;
}

.card {
    background: #ffffff;
    border-radius: 18px;
    padding: 28px 24px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, .04), 0 12px 30px rgba(0, 0, 0, .08);
}

.card-icon {
    width: 42px;
    height: 42px;
    margin-bottom: 18px;
}

.card-icon img {
    width: 100%;
    height: 100%;
    object-fit: contain;
}

.card h3 {
    font-size: clamp(22px, 2vw, 28px);
    font-weight: 700;
    line-height: 1.2;
    color: #1d2338;
    margin-bottom: 10px;
}

.card p {
    font-size: 14px;
    line-height: 1.5;
    color: #777777;
}

/* ===== ПРОЕКТЫ ===== */
.projects {
    padding: 0 20px 90px;
    max-width: 1400px;
    margin: 0 auto;
}

.projects h2 {
    text-align: center;
    font-size: clamp(28px, 3vw, 36px);
    font-weight: 700;
    color: #1e4f78;
    margin-bottom: 55px;
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 20px 30px;
}

.project-tag {
    height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #9bb3c9;
    border-radius: 50px;
    text-decoration: none;
    color: #1e4f78;
    font-size: 14px;
    font-weight: 500;
    transition: .2s;
    padding: 0 16px;
    text-align: center;
}

.project-tag:hover {
    background: #f3f8fc;
}

/* ===== ФУТЕР ===== */
footer {
    background: #051c44;
    padding: 45px 20px;
    color: #ffffff;
}

.footer-container {
    max-width: 1400px;
    margin: 0 auto;
    text-align: center;
}

.footer-container p:first-child {
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: 500;
}

.footer-container p:last-child {
    font-size: 14px;
    opacity: .8;
}

/* ===== АДАПТИВ ===== */
@media (max-width: 1024px) {
    .header-container {
        padding: 0 15px;
    }
    
    nav {
        gap: 20px;
    }
    
    .cards {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    .header-container {
        flex-wrap: wrap;
        justify-content: center;
        padding: 10px 0;
        height: auto;
    }

    header {
        height: auto;
        padding: 15px 0;
    }

    nav {
        gap: 20px;
        justify-content: center;
        flex-wrap: wrap;
        width: 100%;
    }

    nav a {
        font-size: 14px;
        white-space: normal;
        padding: 4px 8px;
    }

    .hero {
        padding: 50px 16px;
    }

    .hero h1 {
        font-size: 28px;
    }
    
    .hero p {
        font-size: 16px;
        margin-top: 20px;
    }

    .btn-primary,
    .btn-secondary {
        min-width: 160px;
        height: 48px;
        font-size: 14px;
    }

    .cards {
        grid-template-columns: 1fr 1fr;
        gap: 20px;
    }
    
    .card {
        padding: 20px 18px;
    }
    
    .card-icon {
        width: 36px;
        height: 36px;
    }

    .projects-grid {
        grid-template-columns: repeat(3, 1fr);
        gap: 14px 20px;
    }
    
    .project-tag {
        font-size: 13px;
        height: 44px;
        padding: 0 12px;
    }
}

@media (max-width: 480px) {
    header {
        padding: 10px 0;
    }
    
    .header-container {
        gap: 10px;
    }
    
    .logo img {
        height: 32px;
    }
    
    nav {
        gap: 8px;
    }
    
    nav a {
        font-size: 12px;
        padding: 3px 6px;
    }
    
    .hero {
        padding: 40px 12px;
    }
    
    .hero h1 {
        font-size: 24px;
    }
    
    .hero p {
        font-size: 14px;
        margin-top: 16px;
    }
    
    .hero-buttons {
        gap: 12px;
        margin-top: 30px;
        flex-direction: column;
        align-items: center;
    }

    .btn-primary,
    .btn-secondary {
        min-width: 200px;
        width: 100%;
        max-width: 280px;
        height: 44px;
        font-size: 13px;
    }

    .services {
        padding: 40px 12px 60px;
    }
    
    .services h2 {
        font-size: 24px;
        margin-bottom: 40px;
    }
    
    .cards {
        grid-template-columns: 1fr;
        gap: 16px;
    }

    .card {
        padding: 16px 16px;
    }
    
    .card h3 {
        font-size: 20px;
    }
    
    .card p {
        font-size: 13px;
    }
    
    .card-icon {
        width: 32px;
        height: 32px;
        margin-bottom: 12px;
    }

    .projects {
        padding: 0 12px 60px;
    }
    
    .projects h2 {
        font-size: 24px;
        margin-bottom: 35px;
    }

    .projects-grid {
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
    }

    .project-tag {
        font-size: 12px;
        height: 38px;
        padding: 0 8px;
        border-radius: 50px;
    }

    footer {
        padding: 30px 16px;
    }
    
    .footer-container p:first-child {
        font-size: 14px;
    }
    
    .footer-container p:last-child {
        font-size: 12px;
    }
}

@media (max-width: 360px) {
    nav {
        gap: 4px;
    }
    
    nav a {
        font-size: 11px;
        padding: 2px 4px;
    }
    
    .hero h1 {
        font-size: 20px;
    }
    
    .projects-grid {
        grid-template-columns: 1fr 1fr;
        gap: 8px;
    }
    
    .project-tag {
        font-size: 11px;
        height: 34px;
        padding: 0 6px;
    }
}
</style>