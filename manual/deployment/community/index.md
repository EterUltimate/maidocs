# 🌐 社区部署方案

<div class="manual-hero">
  <div class="hero-content">
    <h2 class="hero-title">社区部署方案</h2>
    <p class="hero-subtitle">由社区成员贡献的各种部署方案，为不同场景和需求的用户提供更多选择</p>
    <div class="hero-decoration"></div>
  </div>
</div>

::: tip 提示
这些部署方案由社区成员维护，如有问题请在对应项目的 Issue 中反馈，或加入[官方社群](/manual/other/group)寻求帮助。
:::

## 部署方案列表

<div class="manual-nav-grid">

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">📱</span>
  <div class="card-title-group">
    <h3 class="card-title">Android 部署</h3>
    <p class="card-desc">通过 ZeroTermux 在移动端运行</p>
  </div>
</div>

<ul class="card-features">
  <li>无需服务器，手机即可运行</li>
  <li>适合个人使用和测试</li>
  <li>配置相对复杂，适合有 Linux 基础</li>
</ul>

<div class="card-links">
  <a href="./mmc_deploy_android" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>Android 部署指南</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">☸️</span>
  <div class="card-title-group">
    <h3 class="card-title">Kubernetes 部署</h3>
    <p class="card-desc">使用 Helm Chart 在 K8s 集群部署</p>
  </div>
</div>

<ul class="card-features">
  <li>支持水平扩展和高可用</li>
  <li>适合有 K8s 经验的用户</li>
  <li>需要已部署的 Kubernetes 集群</li>
</ul>

<div class="card-links">
  <a href="./mmc_deploy_kubernetes" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>Kubernetes 部署指南</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🖥️</span>
  <div class="card-title-group">
    <h3 class="card-title">1Panel 部署</h3>
    <p class="card-desc">通过 1Panel 面板快速部署</p>
  </div>
</div>

<div class="card-author">作者：梦归云帆</div>

<ul class="card-features">
  <li>图形化管理界面</li>
  <li>一键安装和更新</li>
  <li>适合不熟悉命令行的用户</li>
</ul>

<div class="card-links">
  <a href="./1panel" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>1Panel 部署指南</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🐧</span>
  <div class="card-title-group">
    <h3 class="card-title">Linux 一键脚本</h3>
    <p class="card-desc">自动化脚本快速部署</p>
  </div>
</div>

<div class="card-author">作者：Astriora</div>

<ul class="card-features">
  <li>自动化程度高</li>
  <li>快速部署</li>
  <li>适合 Linux 服务器环境</li>
</ul>

<div class="card-links">
  <a href="./linux_one_key" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>Linux 一键脚本部署指南</span>
  </a>
</div>

</div>

</div>

## 贡献部署方案

<div class="contribute-container">

<div class="contribute-card">
  <div class="contribute-icon">🚀</div>
  <h4>分享你的部署方案</h4>
  <p>如果你有新的部署方案想要贡献，欢迎提交到文档仓库</p>
  <div class="contribute-steps">
    <div class="contribute-step">
      <span class="step-num">1</span>
      <span>Fork <a href="https://github.com/Mai-with-u/docs" target="_blank">文档仓库</a></span>
    </div>
    <div class="contribute-step">
      <span class="step-num">2</span>
      <span>添加部署文档到 <code>manual/deployment/community/</code></span>
    </div>
    <div class="contribute-step">
      <span class="step-num">3</span>
      <span>更新本页面添加方案介绍</span>
    </div>
    <div class="contribute-step">
      <span class="step-num">4</span>
      <span>提交 Pull Request</span>
    </div>
  </div>
</div>

</div>

## 故障排除

<div class="help-container">

<div class="help-card">
  <div class="help-icon">❓</div>
  <h4>常见问题</h4>
  <p>查阅 <a href="/manual/faq/">FAQ</a> 中的常见问题</p>
</div>

<div class="help-card">
  <div class="help-icon">🔍</div>
  <h4>查看 Issue</h4>
  <p>查看对应部署方案的 Issue 列表</p>
</div>

<div class="help-card">
  <div class="help-icon">💬</div>
  <h4>加入社群</h4>
  <p>加入<a href="/manual/other/group">麦麦社群</a>获取帮助</p>
</div>

</div>

<style scoped>
/* ===== Hero Section ===== */
.manual-hero {
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
.manual-nav-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.25rem;
  margin-bottom: 2.5rem;
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
  margin-bottom: 0.75rem;
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

.card-author {
  font-size: 0.8rem;
  color: #ff8c00;
  margin-bottom: 0.75rem;
  font-weight: 500;
}

.card-features {
  list-style: none;
  padding: 0;
  margin: 0 0 1rem 0;
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
}

.card-features li {
  position: relative;
  padding-left: 1.25rem;
  font-size: 0.85rem;
  color: var(--vp-c-text-2);
  line-height: 1.5;
}

.card-features li::before {
  content: "•";
  position: absolute;
  left: 0;
  color: #32cd32;
  font-weight: bold;
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

/* ===== Contribute Section ===== */
.contribute-container {
  margin-bottom: 2.5rem;
}

.contribute-card {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 2rem;
  text-align: center;
  transition: all 0.3s ease;
}

.contribute-card:hover {
  border-color: rgba(50, 205, 50, 0.3);
  box-shadow: 0 6px 25px rgba(50, 205, 50, 0.1);
}

.contribute-icon {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.contribute-card h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1.125rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.contribute-card > p {
  margin: 0 0 1.5rem 0;
  font-size: 0.9rem;
  color: var(--vp-c-text-3);
}

.contribute-steps {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 1rem;
  text-align: left;
}

.contribute-step {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  background: rgba(255, 140, 0, 0.04);
  border-radius: 8px;
  font-size: 0.85rem;
  color: var(--vp-c-text-2);
  transition: all 0.2s ease;
}

.contribute-step:hover {
  background: rgba(255, 140, 0, 0.08);
}

.step-num {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ff8c00, #32cd32);
  color: white;
  font-weight: 700;
  font-size: 0.75rem;
  flex-shrink: 0;
}

.contribute-step a {
  color: #ff8c00;
  text-decoration: none;
  font-weight: 500;
}

.contribute-step a:hover {
  text-decoration: underline;
}

.contribute-step code {
  background: rgba(255, 140, 0, 0.1);
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.8rem;
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
  .manual-hero {
    padding: 1.75rem 1.25rem;
  }
  
  .hero-title {
    font-size: 1.4rem;
  }
  
  .hero-subtitle {
    font-size: 0.9rem;
  }
  
  .manual-nav-grid {
    grid-template-columns: 1fr;
  }
  
  .contribute-steps {
    grid-template-columns: 1fr;
  }
  
  .help-container {
    grid-template-columns: 1fr;
  }
}
</style>
