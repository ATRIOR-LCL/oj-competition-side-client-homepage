<script setup>
import { onMounted } from "vue";

onMounted(() => {
  const whiteRoute = document.querySelector(".w-d");
  let whiteRouteHide = false;
  const WhiteHeaderAndText = [...document.querySelectorAll(".w-h")];
  const WhiteRouteIndicators = document.querySelectorAll(
    ".white-route-indicator"
  );
  const whiteRouteDescription = document.querySelector(
    ".white-route-description"
  );
  whiteRoute.addEventListener("click", () => {
    if (whiteRouteHide) {
      WhiteHeaderAndText.forEach((item) => {
        item.classList.remove("header-text-fade");
      });
      WhiteRouteIndicators.forEach((item) => {
        item.classList.remove("white-route-indicator-fade");
      });
      setTimeout(() => {
        whiteRouteDescription.classList.remove("white-route-description-show");
      }, 100);
      whiteRouteHide = false;
    } else {
      whiteRouteDescription.classList.add("white-route-description-show");

      setTimeout(() => {
        WhiteHeaderAndText.forEach((item) => {
          item.classList.add("header-text-fade");
        });
        WhiteRouteIndicators.forEach((item) => {
          item.classList.add("white-route-indicator-fade");
        });
      }, 0);
      whiteRouteHide = true;
    }
  });
  const RedirectRoute = document.querySelector(".redirect-route");
  let redirectRouteHide = false;
  const ReidrectHeaderAndText = [...document.querySelectorAll(".r-h")];
  const RedirectFooter = document.querySelector(".r-f");
  const RedirectRouteDescription = document.querySelector(
    ".redirect-route-description"
  );
  RedirectRoute.addEventListener("click", () => {
    if (redirectRouteHide) {
      ReidrectHeaderAndText.forEach((item) => {
        item.classList.remove("header-text-fade");
      });
      RedirectFooter.classList.remove("white-route-indicator-fade");
      setTimeout(() => {
        RedirectRouteDescription.classList.remove(
          "white-route-description-show"
        );
      }, 100);
      redirectRouteHide = false;
    } else {
      setTimeout(() => {
        ReidrectHeaderAndText.forEach((item) => {
          item.classList.add("header-text-fade");
        });
        RedirectFooter.classList.add("white-route-indicator-fade");
      }, 100);
      RedirectRouteDescription.classList.add("white-route-description-show");
      redirectRouteHide = true;
    }
  });
  const titleObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add("fc-header-show");
      } else {
        entry.target.classList.remove("fc-header-show");
      }
    });
  });
  const title = document.querySelector(".fc-header");
  if (title) titleObserver.observe(title);
});
</script>

