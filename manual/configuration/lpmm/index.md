# 📚 LPMM 知识库

<div class="manual-hero">
  <div class="hero-content">
    <h2 class="hero-title">LPMM 知识库模块</h2>
    <p class="hero-subtitle">通过 OpenIE 技术导入自定义知识，让麦麦拥有专属的领域知识，回复更加智能和个性化</p>
    <div class="hero-decoration"></div>
  </div>
</div>

## 文档导航

<div class="manual-nav-grid">

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">📖</span>
  <div class="card-title-group">
    <h3 class="card-title">使用说明</h3>
    <p class="card-desc">详细的 LPMM 安装、配置和使用指南</p>
  </div>
</div>

<div class="card-links">
  <a href="./lpmm" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看使用说明</span>
  </a>
</div>

<div class="file-info">
  <p>🖥️ 覆盖各平台的安装方法</p>
  <p>⚙️ 包含配置和运行步骤</p>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🔧</span>
  <div class="card-title-group">
    <h3 class="card-title">手动编译</h3>
    <p class="card-desc">针对无预编译版本的平台</p>
  </div>
</div>

<div class="card-links">
  <a href="./lpmm_compile_and_install" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看编译指南</span>
  </a>
</div>

<div class="file-info">
  <p>🐧 适用于 Linux/macOS 用户</p>
  <p>📦 包含依赖安装和编译步骤</p>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">📋</span>
  <div class="card-title-group">
    <h3 class="card-title">导入文件格式</h3>
    <p class="card-desc">知识库导入文件格式说明</p>
  </div>
</div>

<div class="card-links">
  <a href="./lpmm_knowledge_template" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看格式要求</span>
  </a>
</div>

<div class="file-info">
  <p>📄 文件命名需以 <code>-openie.json</code> 结尾</p>
  <p>📝 规范的知识条目格式</p>
</div>

</div>

</div>

::: warning 注意
本知识库与旧版不兼容，如果需要将旧版数据库中的内容迁移到新版，请重新导入。
:::

## 快速入门

<div class="quick-start-container">

<div class="step-card">
  <div class="step-number">1</div>
  <div class="step-content">
    <h4>安装依赖</h4>
    <p>Windows 用户：依赖已包含在 requirements.txt 中，执行 <code>pip install quick_algo</code></p>
    <p>Linux/macOS 用户：参考 <a href="./lpmm_compile_and_install">手动编译说明</a></p>
  </div>
</div>

<div class="step-card">
  <div class="step-number">2</div>
  <div class="step-content">
    <h4>准备知识文件</h4>
    <p>将知识文本整理成合适格式，参考 <a href="./lpmm_knowledge_template">导入文件格式说明</a></p>
  </div>
</div>

<div class="step-card">
  <div class="step-number">3</div>
  <div class="step-content">
    <h4>导入知识</h4>
    <p>按照 <a href="./lpmm">使用说明</a> 中的步骤进行知识提取和导入</p>
  </div>
</div>

</div>

## 注意事项

::: danger 重要提醒
1. **知识提取需要消耗大量 API 调用**，文本越长费用越高，建议使用较大的模型（32B~72B）但避免过小的模型
2. **知识导入时请求频繁**，可能触发 API 限速，请选择合适的模型
3. **导入过程资源消耗大**，建议在性能较好的设备上运行
:::

## 常见问题

<div class="faq-container">

<div class="faq-item">
  <div class="faq-question">Q: 为什么提取失败？</div>
  <div class="faq-answer">
    <p>A: 请确保你的文本分段良好、没有奇怪的字符。过小的模型（32B 以下）可能导致提取效果差或失败。</p>
  </div>
</div>

<div class="faq-item">
  <div class="faq-question">Q: 如何选择提取模型？</div>
  <div class="faq-answer">
    <p>A: 如果要求不高，建议采用 <code>32B~72B</code> 的模型来提取。如果追求高质量，可以使用 DeepSeek V3 等大模型，但费用会更高。</p>
  </div>
</div>

<div class="faq-item">
  <div class="faq-question">Q: Docker 用户如何使用 LPMM？</div>
  <div class="faq-answer">
    <p>A: Docker 镜像已预编译 LPMM，可以直接运行相关脚本。</p>
  </div>
</div>

</div>

## 获取帮助

<div class="help-container">

<div class="help-card">
  <div class="help-icon">📖</div>
  <h4>查阅文档</h4>
  <p>仔细阅读上述相关文档</p>
</div>

<div class="help-card">
  <div class="help-icon">❓</div>
  <h4>常见问题</h4>
  <p>查看 <a href="/manual/faq/">FAQ</a> 获取更多帮助</p>
</div>

<div class="help-card">
  <div class="help-icon">💬</div>
  <h4>加入社群</h4>
  <p>加入<a href="/manual/other/group">官方社群</a>寻求帮助</p>
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
  margin-bottom: 2rem;
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
  margin-bottom: 1rem;
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

.file-info {
  padding-top: 0.75rem;
  border-top: 1px solid var(--vp-c-divider);
}

.file-info p {
  margin: 0.25rem 0;
  font-size: 0.8rem;
  color: var(--vp-c-text-3);
}

.file-info code {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.75rem;
}

/* ===== Quick Start Section ===== */
.quick-start-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1rem;
  margin-bottom: 2.5rem;
}

.step-card {
  position: relative;
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.25rem;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  transition: all 0.3s ease;
}

.step-card:hover {
  border-color: rgba(50, 205, 50, 0.3);
  box-shadow: 0 4px 20px rgba(50, 205, 50, 0.1);
}

.step-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ff8c00, #32cd32);
  color: white;
  font-weight: 700;
  font-size: 1rem;
  flex-shrink: 0;
}

.step-content h4 {
  margin: 0 0 0.375rem 0;
  font-size: 0.975rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.step-content p {
  margin: 0.25rem 0 0 0;
  font-size: 0.85rem;
  color: var(--vp-c-text-3);
  line-height: 1.5;
}

.step-content code {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.8rem;
}

.step-content a {
  color: #ff8c00;
  text-decoration: none;
  font-weight: 500;
}

.step-content a:hover {
  text-decoration: underline;
}

/* ===== FAQ Section ===== */
.faq-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2.5rem;
}

.faq-item {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 1.25rem 1.5rem;
  transition: all 0.3s ease;
}

.faq-item:hover {
  border-color: rgba(255, 140, 0, 0.3);
  box-shadow: 0 4px 20px rgba(255, 140, 0, 0.08);
}

.faq-question {
  font-weight: 600;
  color: var(--vp-c-text-1);
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.faq-answer p {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-2);
  line-height: 1.6;
}

.faq-answer code {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
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
  
  .quick-start-container {
    grid-template-columns: 1fr;
  }
  
  .help-container {
    grid-template-columns: 1fr;
  }
  
  .faq-item {
    padding: 1rem;
  }
}
</style>
