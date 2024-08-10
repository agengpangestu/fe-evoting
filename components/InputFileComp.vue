<script setup>
const fileInput = ref(null);
const fileName = ref('Tidak ada file');
const buttonText = ref('Pilih File');

defineProps(['formData']);

const triggerHandle = () => {
    fileInput.value.click();
};
const handleFileUpload = (e) => {
    const file = e.target.files[0];
    fileName.value = file ? file.name : 'Tidak ada gambar';
    buttonText.value = 'Ubah';
    formData.identityPicture = file;
};
</script>
<template>
    <div class="relative flex items-center justify-around">
        <input class="
                    hidden
                    w-[210px] 
                    text-[16px] 
                    [@media(max-width:540px)]:text-[14px]
                    file:absolute file:right-0 file:bg-blue-500 
                     file:text-white file:border-none
                    file:py-1 file:px-3 file:rounded-full
                    file:shadow-md file:shadow-blue-500/25" type="file" name="uploadCard" id="uploadCard"
            ref="fileInput" @change="handleFileUpload">
            <span>{{ fileName === "Tidak ada gambar" ? "Pilih gambar" : "Ubah gambar" }}</span>
            <button type="button" class="p-1 bg-blue-500" @click="triggerHandle">{{ buttonText }}</button>
    </div>
</template>
