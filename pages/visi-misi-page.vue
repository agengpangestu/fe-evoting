<script setup>
import axios from 'axios';

import logo from '@/assets/img/logo/logo_unsa.png';

const successNotif = (value) => {
    useNuxtApp().$toast.info(value);
};
const errorNotif = (err) => {
    useNuxtApp().$toast.warn(err);
};

const vm = reactive({
    data: [],
    loading: true,
});

const fetchCandidate = async () => {
    vm.loading = true;

    try {
        const data = await axios.get(`${import.meta.env.VITE_APP_ENV}/schedules/?status=${vm.status = 'ACTIVE'}`);

        if (!data.data.data.length) {
            errorNotif('Tidak ada pemilihan');
        }
        else {
            const electionID = data.data.data[0].electionID;
            vm.election = electionID;

            const candidate = await axios.get(`${import.meta.env.VITE_APP_ENV}/candidates/group/?electionID=${electionID}`)
            vm.data = candidate.data.data;
            console.log(vm.data);

            vm.loading = false;
        }
    } catch (err) {
        console.log(err);
        vm.loading = false
    }
    vm.loading = true
};

onMounted(() => {
    fetchCandidate();
});
</script>
<template>
    <div class="flex items-center justify-center w-full h-auto p-10 max-[550px]:p-5 bg-white">
        <div class="space-y-5">
            <div class="flex justify-between px-2">
                <h1 class="text-[22px] max-[700px]:text-[16px] max-[550px]:text-[18px]">E-Voting</h1>
                <nuxt-link class="p-1 px-3 text-white bg-blue-500 rounded" to="/vote-page">Kembali</nuxt-link>
            </div>
            <div class="
                    w-[800px] 
                    max-[840px]:w-[640px] max-[700px]:w-[480px] max-[550px]:w-[320px]
                    h-auto 
                    p-5 space-y-10 
                    max-[550px]:p-7
                    bg-white 
                    rounded drop-shadow">
                <div class="
                        flex 
                        flex-col
                        items-center
                        justify-center 
                        font-bold text-[22px]
                        max-[700px]:text-[20px] max-[550px]:text-[18px]">
                    <img class="w-[80px] [@media(max-width:540px)]:w-[50px] [@media(max-width:540px)]:mb-2.5 mb-5"
                        :src="logo" alt="logo-unsa">
                    <h2>Visi dan Misi Kandidat</h2>
                </div>

                <div class="space-y-4 max-[700px]:space-y-3 max-[550px]:space-y-2">
                    <div class="flex items-center justify-center">
                        <div v-if="Object.keys(vm.data).length">
                            <div v-for="(candidate, i) in vm.data" :key="i">
                                <h2 class="text-[22px] max-[700px]:text-[20px] max-[640px]:text-[18px] font-semibold">
                                    Grup {{ i }}
                                </h2>
                                <div class="flex max-[700px]:flex-col">
                                    <div v-for="item in candidate.DATA" :key="candidate.candidateID"
                                        class="w-[350px] max-[840px]:w-[300px] max-[700px]:w-auto h-auto space-y-5 p-2.5">
                                        <h3 class="text-[20px] max-[700px]:text-[18px] underline">{{ item.candidateName
                                            }}</h3>
                                        <div class="text-center text-[18px] max-[700px]:text-[16px]">
                                            <h4 class="font-bold">Visi</h4>
                                            <p>{{ item.candidateVisi ? item.candidateVisi : 'Tidak ada Visi' }}</p>
                                        </div>
                                        <div class="text-center text-[18px] max-[700px]:text-[16px]">
                                            <h4 class="font-bold">Misi</h4>
                                            <p>{{ item.candidateMisi ? item.candidateMisi : 'Tidak ada Misi' }}</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div v-else class="flex justify-center pt-8">
                            <h1 class="text-[20px] max-[700px]:text-[18px]">Tidak ada pemilihan</h1>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>
