<template>
  <!-- <i @click="drawervisible = true" class="fa-solid fa-bars" style="color: white; font-size: 22px;"></i> -->
  <div class="relative">
    <!-- ปุ่มเปิด Drawer -->
    <button
      @click="drawervisible = true"
      class="w-10 h-10 bg-yellow-200 flex items-center justify-center rounded-full shadow-lg hover:bg-yellow-500 transition"
    >
      <i class="fa-solid fa-bars text-2xl text-black"></i>
    </button>

    <!-- Drawer Menu -->
    <Drawer v-model:visible="drawervisible" :dismissable="true" class="w-64 bg-gray-900 text-white">
      <!-- Header -->
      <template #header>
        <div class="flex items-center justify-between px-4 py-3 bg-yellow-400 text-black w-full" >
          <span class="text-lg font-semibold">เมนูหลัก</span>
        </div>
      </template>

      <!-- รายการเมนู -->
      <ul class="menu-list space-y-3 p-4">
        <li v-for="(item, index) in menuItems" :key="index">
          <button
            class="flex items-center space-x-3 w-full py-2 px-4 text-lg rounded-lg hover:bg-yellow-400 hover:text-black transition"
          >
            <i :class="item.icon" class="text-xl"></i>
            <span>{{ item.name }}</span>
          </button>
        </li>
      </ul>
    </Drawer>
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import { useI18n } from 'vue-i18n';

const { t } = useI18n();
const role_id = localStorage.getItem("role_id");

const baseMenu = ref([
  {
    id: 0,
    label: t('หน้าแรก'),
    icon: "",
    path: "/"
  }
]);

const logoutMenu = {
  label: t('ออกจากระบบ'),
  icon: "",
  path: "/auth/login"
};

const listMenu = ref([]);

onMounted(() => {
  conditionAppendMenu();
});

const conditionAppendMenu = async () => {
  try {
    let dynamicMenu = [...baseMenu.value];

    if (role_id == 2) {
      dynamicMenu.push({
        label: t('ธุรกิจของฉัน'),
        icon: "",
        path: "/vendor/my-business"
      });
    } else if (role_id == 3) {
      dynamicMenu.push({
        label: t('ตรวจสอบข้อมูล'),
        icon: "",
        path: "/inspector/home"
      });
    }

    // รวมเมนูทั้งหมด และให้ "ออกจากระบบ" อยู่ล่างสุดเสมอ
    listMenu.value = [...dynamicMenu, logoutMenu];

  } catch (error) {
    console.error(error);
  }
};

const drawervisible = ref(false);

const menuItems = ref([
  { name: "หน้าแรก", icon: "fa-solid fa-house" },
  { name: "สินค้า", icon: "fa-solid fa-box-open" },
  { name: "ตะกร้า", icon: "fa-solid fa-shopping-cart" },
  { name: "บัญชีของฉัน", icon: "fa-solid fa-user" },
  { name: "ออกจากระบบ", icon: "fa-solid fa-right-from-bracket" },
]);
</script>


<style scoped>
.menu-item i {
  color: #281c74;
}

.menu-item:hover i {
  color: #7d6be3;
}
</style>
