<script setup>
import { onMounted, onUnmounted, nextTick } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/src/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

let animations = [];

onMounted(async () => {
  await nextTick();
  
  // 刷新ScrollTrigger确保正确计算
  ScrollTrigger.refresh();
  
  setTimeout(() => {
    initScrollAnimations();
    initInteractiveFeatures();
  }, 200);
});

onUnmounted(() => {
  // 清理所有GSAP动画和ScrollTrigger实例
  animations.forEach(anim => {
    if (anim.scrollTrigger) {
      anim.scrollTrigger.kill();
    }
    anim.kill();
  });
  ScrollTrigger.getAll().forEach(trigger => trigger.kill());
  
  // 清理所有observers
  if (window.devObservers) {
    window.devObservers.forEach(observer => observer.disconnect());
    window.devObservers = null;
  }
});

function initScrollAnimations() {
  // 设置初始状态 - 使用更简单的变换
  gsap.set(".dev-header", {
    opacity: 0,
    y: 20
  });

  gsap.set(".dev-section", {
    opacity: 0,
    y: 40
  });

  gsap.set(".section-icon", {
    scale: 0.8,
    opacity: 0
  });

  gsap.set(".feature-item", {
    opacity: 0,
    x: -15
  });

  gsap.set(".step", {
    opacity: 0,
    y: 20
  });

  gsap.set(".contribute-step", {
    opacity: 0,
    x: -20
  });

  gsap.set(".tech-item", {
    opacity: 0,
    scale: 0.8
  });

  // Header动画 - 使用Intersection Observer而不是ScrollTrigger
  const headerObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        gsap.to(".dev-header", {
          opacity: 1,
          y: 0,
          duration: 0.8,
          ease: "power2.out"
        });
      }
    });
  }, { threshold: 0.3 });

  headerObserver.observe(document.querySelector(".dev-header"));

  // GitHub section - 使用Intersection Observer
  const githubObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting && entry.target.classList.contains("github-section")) {
        const tl = gsap.timeline();
        tl.to(".github-section", {
          opacity: 1,
          y: 0,
          duration: 0.6,
          ease: "power2.out"
        })
        .to(".github-section .section-icon", {
          scale: 1,
          opacity: 1,
          duration: 0.4,
          ease: "back.out(1.2)"
        }, "-=0.3")
        .to(".github-section .feature-item", {
          opacity: 1,
          x: 0,
          duration: 0.3,
          stagger: 0.08,
          ease: "power2.out"
        }, "-=0.2");
      }
    });
  }, { threshold: 0.2 });

  githubObserver.observe(document.querySelector(".github-section"));

  // CI/CD section - 使用Intersection Observer
  const cicdObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting && entry.target.classList.contains("cicd-section")) {
        const tl = gsap.timeline();
        tl.to(".cicd-section", {
          opacity: 1,
          y: 0,
          duration: 0.6,
          ease: "power2.out"
        })
        .to(".cicd-section .section-icon", {
          scale: 1,
          opacity: 1,
          duration: 0.4,
          ease: "back.out(1.2)"
        }, "-=0.3")
        .to(".step", {
          opacity: 1,
          y: 0,
          duration: 0.3,
          stagger: 0.1,
          ease: "power2.out"
        }, "-=0.2");
      }
    });
  }, { threshold: 0.2 });

  cicdObserver.observe(document.querySelector(".cicd-section"));

  // Contribute section - 使用Intersection Observer
  const contributeObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting && entry.target.classList.contains("contribute-section")) {
        const tl = gsap.timeline();
        tl.to(".contribute-section", {
          opacity: 1,
          y: 0,
          duration: 0.6,
          ease: "power2.out"
        })
        .to(".contribute-section .section-icon", {
          scale: 1,
          opacity: 1,
          duration: 0.4,
          ease: "back.out(1.2)"
        }, "-=0.3")
        .to(".contribute-step", {
          opacity: 1,
          x: 0,
          duration: 0.3,
          stagger: 0.1,
          ease: "power2.out"
        }, "-=0.2")
        .to(".tech-item", {
          opacity: 1,
          scale: 1,
          duration: 0.3,
          stagger: 0.05,
          ease: "back.out(1.2)"
        }, "-=0.1");
      }
    });
  }, { threshold: 0.2 });

  contributeObserver.observe(document.querySelector(".contribute-section"));

  // 简化鼠标跟随效果 - 使用节流
  document.querySelectorAll(".dev-section").forEach(section => {
    let throttleTimer = null;
    
    section.addEventListener("mousemove", (e) => {
      if (throttleTimer) return;
      
      throttleTimer = setTimeout(() => {
        const rect = section.getBoundingClientRect();
        const x = (e.clientX - rect.left) / rect.width - 0.5;
        const y = (e.clientY - rect.top) / rect.height - 0.5;
        
        gsap.to(section, {
          rotationY: x * 1,
          rotationX: y * -1,
          duration: 0.6,
          ease: "power1.out"
        });
        
        gsap.to(section.querySelector(".section-icon"), {
          x: x * 4,
          y: y * 4,
          duration: 0.6,
          ease: "ease"
        });
        
        throttleTimer = null;
      }, 16); // 约60fps
    });
    
    section.addEventListener("mouseleave", () => {
      gsap.to(section, {
        rotationY: 0,
        rotationX: 0,
        duration: 0.8,
        ease: "ease"
      });
      
      gsap.to(section.querySelector(".section-icon"), {
        x: 0,
        y: 0,
        duration: 0.8,
        ease: "power2.out"
      });
    });
  });
}

