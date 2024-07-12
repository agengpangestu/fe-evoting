<script setup>
import axios from 'axios';
import Cookie from 'js-cookie';

definePageMeta({
    middleware: ['not-auth-vote']
});

const screenSize = ref({
    width: window.innerWidth,
    height: window.innerHeight
});
const updateScreenSize = () => {
    screenSize.value.width = window.innerWidth;
    screenSize.value.height = window.innerHeight;
};

window.addEventListener('resize', updateScreenSize);

const styleToast = () => {
    if (screenSize.value.width < 600) {
        return {
            width: '300px',
            fontSize: '14px',
            padding: '5px',
        };
    } else if (screenSize.value.width >= 600 && screenSize.value.width < 1200) {
        return {
            fontSize: '16px',
            padding: '10px'
        };
    } else {
        return {
            fontSize: '20px',
            padding: '15px'
        };
    }
};
const successNotif = (value) => {
    const style = styleToast();
    useNuxtApp().$toast.info(value, {
        style
    });
};
const errorNotif = (err) => {
    const style = styleToast();
    useNuxtApp().$toast.warn(err, {
        style
    });
};
const formData = reactive({
    identityNumber: '',
    fullName: '',
    identityPicture: null,
    loading: false,
});

const submitForm = () => {

    // if (!formData.identityNumber && !formData.identityNumber) return errorNotif('Nomor Identitas & Nama belum diisi');
    // if (!formData.identityNumber) return errorNotif('Nomor Identitas belum diisi');
    // if (!formData.fullName) return errorNotif('Nama belum diisi');

    let body = new FormData();
    body.append('identityNumber', formData.identityNumber);
    body.append('fullName', formData.fullName);
    body.append('identityPicture', formData.identityPicture);

    formData.loading = true;

    axios.post(`${import.meta.env.VITE_APP_ENV}/registration-user/post`, body,
        {
            headers: { "Content-Type": "multipart/form-data" }
        }
    )
        .then((result) => {
            formData.loading = false;
            const user_pemilih = result?.data?.data[0],
                token = result.data?.token;
            console.log(token);

            Cookie.set('user_pemilih.id', user_pemilih.id);
            Cookie.set('user_pemilih.name', user_pemilih.nama_pemilih);
            Cookie.set('user_pemilih.token', token)
            successNotif(result.data?.message);

            setTimeout(() => {
                navigateTo('/vote-page')
            }, 3000);
        }).catch((err) => {
            formData.loading = false;
            console.log(err);
            errorNotif(err?.response?.data?.message);
        });

    formData.loading = true;
};
onMounted(() => {
    updateScreenSize()
});
</script>

<template>

    <div class="flex items-center justify-center w-full h-screen p-10">
        <div class="w-auto h-auto space-y-5 bg-white">
            <div>
                <h1 class="text-[22px] [@media(max-width:540px)]:text-[20px]">E-Voting</h1>
            </div>
            <div class="
                    bg-white 
                    drop-shadow 
                    [@media(max-width:540px)]:w-[340px] 
                    [@media(max-width:540px)]:h-auto
                    w-[450px] h-[500px] 
                    p-10 space-y-10 
                    rounded">
                <div class="
                        flex justify-center 
                        font-bold text-[22px] 
                        [@media(max-width:540px)]:text-[18px]">
                    <h2>Formulir Pendaftaran Pemilihan</h2>
                </div>
                <form @submit.prevent="submitForm">
                    <div class="space-y-4">
                        <div class="flex flex-col space-y-2 text-[18px] [@media(max-width:540px)]:text-[16px]">
                            <label for="identityNumber">Nomor Identitas</label>
                            <input placeholder="Nomor Identitas"
                                class="w-full py-1 pl-2 rounded remove-arrow outline outline-1"
                                v-model="formData.identityNumber" type="number" min="0" name="identityNumber"
                                id="identityNumber">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px] [@media(max-width:540px)]:text-[16px]">
                            <label for="fullname">Nama Lengkap</label>
                            <input placeholder="Nama Lengkap" class="w-full py-1 pl-2 rounded outline outline-1"
                                v-model="formData.fullName" type="text" name="fullname" id="fullname">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px] [@media(max-width:540px)]:text-[16px]">
                            <label for="uploadCard">Upload Kartu Identitas</label>
                            <div class="relative inline-block">
                                <input class="
                                    w-[210px] 
                                    text-[16px] 
                                    [@media(max-width:540px)]:text-[14px]
                                    file:absolute file:right-0 file:bg-blue-500 
                                    file:text-white file:border-none
                                    file:py-1 file:px-3 file:rounded-full
                                    file:shadow-md file:shadow-blue-500/25" type="file" name="uploadCard"
                                    id="uploadCard" @change="e => formData.identityPicture = e.target.files[0]">
                            </div>
                        </div>

                        <div class="flex justify-center pt-8">
                            <nuxt-link to="">
                                <button v-if="!formData.loading" class="
                                        p-2 
                                        w-[350px] 
                                        text-white 
                                        bg-blue-500 
                                        rounded-md 
                                        text-[20px]
                                        [@media(max-width:540px)]:text-[16px]
                                        [@media(max-width:540px)]:w-[260px]
                                        [@media(max-width:540px)]:p-0.5">
                                    Daftar
                                </button>
                                <button v-else class="
                                        flex justify-center items-center
                                        gap-1.5
                                        p-2 w-[350px] 
                                        text-white 
                                        bg-blue-500 
                                        rounded-md 
                                        text-[20px] 
                                        [@media(max-width:540px)]:text-[16px]
                                        [@media(max-width:540px)]:w-[260px]
                                        [@media(max-width:540px)]:p-0.5">
                                    Loading
                                    <svg width="15px" height="15px" viewBox="0 0 16 16"
                                        xmlns="http://www.w3.org/2000/svg" fill="none"
                                        class="hds-flight-icon--animation-loading animate-spin">
                                        <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                                        <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round">
                                        </g>
                                        <g id="SVGRepo_iconCarrier">
                                            <g fill="#ffffff" fill-rule="evenodd" clip-rule="evenodd">
                                                <path
                                                    d="M8 1.5a6.5 6.5 0 100 13 6.5 6.5 0 000-13zM0 8a8 8 0 1116 0A8 8 0 010 8z"
                                                    opacity=".2"></path>
                                                <path
                                                    d="M7.25.75A.75.75 0 018 0a8 8 0 018 8 .75.75 0 01-1.5 0A6.5 6.5 0 008 1.5a.75.75 0 01-.75-.75z">
                                                </path>
                                            </g>
                                        </g>
                                    </svg>
                                </button>
                            </nuxt-link>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<style>
.remove-arrow::-webkit-inner-spin-button,
.remove-arrow::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.remove-arrow {
    -appearance: textfield;
}
</style>