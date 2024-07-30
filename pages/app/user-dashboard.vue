<script setup>
import axios from 'axios';

import DetilComp from '@/components/DetilComp.vue';

const { $day } = useNuxtApp();
const selected = ref('');

const errorNotif = (err) => {
    useNuxtApp().$toast.warn(err);
};
const paginateAdmin = (page) => {
    vm.page = page;
    GetUsers();
};
const paginatePemilih = (page) => {
    userPemilih.page = page;
    GetUsersByRole();
};
const view = (item) => {
    selected.value = item;
    vm.view = true;
    userById(item.id);
};

const deleteUserAdmin = (id) => {
    axios.delete(`${import.meta.env.VITE_APP_ENV}/users/${id}/user-delete`)
        .then((result) => {
            setTimeout(() => {
                window.location.reload();
            }, 1000);
        }).catch((err) => {
            console.log(err);
        });
};
const deleteUser = (id) => {
    axios.delete(`${import.meta.env.VITE_APP_ENV}/users/${id}/user-delete`)
        .then((result) => {
            setTimeout(() => {
                window.location.reload();
            }, 1000);
        }).catch((err) => {
            console.log(err);
        });
}

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
const dialog = reactive({
    data: null,
    loading: true,
});

const userById = () => {
    dialog.loading = true;
    axios.get(`${import.meta.env.VITE_APP_ENV}/users/`)
        .then((res) => {
            dialog.loading = false;
            dialog.data = res.data.data
        }).catch((err) => {
            errorNotif(err.response.data.message)
        })
};
const GetUsers = async () => {
    vm.loading = true;
    // return new Promise((resolve, reject) => {
    axios.get(`${import.meta.env.VITE_APP_ENV}/users/?page=${vm.page}&limit=${vm.limit}&sortByCreated=${vm.sortByDate}&role=ADMIN`)
        .then((result) => {
            // resolve(result.data ?? result);
            vm.loading = false;
            vm.listUser = result.data.data;
            vm.totalItems = result.data.countROLE;
            vm.totalPages = result.data.totalPagesROLE;
            vm.currentPage = result.data.currentPage;
        }).catch((err) => {
            console.log(err);
            vm.loading = false;
            // (err.response.data) ? reject(err.response.data) : reject(err)
        })
    vm.loading = true;
    // })
};

const GetUsersByRole = async () => {
    userPemilih.loading = true;
    axios.get(`${import.meta.env.VITE_APP_ENV}/users/?page=${userPemilih.page}&limit=${userPemilih.limit}&sortByCreated=${userPemilih.sortByDate}&role=PEMILIH`)
        .then((result) => {
            userPemilih.loading = false;
            userPemilih.listUser = result.data.data;
            userPemilih.totalItems = result.data.countROLE;
            userPemilih.totalPages = result.data.totalPagesROLE;
            userPemilih.currentPage = result.data.currentPage;
        }).catch((err) => {
            console.log(err);
            userPemilih.loading = false;
        });
    userPemilih.loading = true;
}

