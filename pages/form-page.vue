<script setup>

const formData = reactive({
    identityNumber: '',
    fullName: '',
    uploadIdentity: ''
});

const submitForm = async () => {
    const { data: responseData } = await useFetch(`${import.meta.env.VITE_APP_ENV}/users/post`, {
        method: 'post',
        body: {
            identityNumber: formData.identityNumber,
            fullName: formData.fullName,
            identityPicture: formData.uploadIdentity
        }
    }).then((result) => {
        (result.status === 200) ? console.log('Post SUccess') : console.log('Have error at form post');
    }).catch((err) => {
        console.log(err);
    });

    return responseData;
};

</script>
<style>
.remove-arrow::-webkit-inner-spin-button,
.remove-arrow::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.remove-arrow {
    -moz-appearance: textfield;
}
</style>
<template>

    <body class="flex justify-center w-auto h-auto">
        <div class="mt-16 space-y-10 bg-white w-max h-max">
            <div>
                <h1 class="text-[22px]">E-Voting</h1>
            </div>
            <div class="bg-white drop-shadow w-[400px] p-4 space-y-4 rounded">
                <div class="flex justify-center font-bold text-[22px]">
                    <h2>Formulir Pendaftaran</h2>
                </div>
                <form @submit.prevent="submitForm" action="">
                    <div class="space-y-4">
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="identityNumber">Nomor Identitas</label>
                            <input placeholder="Nomor Identitas"
                                class="w-[250px] remove-arrow outline outline-1 rounded pl-2 py-1"
                                v-model="formData.identityNumber" type="number" min="0" name="identityNumber"
                                id="identityNumber">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="fullname">Nama Lengkap</label>
                            <input placeholder="Nama Lengkap" class="w-[250px] outline outline-1 rounded pl-2 py-1"
                                v-model="formData.fullName" type="text" name="fullname" id="fullname">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="uploadCard">Upload Kartu Identitas</label>
                            <div class="relative inline-block">
                                <input class="
                                    w-[210px] 
                                    text-[16px] 
                                    file:absolute file:right-0 file:bg-blue-500 
                                    file:text-white file:border-none
                                    file:py-1 file:px-3 file:rounded-full
                                    file:shadow-md file:shadow-blue-500/25" type="file" name="uploadCard"
                                    id="uploadCard" @change="e => formData.uploadIdentity = e.target.files[0]">
                            </div>
                        </div>

                        <div class="flex justify-center pt-8">
                            <nuxt-link to="">
                                <button type="submit" class="bg-blue-500 text-white px-3 py-1.5 rounded">
                                    <h4 class="font-medium">Submit</h4>
                                </button>
                            </nuxt-link>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </body>
</template>
