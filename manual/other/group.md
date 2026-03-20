# 社区与交流

欢迎加入麦麦社区！这里是与我们联系的各种方式。

<style scoped>
/* ===== CSS Variables ===== */
.community-container {
  --brand-orange: #ff8c00;
  --brand-green: #32cd32;
  --brand-purple: #a371f7;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  margin: 1.5rem 0;
}

/* ===== Section Header ===== */
.section-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid var(--vp-c-divider);
}

.section-icon {
  width: 32px;
  height: 32px;
  flex-shrink: 0;
}

.section-title {
  font-size: 1.35rem;
  font-weight: 700;
  color: var(--vp-c-text-1);
  margin: 0;
  background: linear-gradient(120deg, var(--brand-orange) 30%, var(--brand-green));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* ===== QQ Grid ===== */
.qq-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 3rem;
}

.qq-card {
  position: relative;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: var(--radius-md);
  padding: 1.5rem;
  transition: var(--transition);
  overflow: hidden;
}

.qq-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.03) 0%, rgba(50, 205, 50, 0.02) 100%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.qq-card:hover {
  border-color: rgba(255, 140, 0, 0.4);
  transform: translateY(-3px);
  box-shadow: 0 12px 32px -8px rgba(255, 140, 0, 0.15);
}

.qq-card:hover::before {
  opacity: 1;
}

/* Card accent bar by type */
.qq-card.tech::after,
.qq-card.chat::after,
.qq-card.dev::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.qq-card:hover::after {
  opacity: 1;
}

.qq-card.tech::after {
  background: linear-gradient(90deg, var(--brand-orange), #ffa500);
}

.qq-card.chat::after {
  background: linear-gradient(90deg, var(--brand-green), #90ee90);
}

.qq-card.dev::after {
  background: linear-gradient(90deg, var(--brand-purple), #c49bff);
}

.card-top {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 0.75rem;
  margin-bottom: 1rem;
  position: relative;
  z-index: 1;
}

.group-icon {
  font-size: 2.25rem;
  line-height: 1;
  flex-shrink: 0;
}

.type-tag {
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.3rem 0.65rem;
  border-radius: 20px;
  white-space: nowrap;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.type-tag.tech {
  background: rgba(255, 140, 0, 0.12);
  color: var(--brand-orange);
  border: 1px solid rgba(255, 140, 0, 0.25);
}

.type-tag.chat {
  background: rgba(50, 205, 50, 0.12);
  color: var(--brand-green);
  border: 1px solid rgba(50, 205, 50, 0.25);
}

.type-tag.dev {
  background: rgba(163, 113, 247, 0.12);
  color: var(--brand-purple);
  border: 1px solid rgba(163, 113, 247, 0.25);
}

.group-name {
  font-size: 1.15rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
  margin: 0 0 1.25rem 0;
  line-height: 1.3;
  position: relative;
  z-index: 1;
}

.join-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.6rem 1.1rem;
  background: linear-gradient(135deg, var(--brand-orange) 0%, #ff6b00 100%);
  color: white;
  text-decoration: none;
  border-radius: var(--radius-sm);
  font-size: 0.85rem;
  font-weight: 600;
  transition: var(--transition);
  box-shadow: 0 4px 12px rgba(255, 140, 0, 0.25);
  position: relative;
  z-index: 1;
}

.join-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 140, 0, 0.35);
}

.join-btn svg {
  width: 14px;
  height: 14px;
}

/* ===== Social Grid ===== */
.social-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1rem;
  margin-bottom: 2.5rem;
}

.social-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: var(--radius-md);
  padding: 1.25rem 1.5rem;
  text-decoration: none;
  color: var(--vp-c-text-1);
  transition: var(--transition);
}

.social-card:hover {
  border-color: var(--brand-green);
  background: linear-gradient(135deg, rgba(50, 205, 50, 0.05) 0%, rgba(50, 205, 50, 0.02) 100%);
  transform: translateX(4px);
  box-shadow: 0 8px 24px -8px rgba(50, 205, 50, 0.15);
}

.social-card:hover .social-icon svg {
  transform: scale(1.1);
}