onMounted(() => {
    GetUsers();
    GetUsersByRole();
});
</script>
<template>
    <div class="space-y-4">
        <div class="max-w-full p-4 mx-auto bg-white border border-gray-200 rounded-[10px] shadow-lg">
            <header class="flex justify-between px-5 py-4 border-b border-gray-100">
                <h2 class="font-semibold text-gray-800 text-[18px]">Daftar Admin</h2>
                <h2 class="font-medium text-center text-[18px] text-white">
                    <nuxt-link class="p-1 px-4 bg-green-500 rounded" to="/app/user-create/user">
                        Buat User
                    </nuxt-link>
                </h2>

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
                                    <div class="font-semibold text-left">Foto Identitas</div>
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
                                    <div class="font-medium text-left text-[14px]">
                                        <img :src="item.identityPicture"
                                            class="w-[100px] h-[100px]  object-center object-cover" alt="pict-user" />
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left text-green-500 cursor-pointer">
                                        <button @click="view(item)">Lihat</button>
                                    </div>
                                </td>
                                <td class="p-2 text-center whitespace-nowrap">
                                    <div
                                        class="flex justify-center space-x-5 text-lg font-medium text-center text-white">
                                        <nuxt-link :to="`/app/user-create/edit-user-${item.id}`">
                                            <button class="p-1 px-4 bg-blue-500 rounded">Edit</button>
                                        </nuxt-link>
                                        <button @click="deleteUserAdmin(item.id)"
                                            class="p-1 px-4 bg-red-500 rounded">Hapus</button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                        <tbody v-else>
                            <tr>
                                <td colspan="4" class="p-5 text-center text-[20px] font-semibold">
                                    loading
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div class="flex justify-center mt-5" v-if="vm.listUser.length">
                <vue-awesome-paginate :totalItems="vm.totalItems" v-model="vm.currentPage" @click="paginateAdmin"
                    :items-per-page="vm.limit" :max-pages-shown="3" paginate-buttons-class="btn"
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
                                    <div class="font-semibold text-left">Nomor Identitas</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Nama Pemilih</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Voted</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Terdaftar pada tanggal</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-left">Foto Identitas</div>
                                </th>
                                <th class="p-2 whitespace-nowrap">
                                    <div class="font-semibold text-center">Aksi</div>
                                </th>
                            </tr>
                        </thead>
                        <tbody v-if="!userPemilih.loading" class="text-[16px] divide-y divide-gray-100">
                            <tr v-for="(item, i) in userPemilih.listUser" :key="`${i}`">
                                <td class="p-2 whitespace-nowrap">
                                    <div class="text-left">{{ item.identityNumber }}</div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="flex items-center text-center">
                                        <div class="font-medium text-gray-800">{{ item.fullName }}</div>
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left text-gray-800">{{ item.ifVoted ? 'Ya' : 'Tidak' }}
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left">
                                        {{ $day(item.createdAt).locale('id').format('DD MMMM YYYY H:mm') }}
                                    </div>
                                </td>
                                <td class="p-2 whitespace-nowrap">
                                    <div class="font-medium text-left text-[14px]">
                                        <img :src="item.identityPicture" class="w-[100px] h-[100px]"
                                            alt="pict-pemilih" />
                                    </div>
                                </td>
                                <td class="p-2 text-center whitespace-nowrap">
                                    <div
                                        class="flex justify-center space-x-5 text-lg font-medium text-center text-white">
                                        <button class="p-1 px-4 bg-blue-500 rounded">Edit</button>
                                        <button @click="deleteUser(item.id)"
                                            class="p-1 px-4 bg-red-500 rounded">Hapus</button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                        <tbody v-else>
                            <tr>
                                <td colspan="3" class="p-5 text-center text-[20px] font-semibold">
                                    loading
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div class="flex justify-center mt-5" v-if="userPemilih.listUser.length">
                <vue-awesome-paginate :totalItems="userPemilih.totalItems" v-model="userPemilih.currentPage"
                    @click="paginatePemilih" :items-per-page="userPemilih.limit" :max-pages-shown="3"
                    paginate-buttons-class="btn" active-page-class="btn-active" back-button-class="back-btn"
                    next-button-class="next-btn">
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
    <DetilComp :dialog="vm.view" @close="vm.view = false">
        <div v-if="!dialog.loading" style="max-height: 100vh"
            class="rounded-xl bg-white shadow-lg w-[540px] [@media(max-width:650px)]:w-[95%]">
            <div class="flex justify-end px-2 pt-2">
                <button
                    class="rounded-full p-[6px] hover:bg-red-500/20 active:bg-red-500/50 transition duration-150 ease-in-out outline-none h-max"
                    @click="vm.view = false">
                    <mdicon name="close" class="text-red-500" size="21" />
                </button>
            </div>
            <div v-if="selected" class="w-auto h-auto p-2 px-4 space-y-2.5">
                <div class="flex justify-center gap-2.5">
                    <div class="">
                        <img :src="selected.identityPicture" class="h-[250px] w-[250px] object-center object-cover"
                            :alt="selected.fullName" loading="lazy">
                    </div>

                </div>
                <div class="flex items-center">
                    <div class="grid gap-1.5">
                        <div class="relative">
                            <h5 class="text-slate-400">Nama Lengkap</h5>
                            <span>{{ selected.fullName }}</span>
                        </div>
                        <div class="relative">
                            <h5 class="text-slate-400">Username</h5>
                            <span>{{ selected.username }}</span>
                        </div>
                        <div>
                            <h5 class="text-slate-400">Email</h5>
                            <span>{{ selected.email }}</span>
                        </div>
                        <div>
                            <h5 class="text-slate-400">Role</h5>
                            <p>{{ selected.role }}</p>
                        </div>
                        <div>
                            <h5 class="text-slate-400">Di buat</h5>
                            <span>{{ $day(selected.createdAt).locale('id').format('DD MMMM YYYY H:mm') }}</span>
                        </div>
                        <div>
                            <h5 class="text-slate-400">Di edit</h5>
                            <span>{{ $day(selected.updatedAt).locale('id').format('DD MMMM YYYY H:mm') }}</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </DetilComp>
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