function initInteractiveFeatures() {
  // 代码复制功能
  const copyButtons = document.querySelectorAll(".copy-btn");
  copyButtons.forEach(btn => {
    btn.addEventListener("click", async () => {
      const code = btn.previousElementSibling.textContent;
      try {
        await navigator.clipboard.writeText(code);
        
        // 添加复制成功动画
        gsap.to(btn, {
          scale: 1.1,
          duration: 0.1,
          yoyo: true,
          repeat: 1,
          ease: "power2.inOut"
        });
        
        btn.textContent = "已复制!";
        btn.classList.add("copied");
        setTimeout(() => {
          btn.textContent = "复制";
          btn.classList.remove("copied");
        }, 2000);
      } catch (err) {
        console.error("复制失败:", err);
      }
    });
  });

  // 技术栈项目悬停动画
  document.querySelectorAll(".tech-item").forEach(item => {
    item.addEventListener("mouseenter", () => {
      gsap.to(item, {
        scale: 1.05,
        y: -5,
        duration: 0.3,
        ease: "power2.out"
      });
    });
    
    item.addEventListener("mouseleave", () => {
      gsap.to(item, {
        scale: 1,
        y: 0,
        duration: 0.3,
        ease: "power2.out"
      });
    });
  });

  // 步骤数字脉冲动画 - 完全移除避免干扰滚动
  // 改为静态样式，不使用动画
}
</script>

