<script setup>
import axios from 'axios';

const paginate = (page) => {
    vm.page = page;
    GetUsers();
};

const vm = reactive({
    listUser: [],
    totalItems: null,
    totalPages: null,
    currentPage: null,
    sortByDate: 'desc',
    limit: 8,
    page: 1,
    loading: true

});
const userPemilih = reactive({
    listUser: [],
    totalItems: null,
    totalPages: null,
    currentPage: null,
    sortByDate: 'desc',
    limit: 8,
    page: 1,
    loading: true
});

const GetUsers = async () => {
    vm.loading = true;
    // return new Promise((resolve, reject) => {
    axios.get(`${import.meta.env.VITE_APP_ENV}/users/?page=${vm.page}&limit=${vm.limit}&sortByCreated=${vm.sortByDate}&role=ADMIN`)
        .then((result) => {
            // resolve(result.data ?? result);
            vm.listUser = result.data.data;
            vm.totalItems = result.data.countPages;
            vm.totalPages = result.data.totalPages;
            vm.currentPage = result.data.currentPage;
            console.log(result.data);
            vm.loading = false;
        }).catch((err) => {
            console.log(err);
            // (err.response.data) ? reject(err.response.data) : reject(err)
        });
    // })
};

const GetUsersByRole = async () => {
    axios.get(`${import.meta.env.VITE_APP_ENV}/users/?page=${vm.page}&limit=${vm.limit}&sortByCreated=${vm.sortByDate}&role=PEMILIH`)
        .then((result) => {
            userPemilih.listUser = result.data.data;
            userPemilih.totalItems = result.data.countPages;
            userPemilih.totalPages = result.data.totalPages;
            userPemilih.currentPage = result.data.currentPage;
        }).catch((err) => {

        });
}

onMounted(() => {
    GetUsers();
    GetUsersByRole();
});
</script>
<template>
    <div class="space-y-4">
        <div class="max-w-full p-4 mx-auto bg-white border border-gray-200 rounded-[10px] shadow-lg">
            <header class="px-5 py-4 border-b border-gray-100">
                <h2 class="font-semibold text-gray-800">Daftar Admin</h2>
            </header>
            <div class="p-3">
                <div class="overflow-x-auto">
                    <table class="w-full table-auto">
                        <thead class="font-semibold text-gray-400 uppercase text-[16px] bg-gray-50">
                            <tr>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Nama</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Username</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Detil</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-center">Aksi</div>
                                </th>
                            </tr>
                        </thead>
                        <tbody v-if="!vm.loading" class="text-[16px] divide-y divide-gray-100">
                            <tr v-for="(item, i) in vm.listUser" :key="`${i}`">
                                <td class="p-2 whitespace-nowrap">
                                    <div class="flex items-center text-center">
                                        <!-- <div class="flex-shrink-0 w-10 h-10 mr-2 sm:mr-3">
                                        <img class="rounded-full"
                                            src="https://raw.githubusercontent.com/cruip/vuejs-admin-dashboard-template/main/src/images/user-36-05.jpg"
                                            width="40" height="40" alt="Alex Shatov">
                                    </div> -->
                                        <div class="font-medium text-gray-800">{{ item.fullName }}</div>
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="text-left">{{ item.username }}</div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left text-green-500">Lihat</div>
                                </td>
                                <td class="p-2 text-center whitespace-nowrap">
                                    <div class="flex justify-center space-x-5 text-lg text-center">
                                        <button>Edit</button>
                                        <button>Hapus</button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div class="flex justify-center mt-5" v-if="vm.listUser.length">
                <vue-awesome-paginate :totalItems="vm.countPages" v-model="vm.currentPage" @click="paginate"
                    :items-per-page="vm.totalPages" :max-pages-shown="3" paginate-buttons-class="btn"
                    active-page-class="btn-active" back-button-class="back-btn" next-button-class="next-btn">
                    <template #prev-button>
                        <span class="flex items-center justify-center">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="black" width="12" height="12"
                                transform="rotate(180)" viewBox="0 0 24 24">
                                <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
                            </svg>
                        </span>
                    </template>
                    <template #next-button>
                        <span class="flex items-center justify-center">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="black" width="12" height="12"
                                viewBox="0 0 24 24">
                                <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
                            </svg>
                        </span>
                    </template></vue-awesome-paginate>
            </div>
        </div>

        <div class="max-w-full p-4 mx-auto bg-white border border-gray-200 rounded-[10px] shadow-lg">
            <header class="px-5 py-4 border-b border-gray-100">
                <h2 class="font-semibold text-gray-800">Daftar Pemilih</h2>
            </header>
            <div class="p-3">
                <div class="overflow-x-auto">
                    <table class="w-full table-auto">
                        <thead class="font-semibold text-gray-400 uppercase text-[16px] bg-gray-50">
                            <tr>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Nama</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Username</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Detil</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-center">Aksi</div>
                                </th>
                            </tr>
                        </thead>
                        <tbody v-if="!vm.loading" class="text-[16px] divide-y divide-gray-100">
                            <tr v-for="(item, i) in userPemilih.listUser" :key="`${i}`">
                                <td class="p-2 whitespace-nowrap">
                                    <div class="flex items-center text-center">
                                        <!-- <div class="flex-shrink-0 w-10 h-10 mr-2 sm:mr-3">
                                        <img class="rounded-full"
                                            src="https://raw.githubusercontent.com/cruip/vuejs-admin-dashboard-template/main/src/images/user-36-05.jpg"
                                            width="40" height="40" alt="Alex Shatov">
                                    </div> -->
                                        <div class="font-medium text-gray-800">{{ item.fullName }}</div>
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="text-left">{{ item.username }}</div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left text-green-500">Lihat</div>
                                </td>
                                <td class="p-2 text-center whitespace-nowrap">
                                    <div class="flex justify-center space-x-5 text-lg text-center">
                                        <button>Edit</button>
                                        <button>Hapus</button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div class="flex justify-center mt-5" v-if="vm.listUser.length">
                <vue-awesome-paginate :totalItems="vm.countPages" v-model="vm.currentPage" @click="paginate"
                    :items-per-page="vm.totalPages" :max-pages-shown="3" paginate-buttons-class="btn"
                    active-page-class="btn-active" back-button-class="back-btn" next-button-class="next-btn">
                    <template #prev-button>
                        <span class="flex items-center justify-center">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="black" width="12" height="12"
                                transform="rotate(180)" viewBox="0 0 24 24">
                                <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
                            </svg>
                        </span>
                    </template>
                    <template #next-button>
                        <span class="flex items-center justify-center">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="black" width="12" height="12"
                                viewBox="0 0 24 24">
                                <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
                            </svg>
                        </span>
                    </template></vue-awesome-paginate>
            </div>
        </div>
    </div>
</template>
<style>
.btn {
    font-weight: bold;
    background-color: #e0e0e0;
    height: 30px;
    width: 30px;
    border: none;
    margin-inline: 5px;
    cursor: pointer;
    border-radius: 30%;
}

.back-btn {
    background-color: #e0e0e0;
    border-radius: 50%;
}

.next-btn {
    background-color: #e0e0e0;
    border-radius: 50%;
}

.btn-active {
    color: white;
}
</style>
