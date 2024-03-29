<template>
  <main v-show="show">
    <div :class="['hint', { hidden: !hint }]" ref="hint">
      <Image :src="HintImage" class="image" alt="" />
    </div>

    <div class="phrase" ref="introPhrase">
      <p>"과거에서 일어나는 우리의 설레는 이야기 ..."</p>
    </div>

    <div class="logo" ref="logo">
      <Image :src="LogoImage" class="image" alt="" @loaded="loadingComplete" />
    </div>

    <div class="title" ref="title">
      <h3>한림대학교 대동제</h3>
      <br />
      <p>2023.05.16 ~ 05.18</p>
    </div>
    <div class="menu" ref="menu">
      <RouterLink to="/boothmap">부스 목록</RouterLink>
      <RouterLink to="/timetable">타임 테이블</RouterLink>
      <RouterLink to="/comment">방명록</RouterLink>
      <RouterLink to="/program">프로그램</RouterLink>
      <RouterLink to="/announcement">공지사항</RouterLink>
      <button @click="openBoothRecommendation">부스 유형 추천 테스트 하러가기</button>
    </div>

    <div class="sponsor" ref="sponsor">
      <img src="@/assets/naverlogo.png" alt="" @click="openNaverCloud" />
      <p @click="openNaverCloud">본 웹사이트는 Naver Cloud Platform의 협찬을 받았습니다.</p>
    </div>
  </main>
</template>

<script>
import HintImage from '@/assets/hint.png';
import LogoImage from '@/assets/로고.png';
import Image from '../components/Image.vue';
import { gsap } from 'gsap';
import { RouterLink } from 'vue-router';

export default {
  components: {
    Image
  },
  data() {
    return {
      LogoImage,
      HintImage,
      show: false
    };
  },
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    hint: {
      type: Boolean,
      default: true
    }
  },
  watch: {
    loading() {
      if (this.show) {
        this.showPage();
      }
    }
  },
  methods: {
    openBoothRecommendation() {
      window.open('https://hallym-booth.vercel.app/', '_blank');
    },
    openNaverCloud() {
      window.open('https://www.ncloud.com/', '_blank');
    },
    loadingComplete() {
      this.show = true;
      if (!this.loading) {
        this.showPage();
      }
    },
    showPage() {
      // 슬로건 애니메이션
      gsap.fromTo(
        this.$refs.introPhrase,
        {
          top: '50%',
          transform: 'translateY(-50%)',
          opacity: 0
        },
        {
          delay: 0.2,
          duration: 1,
          transform: 'none',
          opacity: 1,
          ease: 'Expo.easeOut'
        }
      );

      // 포스터 애니메이션
      gsap.fromTo(
        this.$refs.logo,
        {
          opacity: 0,
          transform: 'scale(0.5)'
        },
        {
          delay: 0.6,
          duration: 1.5,
          transform: 'none',
          opacity: 1,
          ease: 'Expo.easeOut'
        }
      );

      // 타이틀 애니메이션
      gsap.fromTo(
        this.$refs.title,
        {
          opacity: 0,
          transform: 'translateY(100%)'
        },
        {
          delay: 1.25,
          duration: 2,
          transform: 'none',
          opacity: 1,
          ease: 'Expo.easeOut'
        }
      );

      // 메뉴 애니메이션
      for (let i = 0; i < 6; i++) {
        gsap.fromTo(
          this.$refs.menu.childNodes[i],
          {
            transform: 'translateX(-7%)',
            opacity: 0,
            pointerEvents: 'none'
          },
          {
            delay: 1.75 + i * 0.2,
            duration: 0.4,
            transform: 'none',
            opacity: 1,
            pointerEvents: 'auto'
          }
        );
      }

      if (this.hint) {
        // 힌트 애니메이션
        gsap.fromTo(
          this.$refs.hint,
          {
            opacity: 0,
            transform: 'translateY(-20%)'
          },
          {
            delay: 3.25,
            duration: 0.5,
            transform: 'none',
            opacity: 1,
            ease: 'Expo.easeOut',
            onComplete: function () {
              gsap.set(this.targets(), { clearProps: 'all' });
            }
          }
        );
      }

      // 스폰서 애니메이션
      gsap.fromTo(
        this.$refs.sponsor,
        {
          opacity: 0,
          pointerEvents: 'none'
        },
        {
          delay: 3.25,
          duration: 1.5,
          opacity: 1,
          pointerEvents: 'auto',
          ease: 'Expo.easeOut'
        }
      );
    }
  },
  mounted() {}
};
</script>
<style scoped>
:deep(.hint > img) {
  width: 240px;
}

.hint {
  transition: opacity 0.25s;
}

.hidden {
  opacity: 0;
}

.logo {
  width: 70%;
  max-width: 320px;
  margin: -18px auto;
  display: flex;
  justify-content: center;
}

:deep(.image) {
  width: 260px;
  max-width: 100%;
}

.phrase {
  margin-top: 32px;
  font-family: 'Nanum Gothic', sans-serif;
  font-style: normal;
  font-size: 12pt;
  text-align: center;
  word-break: keep-all;
  letter-spacing: 4px;
  color: white;
}

.title {
  font-family: 'Nanum Gothic', sans-serif;
  text-align: center;
  color: white;
}

.menu {
  max-width: 320px;
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  margin-top: 24px;
}

.menu > * {
  width: 100%;
  margin: 4px 8px;
  padding: 10px 0;
  border-radius: 64px;
  font-size: 12pt;
  font-weight: 600;
  text-align: center;
  text-decoration: none;
  color: #363636;
  background-color: #ffffffb3;
}

.menu > button {
  color: white;
  background-color: #ca434cb3;
}

.menu > button.popcat {
  background-color: #5c859bb3;
}

.sponsor {
  margin: 64px -28px;
  padding: 12px 0;
  padding-bottom: 48px;
  margin-bottom: 0;
  text-align: center;
  color: white;
  font-size: 9pt;
}

.sponsor > img {
  max-width: 140px;
  margin: 18px 0;
  cursor: pointer;
}

.sponsor > p {
  cursor: pointer;
}

@media screen and (max-width: 400px) {
  .logo {
    margin-top: -28px;
  }

  .phrase {
    font-size: 10pt;
    margin-top: 36px;
  }

  .menu > * {
    width: 100%;
    margin: 4px;
    font-size: 11pt;
  }
}
</style>
