# Composables

Composables คือเทคนิคหรือแนวคิดในการเขียนโค้ดใน Vue 3 ซึ่งช่วยให้นักพัฒนาสามารถแบ่งโค้ดออกเป็นส่วนย่อยที่ใช้ซ้ำได้และสามารถนำมาใช้ร่วมกันได้ในโครงสร้างที่กระชับและมีประสิทธิภาพสูงขึ้น

ใน Vue 3, Composables ใช้งานร่วมกับ Composition API ซึ่งเป็นส่วนหนึ่งของ Vue 3 ที่ช่วยให้นักพัฒนาสามารถจัดการและเรียกใช้งานโค้ดได้อย่างเป็นระเบียบและยืดหยุ่นมากขึ้น

Composables มีลักษณะเป็นฟังก์ชันที่ใช้ในการแยกและส่งคืนตัวแปรหรือฟังก์ชันที่ใช้งานร่วมกันในโค้ด ซึ่งสามารถนำไปใช้ในคอมโพเนนต์หรือโมดูลอื่น ๆ ในโปรเจ็กต์ Vue 3

Composables ช่วยให้นักพัฒนาสามารถเขียนโค้ดให้สม่ำเสมอและเป็นระเบียบมากขึ้น และช่วยให้โค้ดสามารถนำไปใช้ซ้ำได้ในหลายส่วนของแอปพลิเคชัน นอกจากนี้ยังช่วยให้การทดสอบและการบำรุงรักษาโค้ดเป็นเรื่องง่ายและเข้าใจได้ง่ายขึ้นด้วย

ตัวอย่างของ Composables ที่พบบ่อยในโปรเจ็กต์ Vue 3 ได้แก่:

* **useFetch**: ใช้สำหรับการจัดการการร้องขอข้อมูลจากแหล่งข้อมูลภายนอก
* **usePagination**: ใช้สำหรับการจัดการรายการแบ่งหน้า
* **useLocalStorage**: ใช้สำหรับการจัดการข้อมูลใน localStorage
* **useFormValidation**: ใช้สำหรับการตรวจสอบความถูกต้องของฟอร์ม
* **useAuthentication**: ใช้สำหรับการจัดการระบบการยืนยันตัวตน

Composables ช่วยให้นักพัฒนาสามารถแบ่งแยกโค้ดในรูปแบบที่สม่ำเสมอและใช้ซ้ำได้ นอกจากนี้ยังช่วยให้โค้ดมีความยืดหยุ่นและเป็นระเบียบมากขึ้น ซึ่งเป็นข้อได้เปรียบสำหรับการพัฒนาแอปพลิเคชัน Vue 3 ที่ใหญ่และซับซ้อน

### ตัวอย่าง Component&#x20;

```javascript
// component/post.vue
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

export default {
  setup() {
    const posts = ref([]);

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

ในตัวอย่างนี้เรามีคอมโพเนนต์ที่ใช้ Composables เพื่อจัดการการร้องขอข้อมูล (`useFetch`) โดยใช้ Composition API (`setup()` function) ในการกำหนดตัวแปรและฟังก์ชันที่ใช้งานร่วมกัน

ใน `<template>` เราแสดงรายการโพสต์โดยใช้ `v-for` และมีปุ่ม "Fetch Posts" ที่เมื่อคลิกจะเรียกใช้ `fetchPosts()` เพื่อเรียกข้อมูลโพสต์จากแหล่งข้อมูลภายนอก

ใน `setup()` เราใช้ `useFetch` เพื่อเรียกใช้ Composable ที่ชื่อ `useFetch` ซึ่งจะส่งคืนฟังก์ชัน `fetchData` ที่ใช้ในการร้องขอข้อมูลจาก URL ที่กำหนด

ฟังก์ชัน `fetchPosts` จะใช้ `fetchData` จาก Composable เพื่อเรียกข้อมูลโพสต์และกำหนดค่าให้กับตัวแปร `posts`

โดยใช้ Composables นี้เราสามารถใช้งานแยกโค้ดออกมาเป็นส่วนย่อยที่สามารถนำไปใช้งานร่วมกันในโครงสร้าง Vue 3 ได้อย่างง่ายดาย นอกจากนี้ยังช่วยให้โค้ดเป็นระเบียบและสามารถทดสอบและบำรุงรักษาได้ง่ายขึ้น

***

### ตัวอย่าง Composables

```javascript
// composables/useFetch.js
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

ในไฟล์ `useFetch.js` เราสร้างฟังก์ชัน `useFetch` ซึ่งจะเป็น Composable ที่ใช้ในการจัดการการร้องขอข้อมูลจากแหล่งข้อมูลภายนอก

ฟังก์ชัน `useFetch` มีฟังก์ชัน `fetchData` ที่ใช้ในการร้องขอข้อมูลจาก URL ที่กำหนด โดยใช้ `fetch` ในการส่งคำขอและรับข้อมูลกลับมา จากนั้นแปลงข้อมูลที่ได้รับเป็น JSON และส่งคืนข้อมูล

ในกรณีที่เกิดข้อผิดพลาดระหว่างการร้องขอ (เช่นเครือข่ายหรือรูปแบบข้อมูลไม่ถูกต้อง) ฟังก์ชัน `fetchData` จะแสดงข้อผิดพลาดในคอนโซล

ที่สุดท้าย เราส่งคืนฟังก์ชัน `fetchData` ในวัตถุที่ส่งคืนจาก Composable เพื่อให้สามารถเรียกใช้งานได้จากคอมโพเนนต์หรือโมดูลอื่นในแอปพลิเคชัน Vue 3



> แนะนำ การใช้งาน Composables ร่วมกับ **Vuex** หรือ **Pinia**
>
> [composables-+-vuex.md](composables-+-vuex.md "mention")
>
> [composables-+-pinia.md](composables-+-pinia.md "mention")