<template>
  <div class="dev-container">
    <header class="dev-header">
      <h1>
        <div class="dev-header-dev">
            开发
            <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#1f1f1f"><path d="M686-132 444-376q-20 8-40.5 12t-43.5 4q-100 0-170-70t-70-170q0-36 10-68.5t28-61.5l146 146 72-72-146-146q29-18 61.5-28t68.5-10q100 0 170 70t70 170q0 23-4 43.5T584-516l244 242q12 12 12 29t-12 29l-84 84q-12 12-29 12t-29-12Zm29-85 27-27-256-256q18-20 26-46.5t8-53.5q0-60-38.5-104.5T386-758l74 74q12 12 12 28t-12 28L332-500q-12 12-28 12t-28-12l-74-74q9 57 53.5 95.5T360-440q26 0 52-8t47-25l256 256ZM472-488Z"/></svg>

        </div>
        友好，快速集成</h1>
      <p>基于现代化工作流，让贡献变得简单高效</p>
    </header>
    
    <main class="dev-main">
      <!-- GitHub 集成部分 -->
      <section class="github-section dev-section">
        <div class="section-icon">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 0C5.374 0 0 5.373 0 12 0 17.302 3.438 21.8 8.207 23.387c.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0112 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"/>
          </svg>
        </div>
        <div class="section-content">
          <h2>GitHub 集成</h2>
          <p>项目托管在 GitHub 上，支持标准的 Git 工作流，包括 Fork、Pull Request 等协作方式</p>
          <div class="code-block">
            <code>git clone https://github.com/ATRIOR-LCL/oj-competition-side-client-homepage.git</code>
            <button class="copy-btn">复制</button>
          </div>
          <div class="features">
            <div class="feature-item">
              <span class="feature-icon">🔀</span>
              <span>分支管理</span>
            </div>
            <div class="feature-item">
              <span class="feature-icon">📋</span>
              <span>Issue 跟踪</span>
            </div>
            <div class="feature-item">
              <span class="feature-icon">🔍</span>
              <span>代码审查</span>
            </div>
          </div>
        </div>
      </section>

      <!-- CI/CD 部分 -->
      <section class="cicd-section dev-section">
        <div class="section-icon">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 2L2 7v10c0 5.55 3.84 9.74 9 11 5.16-1.26 9-5.45 9-11V7l-10-5z"/>
          </svg>
        </div>
        <div class="section-content">
          <h2>自动化 CI/CD</h2>
          <p>配置了完整的持续集成和部署流水线，确保代码质量和部署自动化</p>
          <div class="pipeline-steps">
            <div class="step">
              <div class="step-number">1</div>
              <div class="step-content">
                <h3>代码检查</h3>
                <p>ESLint + Prettier 代码质量检查</p>
              </div>
            </div>
            <div class="step">
              <div class="step-number">2</div>
              <div class="step-content">
                <h3>自动构建</h3>
                <p>Vite 构建优化，生成生产版本</p>
              </div>
            </div>
            <div class="step">
              <div class="step-number">3</div>
              <div class="step-content">
                <h3>自动部署</h3>
                <p>部署到生产环境，支持回滚</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 贡献指南部分 -->
      <section class="contribute-section dev-section">
        <div class="section-icon">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M16 4c0-1.11.89-2 2-2s2 .89 2 2-.89 2-2 2-2-.89-2-2zM4 18v-4h2v4h2v2H4c-1.1 0-2-.9-2-2zm14-3.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5zm3 .5h2v2h-2v2h-2v-2h-2v-2h2v-2h2v2zm-6-6.5c0-1.1-.9-2-2-2s-2 .9-2 2 .9 2 2 2 2-.9 2-2zM2.5 12C1.67 12 1 11.33 1 10.5S1.67 9 2.5 9 4 9.67 4 10.5 3.33 12 2.5 12z"/>
          </svg>
        </div>
        <div class="section-content">
          <h2>快速贡献</h2>
          <p>简单的开发流程，让每个人都能轻松参与项目贡献</p>
          <div class="contribute-steps">
            <div class="contribute-step">
              <span class="step-emoji">🍴</span>
              <div class="step-text">
                <h4>Fork 项目</h4>
                <p>点击 Fork 按钮创建个人副本</p>
              </div>
            </div>
            <div class="contribute-step">
              <span class="step-emoji">💻</span>
              <div class="step-text">
                <h4>本地开发</h4>
                <div class="code-block">
                  <code>npm install && npm run dev</code>
                  <button class="copy-btn">复制</button>
                </div>
              </div>
            </div>
            <div class="contribute-step">
              <span class="step-emoji">🚀</span>
              <div class="step-text">
                <h4>提交 PR</h4>
                <p>创建 Pull Request 提交你的改进</p>
              </div>
            </div>
          </div>
          
          <div class="tech-stack">
            <h3>技术栈</h3>
            <div class="tech-items">
              <span class="tech-item">Vue 3</span>
              <span class="tech-item">Vite</span>
              <span class="tech-item">GSAP</span>
              <span class="tech-item">Less</span>
              <span class="tech-item">GitHub Actions</span>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped lang="less">
@import url(../../assets/css/inline-style.less);
.dev-container {
  width: 100vw;
  min-height: 100vh;
  position: relative;
  left: 0;
  top: 0;
  margin: 0;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, var(--bg-primary-color) 0%, var(--bg-secondary-color) 100%);
  scroll-behavior: auto;
  transform: translateZ(0);
  -webkit-overflow-scrolling: touch;
}

