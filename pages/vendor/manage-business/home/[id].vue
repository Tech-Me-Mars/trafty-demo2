<template>
    <div class="min-h-screen flex flex-col" style="background-color: #ffc83A;">
        <!-- Header -->
        <div class="flex justify-between items-center px-4 py-4 bg-yellow-400">
            <h1 class="text-lg font-bold text-black">ข้อมูลแหล่งท่องเที่ยวของคุณ</h1>
            <div class="flex items-center space-x-4">
                <!-- ปุ่มแจ้งเตือน -->
                <button
                    class="w-10 h-10 bg-yellow-200 flex items-center justify-center rounded-full shadow-md hover:bg-yellow-300 transition">
                    <i class="fa-solid fa-bell text-2xl text-gray-800"></i>
                </button>

                <!-- ปุ่มเมนู -->
                <!-- <button
                    class="w-10 h-10 bg-yellow-200 flex items-center justify-center rounded-full shadow-md hover:bg-yellow-300 transition">
                    <i class="fa-solid fa-bars text-2xl text-gray-800"></i>
                </button> -->
                <drawer-menu></drawer-menu>
            </div>
        </div>

        <!-- รายการสถานที่ -->
        <div class=" p-6">
            <div class="card rounded-lg">
                <div class="flex space-x-3">
                    <!-- รูปภาพ -->
                    <img :src="resShop?.image_profile" alt="สถานที่" class="w-16 h-16 rounded-md object-cover">

                    <!-- รายละเอียด -->
                    <div class="flex-1">
                        <h2 class="text-black font-semibold">{{ resShop?.shop_name }}</h2>
                        <p class="text-gray-500 text-sm">{{ resShop?.business_type_name }}</p>

                        <!-- ป้ายสถานะ -->
                        <div class="mt-2">
                            <p
                                class="bg-red-100 text-red-600 text-xs font-semibold px-2 py-1 rounded-md flex items-center space-x-1 inline-flex ">
                                <i class="fa-solid fa-circle-exclamation text-xs"></i>
                                <span v-if="resShop?.status == true">รออนุมัติ</span>
                                <span v-else>ตรวจสอบแล้ว</span>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- รายการของฉัน -->
        <div class="bg-yellow-100 p-4">
            <h2 class="text-black font-semibold">รายการของฉัน ({{ listItemShop.length }})</h2>
        </div>

        <!-- กล่องว่าง -->
        <div v-if="listItemShop.length > 0"
            class="flex flex-col items-center justify-start h-full bg-white flex-grow p-4">
            <div class="w-full">
                <div v-for="(item, index) in listItemShop" :key="index"
                    class="flex justify-between items-center border-b py-4">
                    <span class="text-lg font-semibold">{{ item.name }}</span>
                    <Button severity="danger" label="ลบรายการ" class="bg-red-600 !text-white px-4 py-2 rounded-md"
                        @click="removeItem(index)" />
                </div>
            </div>

        </div>
        <div v-else class="flex flex-col items-center justify-center h-full bg-white flex-grow p-4">
            <!-- <div class="flex flex-col items-center justify-center mt-10">
                <img src="/image/no-data.png" alt="ไม่มีข้อมูล" class="w-16 h-16" />
                <p class="text-gray-500 mt-2">ยังไม่มีข้อมูล</p>
            </div> -->
            <no-data class="mt-10" />
        </div>


        <!-- ปุ่มเพิ่ม -->
        <button @click="showAddMenu = true"
            class="fixed bottom-6 right-6 bg-black !text-white w-12 h-12 flex items-center justify-center rounded-full shadow-lg hover:scale-105 transition">
            <i class="fa-solid fa-plus text-xl"></i>
        </button>


        <van-popup v-model:show="showAddMenu" round position="bottom" :style="{ height: '50%' }">
            <!-- หัวข้อ และปุ่มปิด -->
            <div class="flex items-center justify-between border-b p-5 ">
                <h2 class="text-lg font-semibold text-black mx-auto">เพิ่มเมนู</h2>
                <button @click="showAddMenu = false"
                    class="w-8 h-8 bg-yellow-400 text-black rounded-full flex items-center justify-center shadow-md hover:bg-yellow-500 transition absolute right-2">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <!-- ฟอร์มกรอกข้อมูล -->
            <div class="mt-4 px-6">
                <label class="text-gray-500 text-sm">ชื่อเมนู</label>
                <input type="text" v-model="menuName"
                    class="w-full border border-yellow-400 focus:border-yellow-500 rounded-md p-2 mt-1 focus:outline-none focus:ring-2 focus:ring-yellow-400"
                    placeholder="ชื่อเมนู" />
            </div>

            <!-- ปุ่มเพิ่มรายการ -->
            <!-- <button
                class="bg-yellow-400 text-black font-semibold py-3 w-full rounded-lg mt-6 hover:bg-yellow-500 transition">
                เพิ่มรายการ
            </button> -->
            <div class="px-6 mt-10">
                <Button :loading="isloadingAxi" :label="'เพิ่มรายการ'" type="submit" class="w-full text-black" />

            </div>
        </van-popup>
    </div>
    <MyToast :data="alertToast" />

</template>

<script setup>
definePageMeta({
    middleware: ["auth"],
});

const alertToast = ref({});

const showAddMenu = ref(false)
const isloadingAxi = useState("isloadingAxi");
import * as dataApi from "./api/data.js";
const route = useRoute();

const resShop = ref()
const loadShop = async () => {
    try {
        const res = await dataApi.getShopById(route.params.id)
        resShop.value = res.data.data;
    } catch (error) {
        console.error(error)
        alertToast.value = {
            title: 'ล้มเหลว',
            isError: true,
            color: "error",
            msg: error.response?.data?.message || "Error occurred",
            dataError: error,
        };
    }
}
onMounted(() => { loadShop() })

const listItemShop = ref([
    { name: "ลาวาโทสต์วานอฟฟี่" },
    { name: "บิงซู" },
    { name: "สเลอปี้ชาเขียว" },
])

const removeItem = (index) => {
  listItemShop.value.splice(index, 1);
};
</script>