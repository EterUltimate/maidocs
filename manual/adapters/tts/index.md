# 🔊 TTS 语音适配器

<div class="tts-hero">
  <div class="hero-content">
    <h2 class="hero-title">让 MaiBot 开口说话</h2>
    <p class="hero-subtitle">TTS 适配器可将文字消息转换为语音，让你的机器人用声音与群友互动</p>
    <div class="hero-decoration"></div>
  </div>
</div>

<div class="tts-nav-grid">

<div class="nav-card nav-card-main">

<div class="card-header">
  <span class="card-icon">📖</span>
  <div class="card-title-group">
    <h3 class="card-title">配置指南</h3>
    <p class="card-desc">从安装到配置的完整说明</p>
  </div>
</div>

<div class="card-links">
  <a href="./configuration-guide" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>TTS 配置指南</span>
  </a>
  <a href="./configuration-guide#安装步骤" class="card-link">
    <span class="link-arrow">→</span>
    <span>安装步骤</span>
  </a>
  <a href="./configuration-guide#配置文件详解" class="card-link">
    <span class="link-arrow">→</span>
    <span>配置文件详解</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🎵</span>
  <div class="card-title-group">
    <h3 class="card-title">GPT_Sovits</h3>
    <p class="card-desc">基于 GPT-SoVITS 的语音合成</p>
  </div>
</div>

<div class="card-links">
  <a href="./gpt_sovits" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>模块配置</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🤖</span>
  <div class="card-title-group">
    <h3 class="card-title">Qwen_omni</h3>
    <p class="card-desc">通义千问全模态语音合成</p>
  </div>
</div>

<div class="card-links">
  <a href="./qwen_omni" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>模块配置</span>
  </a>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🫘</span>
  <div class="card-title-group">
    <h3 class="card-title">Doubao_TTS</h3>
    <p class="card-desc">字节跳动豆包语音服务</p>
  </div>
</div>

<div class="card-links">
  <a href="./doubao_tts" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>模块配置</span>
  </a>
</div>

</div>

</div>

## 工作原理

<div class="flow-container">

<div class="flow-card">
  <div class="flow-icon">💬</div>
  <h4>接收消息</h4>
  <p>MaiBot 核心生成文字回复</p>
</div>

<div class="flow-arrow">→</div>

<div class="flow-card">
  <div class="flow-icon">🔊</div>
  <h4>TTS 转换</h4>
  <p>适配器将文字转为语音</p>
</div>

<div class="flow-arrow">→</div>

<div class="flow-card">
  <div class="flow-icon">📱</div>
  <h4>发送语音</h4>
  <p>通过 Napcat 发送到 QQ</p>
</div>

</div>

::: tip 前提条件
使用 TTS 适配器需要**同时安装 Napcat 适配器和 TTS 适配器**。如果使用的其他平台消息适配器声明了兼容，则可不安装 Napcat。
:::

## 获取帮助

<div class="help-container">

<div class="help-card">
  <div class="help-icon">📖</div>
  <h4>查阅配置指南</h4>
  <p>详细的 <a href="./configuration-guide">配置指南</a> 帮助你完成设置</p>
</div>

<div class="help-card">
  <div class="help-icon">💬</div>
  <h4>加入社群</h4>
  <p>加入<a href="/manual/other/group">官方社群</a>获取帮助</p>
</div>

<div class="help-card">
  <div class="help-icon">🐛</div>
  <h4>反馈问题</h4>
  <p>在 GitHub 上提交 Issue 反馈</p>
</div>

</div>

<style scoped>
/* ===== Hero Section ===== */
.tts-hero {
  position: relative;
  padding: 2.5rem 2rem;
  margin-bottom: 2.5rem;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(147, 112, 219, 0.1) 0%, rgba(64, 224, 208, 0.08) 100%);
  border: 1px solid rgba(147, 112, 219, 0.2);
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
  background: linear-gradient(120deg, #9370db 30%, #40e0d0);
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
  background: radial-gradient(circle, rgba(147, 112, 219, 0.15) 0%, transparent 70%);
  pointer-events: none;
}

/* ===== Navigation Grid ===== */
.tts-nav-grid {
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
  border-color: rgba(147, 112, 219, 0.3);
  box-shadow: 0 8px 30px rgba(147, 112, 219, 0.12);
  transform: translateY(-4px);
}

.nav-card-main {
  grid-column: 1 / -1;
}

@media (min-width: 800px) {
  .nav-card-main {
    grid-column: span 2;
  }
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
  background: rgba(147, 112, 219, 0.03);
}

.card-link:hover {
  background: rgba(147, 112, 219, 0.1);
  color: #9370db;
  padding-left: 1.125rem;
}

.link-arrow {
  color: #9370db;
  font-size: 0.8rem;
  transition: transform 0.2s ease;
}

.card-link:hover .link-arrow {
  transform: translateX(3px);
}

.card-link-main {
  background: linear-gradient(135deg, rgba(147, 112, 219, 0.08) 0%, rgba(64, 224, 208, 0.05) 100%);
  font-weight: 500;
  border-left: 3px solid #9370db;
}

.card-link-main:hover {
  background: linear-gradient(135deg, rgba(147, 112, 219, 0.15) 0%, rgba(64, 224, 208, 0.1) 100%);
}

/* ===== Flow Section ===== */
.flow-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2.5rem;
  padding: 1.5rem;
  background: var(--vp-c-bg-soft);
  border-radius: 12px;
  border: 1px solid var(--vp-c-divider);
}

.flow-card {
  text-align: center;
  padding: 1.25rem 1.5rem;
  min-width: 140px;
}

.flow-icon {
  font-size: 2rem;
  margin-bottom: 0.75rem;
}

.flow-card h4 {
  margin: 0 0 0.375rem 0;
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.flow-card p {
  margin: 0;
  font-size: 0.8rem;
  color: var(--vp-c-text-3);
}

.flow-arrow {
  font-size: 1.5rem;
  color: #9370db;
  font-weight: 300;
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
  border-color: rgba(147, 112, 219, 0.3);
  transform: translateY(-3px);
  box-shadow: 0 6px 25px rgba(147, 112, 219, 0.1);
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
  color: #9370db;
  text-decoration: none;
  font-weight: 500;
}

.help-card a:hover {
  text-decoration: underline;
}

/* ===== Responsive Design ===== */
@media (max-width: 640px) {
  .tts-hero {
    padding: 1.75rem 1.25rem;
  }
  
  .hero-title {
    font-size: 1.4rem;
  }
  
  .hero-subtitle {
    font-size: 0.9rem;
  }
  
  .tts-nav-grid {
    grid-template-columns: 1fr;
  }
  
  .nav-card-main {
    grid-column: 1;
  }
  
  .flow-container {
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .flow-arrow {
    transform: rotate(90deg);
  }
  
  .help-container {
    grid-template-columns: 1fr;
  }
}
</style>
