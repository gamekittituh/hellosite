# 🍍 Composables + Pinia

หากต้องการใช้ Composables ร่วมกับ Pinia (เพล็กซ์สถานะสโตร์สำหรับ Vue 3) สามารถทำได้ดังนี้:

1. ติดตั้ง Pinia โดยใช้ npm หรือ yarn:

```bash
npm install pinia
```

หรือ

```bash
yarn add pinia
```

2. สร้างไฟล์ `composables/useFetch.js` เพื่อใช้ในการจัดการการร้องขอข้อมูล:

```javascript
import { defineStore } from 'pinia';

export const useFetchStore = defineStore('fetch', {
  state: () => ({
    posts: [],
  }),
  actions: {
    async fetchData(url) {
      try {
        const response = await fetch(url);
        const data = await response.json();
        this.posts = data;
        return data;
      } catch (error) {
        console.error(error);
      }
    },
  },
});
```

3. ในคอมโพเนนต์หรือโมดูลที่ต้องการใช้ Composables ร่วมกับ Pinia เพิ่ม `useFetchStore`:

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
import { defineComponent } from 'vue';
import { useFetchStore } from './composables/useFetch';

export default defineComponent({
  setup() {
    const fetchStore = useFetchStore();

    const fetchPosts = async () => {
      const response = await fetchStore.fetchData('https://api.example.com/posts');
      // แสดงรายการโพสต์
    };

    return {
      posts: fetchStore.posts,
      fetchPosts,
    };
  },
});
</script>
```

ในตัวอย่างนี้ เราใช้ `defineStore` จาก Pinia เพื่อสร้าง store ที่ชื่อ `fetch` ซึ่งมี state ของ `posts` และ actions ของ `fetchData` ที่ใช้ในการร้องขอข้อมูล

ใน `setup()` เราสร้างตัวแปร `fetchStore` ซึ่งเป็น instance ของ `useFetchStore` และเรียกใช้ `fetchData` เมื่อคลิกปุ่ม "Fetch Posts" เพื่อร้องขอข้อมูล

เราส่งค่า `posts` จาก store มาใช้ใน `<template>` เพื่อแสดงรายการโพสต์

ดังนั้น เราสามารถใช้ Composables ร่วมกับ Pinia สำหรับการจัดการสถานะและการร้องขอข้อมูลใน Vue 3 ได้อย่างสมบูรณ์
