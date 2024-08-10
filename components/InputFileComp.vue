<script setup>
const fileInput = ref(null);
const fileName = ref('Tidak ada file');
const buttonText = ref('Pilih gambar');

defineProps(['formData']);

const triggerHandle = () => {
    fileInput.value.click();
};
const handleFileUpload = (e) => {
    const file = e.target.files[0];
    fileName.value = file ? file.name : 'Tidak ada gambar';
    buttonText.value = file ? "Ubah gambar" : "Pilih gambar";
    formData.identityPicture = file;
};

</script>
<template>
    <div class="relative flex items-center justify-around p-1 rounded outline-dashed outline-[1px]">
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
            <span>{{ fileName }}</span>
            <button type="button" class="p-1 px-2 text-white bg-blue-500 rounded-md" @click="triggerHandle">{{ buttonText }}</button>
    </div>
</template>