<template>
  <div class="fc-container">
    <header class="fc-header">为您提供更加
      <div class="fc-header-focus">
        专注
    <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#1f1f1f"><path d="M440-42v-80q-125-14-214.5-103.5T122-440H42v-80h80q14-125 103.5-214.5T440-838v-80h80v80q125 14 214.5 103.5T838-520h80v80h-80q-14 125-103.5 214.5T520-122v80h-80Zm40-158q116 0 198-82t82-198q0-116-82-198t-198-82q-116 0-198 82t-82 198q0 116 82 198t198 82Zm0-120q-66 0-113-47t-47-113q0-66 47-113t113-47q66 0 113 47t47 113q0 66-47 113t-113 47Zm0-80q33 0 56.5-23.5T560-480q0-33-23.5-56.5T480-560q-33 0-56.5 23.5T400-480q0 33 23.5 56.5T480-400Zm0-80Z"/></svg>

      </div>
      的比赛环境</header>
    <div class="fc-content">
      <div class="fc-content-desc w-d" style="color: #fff;">
        <header class="fc-content-desc-header w-h">路由白名单拦截</header>
        <div class="fc-content-desc-text w-h">
          <p>
            我们采取白名单拦截机制，让您更加专注于比赛内容，避免被无关页面干扰。
          </p>
        </div>
        <div class="fc-content-desc-footer white-route w-c">
          <div class="white-route-container">
            <div class="white-route-indicator">✅ https://www.example.com</div>
            <div class="white-route-indicator">✅ https://www.example.com</div>
            <div class="white-route-indicator white-route-indicator-large">
              <p>🚫 https://www.example.com</p>
              <img src="../../assets/images/cursor.png" alt="" />
            </div>
          </div>
        </div>
        <div class="white-route-description">
          <header class="white-route-description-header">
            <div class="white-route-description-header-title">
              比赛模式已开启，请专心答题！
            </div>
            <div class="white-route-description-header-subtitle">
              <div class="white-route-description-header-subtitle-text">
                访问限制：https://www.example.com
              </div>
              <div class="white-route-description-header-subtitle-text">
                限制说明：该域名不在允许访问范围
              </div>
            </div>
            <div class="white-route-description-header-divider">
              专注比赛才能发挥更佳水平！
            </div>
            <div class="white-route-description-header-button">
              了解，继续答题！
            </div>
          </header>
          <main class="white-route-description-main">
            <p>
              当用户点击了非白名单链接时，将会触发弹窗拦截机制，用户可以简单地点击确认按钮来关闭弹窗，避免被无关页面干扰。
            </p>
          </main>
        </div>
      </div>
      <div class="fc-content-desc redirect-route" style="color: #000;">
        <header class="fc-content-desc-header r-h" >重定向拦截</header>
        <div class="fc-content-desc-text r-h">
          <p>拦截非法的重定向跳转，但您仍可以在合法页面尽情探索！</p>
        </div>
        <div class="fc-content-desc-footer redirect-route r-f">
          <div class="redirect-route-container">
            <div class="redirect-route-container-indicator">
              🚫 https://www.example.com
            </div>
            <div class="redirect-route-container-indicator">
              ✅ https://www.example.com
            </div>
            <img
              src="../../assets/images/redirect.png"
              class="redirect-route-container-icon"
              alt=""
            />
          </div>
        </div>
        <div class="redirect-route-description">
          <header class="redirect-route-description-header">
            <div class="redirect-route-description-header-title">
              <div class="red"></div>
              <div class="yellow"></div>
              <div class="green"></div>
            </div>
            <div class="redirect-route-description-header-content">
              <main class="redirect-route-description-header-content-main">
                <header
                  class="redirect-route-description-header-content-main-title"
                >
                  页面重定向被拦截，保护你的比赛环境！
                </header>
                <div
                  class="redirect-route-description-header-content-main-subtitle"
                >
                  <p>
                    检测到页面发生重定向，目标网站：oj.sdutacm.cn 已被系统拦截！
                  </p>
                  <p>原因：非法重定向拦截，已自动回退主页。</p>
                </div>
                <aside
                  class="redirect-route-description-header-content-main-aside"
                >
                  <p>比赛期间请勿访问非指定网站，祝你取得好成绩！</p>
                </aside>
                <div
                  class="redirect-route-description-header-content-main-button"
                >
                  <p>收到，加油比赛！</p>
                </div>
              </main>
            </div>
          </header>
          <main class="redirect-route-description-main">
            <p>
              当选手不小心触发了重定向的网页后，不用担心，我们仍然会为你拦截非法的重定向跳转，但是会保留跳转之前的合法连接，您可以尽情浏览任何在白名单内的网页！
            </p>
          </main>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="less">
@import url(../../assets/css/inline-style.less);

