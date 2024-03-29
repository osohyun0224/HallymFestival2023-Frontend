<template>
  <div class="container" :class="{ large: isAdmin }">
    <div class="header">
      <div class="content">
        <div class="image-content">
          <Image class="image" :src="icon" :alt="type" :width="60" :height="60" />
        </div>
        <div class="text-content">
          <h3 v-text="title" />
          <p v-text="`${content}`" />
        </div>
      </div>
      <div class="info">
        <div v-if="mode == 4" class="comment">
          <img :src="Icon.comment" />
          <p>{{ numberToString(comment) }}</p>
        </div>
        <div v-if="isAdmin && mode == 6" class="report">
          <img :src="Icon.report" />
          <p>{{ numberToString(report) }}</p>
        </div>
        <div v-if="mode == 5" class="like">
          <img :src="Icon.heart" />
          <p>{{ numberToString(like) }}</p>
        </div>
      </div>
    </div>
    <div v-if="isAdmin" class="footer">
      <p>작성자: {{ writer }}</p>
      <p>생성시간: {{ dateToString(date) }}</p>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs';
import { Icon } from '../library/icon';
import Image from './Image.vue';

export default {
  components: {
    Image
  },

  data() {
    return {
      Icon
    };
  },
  props: {
    title: {
      type: String,
      default: 'Untitled Announcement'
    },
    content: {
      type: String,
      default: '내용입니다.'
    },
    type: {
      type: String,
      default: '부스'
    },
    like: {
      type: Number,
      default: 0
    },
    comment: {
      type: Number,
      default: 0
    },
    report: {
      type: Number,
      default: 0
    },
    mode: {
      type: Number,
      default: 0
    },
    writer: {
      type: String,
      default: '유저'
    },
    date: {
      type: String,
      default: '2023-01-01 00:00:00'
    },
    isAdmin: {
      type: Boolean,
      default: false
    }
  },

  methods: {
    numberToString(number) {
      // 숫자가 너무 크면 축약해서 표시하기
      return number
        .toLocaleString('ko-KR', {
          notation: number > 9900 ? 'compact' : 'standard',
          maximumFractionDigits: 1
        })
        .toLowerCase();
    },

    dateToString(date) {
      return dayjs(date).format('YYYY.MM.DD HH:mm');
    }
  },

  computed: {
    icon() {
      const typeObject = {
        플리마켓: Icon.basket,
        푸드트럭: Icon.food,
        주점: Icon.interest,
        부스: Icon.store
      };

      return typeObject[this.type];
    }
  }
};
</script>
<style scoped>
.container {
  box-sizing: border-box;
  width: 100%;
  height: 80px;
  padding: 2px 12px;
  border-radius: 4px;
  overflow: hidden;
  background-color: #eef3f6;
  display: flex;
  flex-direction: column;
  justify-content: center;
  transition: transform 0.25s;
}

.container:hover {
  transform: scale(1.07);
}

.container.large {
  height: 100px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.content {
  display: flex;
  justify-content: left;
  align-items: center;
  overflow: hidden;
}
.text-content {
  margin-left: 10px;
  overflow: hidden;
}

.text-content > * {
  margin: 0;
  margin-right: 10px;

  /* 텍스트 오버플로우를 ... 으로 숨기기 */
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.text-content > p {
  margin-top: 4px;
}

.image-content {
  border-radius: 4px;
  min-width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

:deep(.image-content > .image) {
  object-fit: cover;
  width: 32px;
}
.info {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10pt;
}

.info > div {
  width: 42px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.info > div > img {
  width: 18px;
  height: 18px;
  margin-bottom: 8px;
}

.footer {
  margin-top: 2px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  overflow: hidden;
}

.footer > p {
  font-size: 9pt;
  overflow: hidden;
}

.hidden {
  display: none;
}

@media screen and (max-width: 400px) {
  .container {
    height: 70px;
    padding: 2px 8px;
  }

  .container.large {
    height: 76px;
  }

  .image-content {
    min-width: 36px;
    height: 36px;
  }
  :deep(.image-content > .image) {
    object-fit: cover;
    width: 24px;
  }

  .text-content > h3 {
    font-size: 11pt;
  }
  .text-content > p {
    font-size: 9pt;
  }

  .info > div {
    font-size: 8pt;
  }

  .info > div > img {
    width: 16px;
    height: 16px;
  }

  .footer {
    margin-top: 8px;
    flex-direction: column;
    align-items: flex-start;
  }

  .footer > p {
    font-size: 5pt;
  }
}
</style>
