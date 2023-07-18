# 🧃 Composables + Vuex

หากต้องการใช้ Composables ร่วมกับ Vuex store ใน Vue 3 สามารถทำได้ดังนี้:

1. สร้างไฟล์ `composables/useFetch.js` เพื่อใช้ในการจัดการการร้องขอข้อมูล:

```javascript
import { ref } from 'vue';

export function useFetch() {
  const fetchData = async (url) => {
    try {
      const response = await fetch(url);
      const data = await response.json();
      return data;
    } catch (error) {
      console.error(error);
    }
  };

  return {
    fetchData,
  };
}
```

2. สร้างไฟล์ `store/index.js` สำหรับ Vuex store โดยประกาศ state และ mutations ที่ใช้ในการจัดการข้อมูล:

```javascript
import { createStore } from 'vuex';

export default createStore({
  state: {
    posts: [],
  },
  mutations: {
    SET_POSTS(state, posts) {
      state.posts = posts;
    },
  },
});
```

3. แก้ไขไฟล์ `composables/useFetch.js` เพื่อนำ Vuex store เข้ามาใช้งาน:

```javascript
import { ref } from 'vue';
import { useStore } from 'vuex';

export function useFetch() {
  const store = useStore();

  const fetchData = async (url) => {
    try {
      const response = await fetch(url);
      const data = await response.json();
      store.commit('SET_POSTS', data); // เรียกใช้ mutation เพื่ออัปเดต state ใน Vuex store
      return data;
    } catch (error) {
      console.error(error);
    }
  };

  return {
    fetchData,
  };
}
```

4. ในคอมโพเนนต์หรือโมดูลที่ต้องการใช้ Composables ร่วมกับ Vuex store เพิ่ม `useFetch` และ import store เข้ามา:

```vue
<template>
  <div>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
    <button @click="fetchPosts">Fetch Posts</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import { useFetch } from './composables/useFetch';
import { useStore } from 'vuex';

export default {
  setup() {
    const posts = ref([]);
    const store = useStore();

    const { fetchData } = useFetch();

    const fetchPosts = async () => {
      const response = await fetchData('https://api.example.com/posts');
      posts.value = response.data;
    };

    return {
      posts,
      fetchPosts,
    };
  },
};
</script>
```

ในการใช้งานนี้ เรา import `useStore` จาก Vuex และสร้างตัวแปร `store` เพื่อใช้ใน Composable `useFetch` ในการเรียกใช้ mutation เพื่ออัปเดต state ใน Vuex store

ใน `fetchPosts` เราเรียกใช้ `fetchData` และเมื่อได้รับข้อมูลกลับมา เรากำหนดค่า `posts` ใน state ของคอมโพเนนต์

ดังนั้น เราสามารถใช้ Composables ร่วมกับ Vuex store ใน Vue 3 เพื่อจัดการสถานะและการร้องขอข้อมูลได้อย่างมีประสิทธิภาพ