.redirect-route-description {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  transform: translateY(5rem);
  opacity: 0;
  transition: all 0.5s ease;
  &-header {
    width: 60%;
    height: 50%;
    border-radius: var(--border-radius);
    display: flex;
    flex-direction: column;
    justify-content: start;
    align-items: center;
    position: relative;
    background-color: var(--bg-primary-color);

    &::after {
      content: "😲";
      position: absolute;
      right: 0;
      top: 0;
      font-size: 3rem;
      transform: translateX(50%);
    }
    &-title {
      width: 100%;
      height: 10%;
      display: flex;
      justify-content: start;
      align-items: center;
      gap: 0.2rem;
      padding-left: 0.5rem;
      border-top-left-radius: var(--border-radius);
      border-top-right-radius: var(--border-radius);
      & div {
        height: 40%;
        aspect-ratio: 1;
        border-radius: 50%;
        &.red {
          background-color: #ff5f57;
        }
        &.yellow {
          background-color: #f3bc2e;
        }
        &.green {
          background-color: #28c840;
        }
      }
    }

    &-content {
      width: 100%;
      height: 90%;
      display: flex;
      justify-content: center;
      align-items: center;

      &-main {
        width: 80%;
        aspect-ratio: 4/3;
        color: var(--text-color);
        border-radius: var(--border-radius);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        &-title {
          width: 100%;
          height: 20%;
          display: flex;
          justify-content: start;
          align-items: center;
          font-size: var(--text-medium-size);
        }

        &-subtitle {
          width: 100%;
          height: 40%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          & p {
            width: 100%;
            height: fit-content;
            display: flex;
            justify-content: start;
            font-size: var(--text-small-size);
          }
        }

        &-aside {
          width: 100%;
          height: 20%;
          display: flex;
          justify-content: center;
          align-items: center;
          font-size: var(--text-small-size);
        }

        &-button {
          width: 70%;
          height: 20%;
          display: flex;
          justify-content: center;
          align-items: center;
          background-color: var(--blue-font-color);
          color: #fff;
          font-size: var(--text-small-size);
          border-radius: 3rem;
          margin-top: 1rem;
        }
      }
    }
  }

  &-main {
    width: 60%;
    height: fit-content;
    line-height: 1.5;
    color: #000;
    font-size: var(--text-medium-size);
  }
}

.redirect-route-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  color: #000;

  &-icon {
    position: absolute;
    width: 20%;
    left: 10%;
    top: 5%;
    transition: transform 0.5s ease;
    transform-origin: center center;
  }

  &-indicator {
    width: 60%;
    height: 25%;
    position: absolute;
    border-radius: var(--border-radius);
    display: flex;
    justify-content: center;
    align-items: center;
    color: #000;
    &:nth-child(1) {
      background-color: white;
      transform: translate(-10%, 20%) scale(1);
      z-index: 1;
      box-shadow: var(--box-shadow);
      transition: all 0.5s ease;
      &::after {
        content: "😵‍💫";
        position: absolute;
        right: 0;
        bottom: 0;
        font-size: 3rem;
        transform: translate(20%, 50%) rotate(10deg);
      }
    }
    &:nth-child(2) {
      background-color: rgba(227, 227, 227, 0.837);

      transform: translate(10%, -50%);
    }
  }
}

.white-route-description {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: var(--text-color);
  font-size: var(--text-medium-size);
  left: 0;
  top: 0;
  gap: 2rem;
  opacity: 0;
  transform: translateY(5rem);
  transition: all 0.5s ease;

  &-header {
    width: 65%;
    aspect-ratio: 4/3;
    background-color: var(--blue-bg-color);
    border-radius: var(--border-radius);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
    padding-left: 1rem;
    &-title {
      width: 100%;
      height: 20%;
      display: flex;
      justify-content: start;
      align-items: center;
      font-size: var(--text-medium-size);
      font-weight: 700;
      color: #000;
    }

    &-subtitle {
      width: 100%;
      height: 20%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      &-text {
        width: 100%;
        height: fit-content;
        display: flex;
        justify-content: start;
        color: #000;
        font-size: var(--text-small-size);
      }
    }

    &-divider {
      width: 100%;
      height: 10%;
      display: flex;
      justify-content: start;
      align-items: center;
      font-size: var(--text-small-size);
      color: #000;
    }

    &-button {
      width: 70%;
      height: 10%;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: var(--blue-font-color);
      color: #fff;
      font-size: var(--text-small-size);
      border-radius: 3rem;
      margin-top: 2rem;
    }
  }

  &-main {
    width: 65%;
    height: fit-content;
    font-size: var(--text-medium-size);
    line-height: 1.5;
  }
}

