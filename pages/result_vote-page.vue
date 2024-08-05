<script setup>
import axios from 'axios';

import logo from '@/assets/img/logo/logo_unsa.png';

const loading = ref(true);

const successNotif = (value) => {
    useNuxtApp().$toast.info(value);
};
const errorNotif = (err) => {
    useNuxtApp().$toast.warn(err);
};

function refresh() {
    loading.value = true
    setTimeout(() => {
        loading.value = false;
        window.location.reload();
        fetchResultVote();
    }, 100);
    loading.value = true;
};

const vm = reactive({
    list: [],
    loading: true,
    status: null,
});

const fetchResultVote = async () => {

    vm.loading = true;

    try {
        vm.loading = false;
        const schedule = await axios.get(`${import.meta.env.VITE_APP_ENV}/schedules/?status=${vm.status = 'ACTIVE'}`);

        if (!schedule.data.data.length) {
            errorNotif('Tidak ada jadwal yang aktif')
        }
        else {

            const electionID = schedule.data.data[0].electionID;


            const result_vote = await axios.get(`${import.meta.env.VITE_APP_ENV}/result-vote/result-new?electionID=${electionID}`);
            vm.list = result_vote.data.data;
            console.log(result_vote.data.data);
        }

    } catch (err) {
        vm.loading = false;
        console.log(err);
        errorNotif(err.response?.data?.message);
    }
    vm.loading = true;
};


onMounted(() => {
    fetchResultVote();
});
</script>
<template>
    <div class="flex items-center justify-center w-full h-auto p-10 bg-white">
        <div class="space-y-5">
            <div class="flex items-center justify-between">
                <h1 class="text-[22px] max-[860px]:text-[18px]">E-Voting</h1>
                <img class="w-[80px] [@media(max-width:540px)]:w-[50px]" :src="logo" alt="logo-unsa">
            </div>
            <div class="w-auto h-auto p-10 max-[540px]:p-4 space-y-10 bg-white rounded drop-shadow">
                <div class="flex justify-center font-bold text-[22px] max-[840px]:text-[20px] max-[540px]:text-[18px]">
                    <h2>Hasil Pemilihan</h2>
                </div>
                <div class="space-y-4">
                    <div v-if="vm.list.length"
                        class="flex flex-col items-center justify-center text-[18px] max-[540px]:text-[16px]">
                        <div v-for="(item, i) in vm.list" :key="i">
                            <div
                                class="space-y-5 w-[700px] max-[860px]:w-[580px] max-[680px]:w-[380px] max-[540px]:w-[300px]">
                                <h1>Kandidat Grup {{ item.group }}</h1>
                                <div class="flex justify-around gap-10 max-[540px]:gap-4 max-[480px]:gap-1">
                                    <div
                                        class="flex flex-col items-center justify-center space-y-5 max-[540px]:space-y-2.5">
                                        <img class="w-[200px] h-[200px] max-[540px]:w-[100px] max-[540px]:h-[100px] object-center object-cover"
                                            :src="item.avatar_ketua" alt="picture-candidate" srcset="">
                                        <span>{{ item.role_ketua ? 'Ketua' : '' }}</span>
                                        <span>{{ item.ketua }}</span>
                                    </div>
                                    <div
                                        class="flex flex-col items-center justify-center space-y-5 max-[540px]:space-y-2.5">
                                        <img class="w-[200px] h-[200px] max-[540px]:w-[100px] max-[540px]:h-[100px] object-center object-cover"
                                            :src="item.avatar_wakil_ketua" alt="picture-candidate" srcset="">
                                        <span>{{ item.role_wakil_ketua ? 'Wakil Ketua' : '' }}</span>
                                        <span>{{ item.wakil_ketua }}</span>
                                    </div>
                                </div>
                                <div class="mt-5">
                                    <span class="grid place-items-center space-y-2.5">
                                        <label class="font-bold text-[18px]" for="hasil-voting">hasil voting</label>
                                        <p id="hasil-voting" class="text-[18px]">
                                            {{ item.count_result_vote }}
                                        </p>
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div v-else class="flex justify-center pt-8 max-[540px]:pt-4">
                        <h1 class="text-[20px] max-[540px]:text-[18px]">Tidak ada pemilihan</h1>
                    </div>
                </div>
                <span class="grid pt-10 max-[540px]:pt-5 place-items-center">
                    <button @click.prevent="refresh()"
                        class="p-1 px-5 max-[540px]:px-2.5 max-[504px]:text-[16px] text-white bg-blue-500 text-[18px] rounded">
                        <h6 v-if="loading">Refresh</h6>
                        <h6 v-else>Loading</h6>
                    </button>
                </span>
            </div>
        </div>
    </div>
</template>