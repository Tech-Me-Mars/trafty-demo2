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
                <button
                    class="w-10 h-10 bg-yellow-200 flex items-center justify-center rounded-full shadow-md hover:bg-yellow-300 transition">
                    <i class="fa-solid fa-bars text-2xl text-gray-800"></i>
                </button>
            </div>
        </div>

        <!-- รายการสถานที่ -->
        <div class=" p-6">
            <div class="card">
                <div class="bg-white border rounded-lg shadow-sm p-3 flex space-x-3">
                    <!-- รูปภาพ -->
                    <img src="https://via.placeholder.com/80" alt="สถานที่" class="w-16 h-16 rounded-md object-cover">

                    <!-- รายละเอียด -->
                    <div class="flex-1">
                        <h2 class="text-black font-semibold">หนึ่ง นม นัว</h2>
                        <p class="text-gray-500 text-sm">ร้านอาหาร</p>

                        <!-- ป้ายสถานะ -->
                        <div class="mt-2">
                            <p
                                class="bg-red-100 text-red-600 text-xs font-semibold px-2 py-1 rounded-md flex items-center space-x-1 inline-flex ">
                                <i class="fa-solid fa-circle-exclamation text-xs"></i>
                                <span>รออนุมัติ</span>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- รายการของฉัน -->
        <div class="bg-yellow-100 p-4">
            <h2 class="text-black font-semibold">รายการของฉัน (0)</h2>
        </div>

        <!-- กล่องว่าง -->
        <div class="flex flex-col items-center justify-center h-full bg-white flex-grow">
            <img src="/image/no-data.png" alt="ไม่มีข้อมูล" class="w-16 h-16">
            <p class="text-gray-500 mt-2">ยังไม่มีข้อมูล</p>
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

</template>

<script setup>
const showAddMenu = ref(false)

</script>