.social-icon {
  width: 28px;
  height: 28px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.social-icon svg {
  width: 100%;
  height: 100%;
  transition: transform 0.25s ease;
}

/* Brand colors for each platform */
.social-card.github .social-icon svg {
  fill: var(--vp-c-text-1);
}

.social-card.twitter .social-icon svg {
  fill: #1da1f2;
}

.social-card.discord .social-icon svg {
  fill: #5865f2;
}

.social-card.telegram .social-icon svg {
  fill: #0088cc;
}

.social-info {
  flex: 1;
  min-width: 0;
}

.social-name {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
  margin-bottom: 0.2rem;
}

.social-handle {
  font-size: 0.75rem;
  color: var(--vp-c-text-3);
  font-family: var(--vp-font-family-mono);
}

/* ===== Tip Box ===== */
.community-tip {
  margin-top: 2rem;
  padding: 1.25rem 1.5rem;
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.06) 0%, rgba(50, 205, 50, 0.04) 100%);
  border: 1px solid rgba(255, 140, 0, 0.15);
  border-radius: var(--radius-md);
  border-left: 4px solid var(--brand-orange);
}

.community-tip p {
  margin: 0;
  font-size: 0.9rem;
  color: var(--vp-c-text-1);
  display: flex;
  align-items: center;
  gap: 0.6rem;
  line-height: 1.6;
}

.tip-icon {
  font-size: 1.2rem;
  flex-shrink: 0;
}

/* ===== Dark Mode ===== */
:global(.dark) .type-tag.tech {
  background: rgba(255, 140, 0, 0.18);
  color: #ffb347;
  border-color: rgba(255, 140, 0, 0.3);
}

:global(.dark) .type-tag.chat {
  background: rgba(50, 205, 50, 0.18);
  color: #6ee66e;
  border-color: rgba(50, 205, 50, 0.3);
}

:global(.dark) .type-tag.dev {
  background: rgba(163, 113, 247, 0.18);
  color: #c49bff;
  border-color: rgba(163, 113, 247, 0.3);
}

:global(.dark) .community-tip {
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.1) 0%, rgba(50, 205, 50, 0.06) 100%);
}

:global(.dark) .social-card:hover {
  background: linear-gradient(135deg, rgba(50, 205, 50, 0.08) 0%, rgba(50, 205, 50, 0.03) 100%);
}

/* ===== Responsive ===== */
@media (max-width: 640px) {
  .qq-grid,
  .social-grid {
    grid-template-columns: 1fr;
  }
  
  .section-header {
    flex-wrap: wrap;
  }
}
</style>

<div class="community-container">

<!-- QQ Groups Section -->
<div class="section-header">
  <svg class="section-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
    <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path>
  </svg>
  <h2 class="section-title">QQ 群</h2>
</div>

<div class="qq-grid">

<div class="qq-card tech">
  <div class="card-top">
    <span class="group-icon">🧠</span>
    <span class="type-tag tech">技术交流</span>
  </div>
  <h3 class="group-name">麦麦脑电图</h3>
  <a href="https://qm.qq.com/q/RzmCiRtHEW" target="_blank" rel="noopener" class="join-btn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10 17 15 12 10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>
    加入群聊
  </a>
</div>

<div class="qq-card tech">
  <div class="card-top">
    <span class="group-icon">🔮</span>
    <span class="type-tag tech">技术交流</span>
  </div>
  <h3 class="group-name">麦麦大脑磁共振</h3>
  <a href="https://qm.qq.com/q/VQ3XZrWgMs" target="_blank" rel="noopener" class="join-btn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10 17 15 12 10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>
    加入群聊
  </a>
</div>

<div class="qq-card tech">
  <div class="card-top">
    <span class="group-icon">🎭</span>
    <span class="type-tag tech">技术交流</span>
  </div>
  <h3 class="group-name">麦麦要当VTB</h3>
  <a href="https://qm.qq.com/q/wGePTl1UyY" target="_blank" rel="noopener" class="join-btn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10 17 15 12 10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>
    加入群聊
  </a>
</div>

<div class="qq-card chat">
  <div class="card-top">
    <span class="group-icon">🌊</span>
    <span class="type-tag chat">闲聊吹水</span>
  </div>
  <h3 class="group-name">麦麦之闲聊群</h3>
  <a href="https://qm.qq.com/q/JxvHZnxyec" target="_blank" rel="noopener" class="join-btn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10 17 15 12 10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>
    加入群聊
  </a>
</div>

