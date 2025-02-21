<template>
    <div class="bg-zinc-100 min-h-screen">
        <van-nav-bar :title="'ตรวจสอบ'" left-arrow @click-left="navigateTo('/')">
            <template #left>
                <back-page />
            </template>
        </van-nav-bar>

        <section class="p-4 bg-gray-50 ">
            <!-- รูปภาพร้านค้า -->
            <div class="relative">
                <img :src="resInfo?.image_profile" alt="รูปภาพร้านค้า"
                    class="w-full h-64 object-cover rounded-lg" />
                <!-- Indicator -->
                <div class="absolute bottom-2 left-1/2 transform -translate-x-1/2 flex space-x-2">
                    <span class="w-8 h-1 bg-yellow-500 rounded"></span>
                    <span class="w-3 h-1 bg-gray-400 rounded"></span>
                    <span class="w-3 h-1 bg-gray-400 rounded"></span>
                </div>
            </div>

            <!-- รายละเอียดร้านค้า -->
            <div class="bg-white p-4 rounded-lg shadow-md  relative">
                <!-- ชื่อร้าน + สถานะ -->
                <div class="flex justify-between items-center">
                    <h2 class="text-xl font-bold">{{ resInfo?.shop_name }}</h2>
                    <span v-if="resInfo?.status == false" class="bg-red-500 text-white text-sm px-3 py-1 rounded-full flex items-center gap-1">
                        <i class="fa-solid fa-exclamation-circle"></i> รอตรวจสอบ
                    </span>
                    <span v-if="resInfo?.status == true" class="bg-green-500 text-white text-sm px-3 py-1 rounded-full flex items-center gap-1">
                        <i class="fa-solid fa-exclamation-circle"></i> ตรวจสอบแล้ว
                    </span>
                </div>

                <!-- ที่อยู่ -->
                <p class="text-gray-600 text-sm flex items-center mt-1">
                    <i class="fa-solid fa-map-marker-alt text-yellow-500 mr-2"></i>
                    {{ resInfo?.shop_address }}
                </p>

                <!-- เบอร์โทร -->
                <div class="bg-yellow-100 p-3 mt-3 rounded-lg flex items-center">
                    <i class="fa-solid fa-phone text-yellow-500 mr-3"></i>
                    <span class="text-gray-800 font-semibold">{{ resInfo?.shop_phone }}</span>
                </div>

                <!-- รายละเอียดร้านค้า -->
                <div class="mt-4">
                    <h3 class="text-lg font-bold">ข้อมูลร้านค้า</h3>
                    <p class="text-gray-600 text-sm mt-1">
                        {{ resInfo?.shop_details }}
                    </p>
                </div>

                <!-- Accordion -->
                <div class="mt-6">
                    <!-- <div class="border-b py-3 flex justify-between items-center cursor-pointer">
                        <span class="text-gray-800 font-bold">รายการอาหาร</span>
                        <i class="fa-solid fa-chevron-down text-yellow-500"></i>
                    </div> -->
                    <div class="mt-6">
                        <van-collapse v-model="business_items_active" accordion class="mt-4">
        <van-collapse-item title="📖 รายการอาหาร" name="menu">
            <div>
                {{   }}
            </div>
        </van-collapse-item>
      </van-collapse>
      </div>
                    <div class="border-b py-3 flex justify-between items-center cursor-pointer">
                        <span class="text-gray-800 font-bold">ข้อมูลมาตรฐานความปลอดภัย</span>
                        <i class="fa-solid fa-chevron-down text-yellow-500"></i>
                    </div>
                </div>

                <!-- ปุ่มประเมิน -->
                <button
                    class="w-full mt-6 bg-yellow-400 text-black font-semibold py-3 rounded-lg shadow-md hover:bg-yellow-500 transition">
                    ประเมิน
                </button>
            </div>
        </section>


    </div>
</template>
<style scoped>
.van-nav-bar {
    --van-nav-bar-background: #ffc83A;
    --van-nav-bar-text-color: black;
    --van-nav-bar-icon-color: black;
    --van-nav-bar-title-text-color: black;
    --van-nav-bar-height: 70px
}
</style>
<script setup>
// definePageMeta({
//     middleware: ["auth"],
// });
import * as dataApi from './api/data.js'
import widgetSocial from './widgets/widget-social.vue';
import widgetItemsBusiness from './widgets/widget-items-business.vue';
import widgetReview from './widgets/widget-review.vue';
import widgetPolicy from './widgets/widget-policy.vue';
const router = useRouter();
const route = useRoute();
const isloadingAxi = useState("isloadingAxi");
import { useI18n } from 'vue-i18n';
const { t } = useI18n();
const localPath = useLocalePath();

const business_items_active =ref(false)

const showShare = ref(false);
const options = [
    {
        name: 'Facebook',
        icon: '/image/social/facebook.png',
    },
    {
        name: 'Line',
        icon: '/image/social/line.png',
    },
    {
        name: 'Discord',
        icon: '/image/social/discord.png',
    },
    {
        name: 'Twitter',
        icon: '/image/social/twitter.png',
    },
];

const resProfile = ref({ role_id: null })
const loadProfile = async () => {
    try {
        const res = await dataApi.getProfile();
        resProfile.value = res.data.data;
    } catch (error) {
        console.error(error)
    }
}
// Handle selection
const onSelect = (option) => {
    console.log(`Selected: ${option.name}`);
    // Add share logic here, such as opening links or triggering actions
};
const token = localStorage.getItem("token");
onMounted(() => {
    if (token) {
        loadProfile();
    }
    loadDataInfo()
})

const resInfo = ref();
const loadDataInfo = async () => {
    try {
        const res = await dataApi.getBusinessById(route.params.id);
        resInfo.value = res.data.data;

        console.log(resInfo.value)


    } catch (error) {
        console.error(error)
    }
}

const isLiked = ref(false);

const toggleLike = () => {
    isLiked.value = !isLiked.value;
};
</script>
