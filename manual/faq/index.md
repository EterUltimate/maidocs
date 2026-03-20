# ❓ 常见问题

<div class="faq-hero">
  <div class="hero-content">
    <h2 class="hero-title">问题排查与解答</h2>
    <p class="hero-subtitle">汇总部署、配置与日常使用中最常见的问题，帮助您快速定位并解决问题</p>
    <div class="hero-decoration"></div>
  </div>
</div>

<div class="faq-nav-grid">

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">❓</span>
  <div class="card-title-group">
    <h3 class="card-title">常见问题概览</h3>
    <p class="card-desc">通用问题与解决方案</p>
  </div>
</div>

<div class="card-links">
  <a href="./faq-overview" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看全部常见问题</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🪟</span>
  <div class="card-title-group">
    <h3 class="card-title">Windows 问题</h3>
    <p class="card-desc">Windows 系统相关问题</p>
  </div>
</div>

<div class="card-links">
  <a href="./windows" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>Windows 常见问题</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🐧</span>
  <div class="card-title-group">
    <h3 class="card-title">Linux 问题</h3>
    <p class="card-desc">Linux 系统相关问题</p>
  </div>
</div>

<div class="card-links">
  <a href="./linux" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>Linux 常见问题</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🍎</span>
  <div class="card-title-group">
    <h3 class="card-title">macOS 问题</h3>
    <p class="card-desc">macOS 系统相关问题</p>
  </div>
</div>

<div class="card-links">
  <a href="./macos" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>macOS 常见问题</span>
  </a>
</div>

</div>

</div>

## 获取帮助

<div class="help-container">

<div class="help-card">
  <div class="help-icon">📖</div>
  <h4>查阅手册</h4>
  <p>浏览用户手册的相关章节寻找答案</p>
</div>

<div class="help-card">
  <div class="help-icon">💬</div>
  <h4>加入社群</h4>
  <p>加入<a href="../other/group">官方社群</a>获取帮助</p>
</div>

<div class="help-card">
  <div class="help-icon">🐛</div>
  <h4>提交 Issue</h4>
  <p>在 GitHub 上提交问题反馈</p>
</div>

</div>

<style scoped>
/* ===== Hero Section ===== */
.faq-hero {
  position: relative;
  padding: 2.5rem 2rem;
  margin-bottom: 2.5rem;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.08) 0%, rgba(50, 205, 50, 0.08) 100%);
  border: 1px solid rgba(255, 140, 0, 0.15);
  overflow: hidden;
}

.hero-content {
  position: relative;
  z-index: 1;
}

.hero-title {
  margin: 0 0 0.75rem 0;
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(120deg, #ff8c00 30%, #32cd32);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  margin: 0;
  font-size: 1rem;
  color: var(--vp-c-text-2);
  max-width: 600px;
  line-height: 1.6;
}

.hero-decoration {
  position: absolute;
  top: -50%;
  right: -10%;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(255, 140, 0, 0.1) 0%, transparent 70%);
  pointer-events: none;
}

/* ===== Navigation Grid ===== */
.faq-nav-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 3rem;
}

.nav-card {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-card:hover {
  border-color: rgba(255, 140, 0, 0.3);
  box-shadow: 0 8px 30px rgba(255, 140, 0, 0.12);
  transform: translateY(-4px);
}

.card-header {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1.25rem;
}

.card-icon {
  font-size: 2rem;
  line-height: 1;
  flex-shrink: 0;
}

.card-title-group {
  flex: 1;
}

.card-title {
  margin: 0 0 0.25rem 0;
  font-size: 1.125rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.card-desc {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-3);
}

.card-links {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.card-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.625rem 0.875rem;
  border-radius: 8px;
  color: var(--vp-c-text-2);
  font-size: 0.9rem;
  text-decoration: none;
  transition: all 0.2s ease;
  background: rgba(255, 140, 0, 0.03);
}

.card-link:hover {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding-left: 1.125rem;
}

.link-arrow {
  color: #ff8c00;
  font-size: 0.8rem;
  transition: transform 0.2s ease;
}

.card-link:hover .link-arrow {
  transform: translateX(3px);
}

.card-link-main {
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.08) 0%, rgba(50, 205, 50, 0.05) 100%);
  font-weight: 500;
  border-left: 3px solid #ff8c00;
}

.card-link-main:hover {
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.15) 0%, rgba(50, 205, 50, 0.1) 100%);
}

/* ===== Help Section ===== */
.help-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.25rem;
}

.help-card {
  text-align: center;
  padding: 2rem 1.5rem;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  transition: all 0.3s ease;
}

.help-card:hover {
  border-color: rgba(255, 140, 0, 0.3);
  transform: translateY(-3px);
  box-shadow: 0 6px 25px rgba(255, 140, 0, 0.1);
}

.help-icon {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.help-card h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.help-card p {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-3);
  line-height: 1.5;
}

.help-card a {
  color: #ff8c00;
  text-decoration: none;
  font-weight: 500;
}

.help-card a:hover {
  text-decoration: underline;
}

/* ===== Responsive Design ===== */
@media (max-width: 640px) {
  .faq-hero {
    padding: 1.75rem 1.25rem;
  }
  
  .hero-title {
    font-size: 1.4rem;
  }
  
  .hero-subtitle {
    font-size: 0.9rem;
  }
  
  .faq-nav-grid {
    grid-template-columns: 1fr;
  }
  
  .help-container {
    grid-template-columns: 1fr;
  }
}
</style>