<div class="qq-card dev">
  <div class="card-top">
    <span class="group-icon">🧩</span>
    <span class="type-tag dev">插件开发</span>
  </div>
  <h3 class="group-name">插件开发群</h3>
  <a href="https://qm.qq.com/q/1ibfetCXoY" target="_blank" rel="noopener" class="join-btn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10 17 15 12 10 7"/><line x1="15" y1="12" x2="3" y2="12"/></svg>
    加入群聊
  </a>
</div>

</div>

<!-- Social Media Section -->
<div class="section-header">
  <svg class="section-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
    <circle cx="12" cy="12" r="10"></circle>
    <line x1="2" y1="12" x2="22" y2="12"></line>
    <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path>
  </svg>
  <h2 class="section-title">社交媒体</h2>
</div>

<div class="social-grid">

<a href="https://github.com/MaiM-with-u/MaiBot" target="_blank" rel="noopener" class="social-card github">
  <div class="social-icon">
    <svg viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
  </div>
  <div class="social-info">
    <div class="social-name">GitHub</div>
    <div class="social-handle">MaiM-with-u/MaiBot</div>
  </div>
</a>

<a href="https://x.com/MaiWithYou" target="_blank" rel="noopener" class="social-card twitter">
  <div class="social-icon">
    <svg viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
  </div>
  <div class="social-info">
    <div class="social-name">X (Twitter)</div>
    <div class="social-handle">@MaiWithYou</div>
  </div>
</a>

<a href="https://discord.gg/UvgPVSVX" target="_blank" rel="noopener" class="social-card discord">
  <div class="social-icon">
    <svg viewBox="0 0 24 24"><path d="M20.317 4.3698a19.7913 19.7913 0 00-4.8851-1.5152.0741.0741 0 00-.0785.0371c-.211.3753-.4447.8648-.6083 1.2495-1.8447-.2762-3.68-.2762-5.4868 0-.1636-.3933-.4058-.8742-.6177-1.2495a.077.077 0 00-.0785-.037 19.7363 19.7363 0 00-4.8852 1.515.0699.0699 0 00-.0321.0277C.5334 9.0458-.319 13.5799.0992 18.0578a.0824.0824 0 00.0312.0561c2.0528 1.5076 4.0413 2.4228 5.9929 3.0294a.0777.0777 0 00.0842-.0276c.4616-.6304.8731-1.2952 1.226-1.9942a.076.076 0 00-.0416-.1057c-.6528-.2476-1.2743-.5495-1.8722-.8923a.077.077 0 01-.0076-.1277c.1258-.0943.2517-.1923.3718-.2914a.0743.0743 0 01.0776-.0105c3.9278 1.7933 8.18 1.7933 12.0614 0a.0739.0739 0 01.0785.0095c.1202.099.246.1981.3728.2924a.077.077 0 01-.0066.1276 12.2986 12.2986 0 01-1.873.8914.0766.0766 0 00-.0407.1067c.3604.698.7719 1.3628 1.225 1.9932a.076.076 0 00.0842.0286c1.961-.6067 3.9495-1.5219 6.0023-3.0294a.077.077 0 00.0313-.0552c.5004-5.177-.8382-9.6739-3.5485-13.6604a.061.061 0 00-.0312-.0286zM8.01 15.3212c-1.1885 0-2.1714-1.0913-2.1714-2.4322 0-1.3409.961-2.4322 2.1714-2.4322 1.217 0 2.1881 1.101 2.1714 2.4322 0 1.3409-.9611 2.4322-2.1714 2.4322zm7.8323 0c-1.1885 0-2.1714-1.0913-2.1714-2.4322 0-1.3409.9609-2.4322 2.1714-2.4322 1.217 0 2.1882 1.101 2.1714 2.4322 0 1.3409-.9611 2.4322-2.1714 2.4322z"/></svg>
  </div>
  <div class="social-info">
    <div class="social-name">Discord</div>
    <div class="social-handle">加入服务器</div>
  </div>
</a>

<a href="https://t.me/MaiWithYou" target="_blank" rel="noopener" class="social-card telegram">
  <div class="social-icon">
    <svg viewBox="0 0 24 24"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.48.33-.913.49-1.302.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>
  </div>
  <div class="social-info">
    <div class="social-name">Telegram</div>
    <div class="social-handle">@MaiWithYou</div>
  </div>
</a>

</div>

<!-- Tip -->
<div class="community-tip">
  <p><span class="tip-icon">💡</span> 技术交流群仅供讨论技术问题和答疑，闲聊请加入吹水群。</p>
</div>

</div>