.white-route {
  position: relative;
  color: #000;
  -webkit-user-drag: none;

  &-container {
    width: 100%;
    position: absolute;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: start;
    align-items: start;
    padding-left: 3rem;
    gap: 1rem;
  }

  &-indicator {
    width: 55%;
    aspect-ratio: 4/1;
    background-color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: var(--border-radius);
    transition: all 0.5s ease;
    &:nth-child(1) {
      transition-delay: 0.2s;
    }
    &:nth-child(2) {
      transition-delay: 0.1s;
    }
    &-large {
      width: 60%;
      aspect-ratio: 3/1;
      position: absolute;
      top: 45%;
      left: 30%;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);

      & img {
        display: block;
        position: absolute;
        right: 0;
        bottom: 0;
        width: 20%;
        transform: translate(20%, 50%) scale(1);
        transform-origin: center center;
        transition: transform 0.5s ease;
      }

      &::before {
        content: "🤫";
        position: absolute;
        left: 0;
        bottom: 0;
        display: block;
        width: fit-content;
        height: fit-content;
        font-size: 3rem;
        transform: translate(-40%, 30%) rotate(-10deg);
        transform-origin: center center;
      }
    }
  }
}
.header-text-fade {
  opacity: 0;
  transform: translateY(-5rem);
}

.white-route-indicator-fade {
  opacity: 0;
  transform: translateY(10rem);
}

.white-route-description-show {
  opacity: 1 !important;
  transform: translateY(0) !important;
}

.fc-container {
  width: 100%;
  height: 70vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--bg-primary-color);
}

.fc-header {
  width: 100%;
  height: 10%;
  font-size: 3rem;
  font-weight: 700;
  display: flex;
  color: var(--text-color);
  justify-content: center;
  align-items: center;
  transform: translateY(2rem);
  opacity: 0;
  transition: all 0.5s ease;

  &-focus {
    .inline-style(
      var(--blue-bg-color),
      var(--blue-font-color)
    );
  }
}

.fc-header-show {
  transform: translateY(0) !important;
  opacity: 1 !important;
}

.fc-content {
  width: 80%;
  height: 90%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;

  &-desc {
    height: 90%;
    aspect-ratio: 1;
    padding: 1rem;
    user-select: none;
    cursor: pointer;
    overflow: hidden;
    position: relative;

    &:nth-child(1) {
      color: var(--bg-primary-color);
      background-color: var(--blue-font-color);

      &:hover {
        .white-route-indicator-large img {
          transform: translate(20%, 50%) scale(1.2);
        }
      }
    }

    &:nth-child(2) {
      color: var(--text-color);
      background-color: var(--blue-bg-color);

      &:hover {
        .redirect-route-container-icon {
          transform: rotate(-10deg) scale(1.1);
        }

        .redirect-route-container {
          &-indicator:nth-child(1) {
            transform: scale(1.1) translate(-10%, 20%);
            // background-color: var(--blue-bg-color);
          }
        }
      }
    }
    border-radius: var(--border-radius);

    &-header {
      font-size: var(--text-medium-size);
      width: 100%;
      height: 10%;
      display: flex;
      justify-content: start;
      align-items: center;
      transition: all 0.5s ease;
    }

    &-text {
      font-size: var(--text-large-size);
      width: 100%;
      height: 30%;
      line-height: 1.5;
      font-weight: 700;
      transition: all 0.5s ease;
      transition-delay: 0.1s;
    }

    &-footer {
      width: 100%;
      height: 60%;
      display: flex;
      transition: all 0.5s ease;
      // background-color: red;
    }
  }
}
</style>