.dev-header {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  opacity: 0;
  will-change: transform, opacity;
  transform: translateZ(0);

  &-dev {
    .inline-style(
        var(--green-bg-color),
        var(--green-font-color)
    );
  }
  
  h1 {
    font-size: 3rem;
    font-weight: 700;
    color: var(--text-color);
    margin-bottom: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  p {
    font-size: 1.2rem;
    color: var(--text-secondary-color);
    max-width: 600px;
  }
}

.dev-main {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 4rem;
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.dev-section {
  display: flex;
  gap: 2rem;
  padding: 3rem;
  background: var(--bg-secondary-color);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  opacity: 0;
  transform-style: preserve-3d;
  perspective: 1000px;
  will-change: transform, opacity;
  transform: translateZ(0);
  
  &:hover {
    box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
    transition: box-shadow 0.3s ease;
  }
}

.section-icon {
  flex-shrink: 0;
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  background: var(--green-bg-color);
  
  svg {
    width: 40px;
    height: 40px;
    color: var(--blue-font-color);
  }
}

.section-content {
  flex: 1;
  
  h2 {
    font-size: 2rem;
    font-weight: 600;
    color: var(--text-color);
    margin-bottom: 1rem;
  }
  
  p {
    font-size: 1.1rem;
    color: var(--text-secondary-color);
    line-height: 1.6;
    margin-bottom: 1.5rem;
  }
}

.code-block {
  display: flex;
  align-items: center;
  gap: 1rem;
  background: #f8f9fa;
  padding: 1rem;
  border-radius: var(--border-small-radius);
  border-left: 4px solid var(--blue-font-color);
  margin: 1rem 0;
  
  code {
    flex: 1;
    font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
    font-size: 0.9rem;
    color: #2d3748;
    background: none;
  }
}

.copy-btn {
  padding: 0.5rem 1rem;
  background: var(--blue-font-color);
  color: white;
  border: none;
  border-radius: var(--border-small-radius);
  cursor: pointer;
  font-size: 0.8rem;
  transition: all 0.3s ease;
  
  &:hover {
    background: var(--green-font-color);
    transform: scale(1.05);
  }
  
  &.copied {
    background: var(--green-font-color);
  }
}

.features {
  display: flex;
  gap: 2rem;
  margin-top: 1.5rem;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background: var(--blue-bg-color);
  border-radius: var(--border-small-radius);
  color: var(--blue-font-color);
  font-weight: 500;
  
  .feature-icon {
    font-size: 1.2rem;
  }
}

.pipeline-steps {
  display: flex;
  gap: 2rem;
  margin-top: 2rem;
}

.step {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  
  .step-number {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--green-bg-color), var(--yellow-bg-color));
    color: var(--green-font-color);
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 700;
    font-size: 1.2rem;
    margin-bottom: 1rem;
  }
  
  .step-content {
    h3 {
      font-size: 1.2rem;
      color: var(--text-color);
      margin-bottom: 0.5rem;
    }
    
    p {
      font-size: 0.9rem;
      color: var(--text-secondary-color);
      margin: 0;
    }
  }
}

.contribute-steps {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin: 2rem 0;
}

.contribute-step {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  
  .step-emoji {
    font-size: 2rem;
    flex-shrink: 0;
  }
  
  .step-text {
    flex: 1;
    
    h4 {
      font-size: 1.2rem;
      color: var(--text-color);
      margin-bottom: 0.5rem;
    }
    
    p {
      margin: 0;
      font-size: 1rem;
    }
  }
}

.tech-stack {
  margin-top: 2rem;
  padding-top: 2rem;
  border-top: 2px solid var(--border-color);
  
  h3 {
    font-size: 1.3rem;
    color: var(--text-color);
    margin-bottom: 1rem;
  }
}

.tech-items {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.tech-item {
  padding: 0.5rem 1rem;
  background: linear-gradient(135deg, var(--pink-bg-color), var(--yellow-bg-color));
  color: var(--pink-font-color);
  border-radius: var(--border-small-radius);
  font-weight: 500;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  
  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .dev-header h1 {
    font-size: 2rem;
  }
  
  .dev-main {
    padding: 1rem;
    gap: 2rem;
  }
  
  .dev-section {
    flex-direction: column;
    padding: 2rem;
    gap: 1rem;
  }
  
  .section-icon {
    width: 60px;
    height: 60px;
    align-self: center;
    
    svg {
      width: 30px;
      height: 30px;
    }
  }
  
  .pipeline-steps {
    flex-direction: column;
    gap: 1rem;
  }
  
  .features {
    flex-direction: column;
    gap: 1rem;
  }
  
  .code-block {
    flex-direction: column;
    align-items: stretch;
    gap: 0.5rem;
  }
}
</style>
