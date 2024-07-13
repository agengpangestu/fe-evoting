<script setup>

</script>
<template>
    <div class="
            flex items-center justify-center 
            w-full h-screen 
            p-10 space-y-5 
            bg-white 
            [@media(max-width:540px)]:w-auto">
        <div class="w-auto space-y-5">
            <div class="w-auto">
                <h1 class="
                    text-[24px] 
                    [@media(max-width:860px)]:text-[20px] 
                    [@media(max-width:540px)]:text-[18px]">
                    E-Voting</h1>
            </div>
            <div class="w-auto h-auto p-5 [@media(max-width:860px)]:p-4">
                <div class="
                        flex justify-center 
                        h-[200px] w-[500px] 
                        [@media(max-width:860px)]:w-[300px] 
                        [@media(max-width:660px)]:w-[250px]">
                    <div class="
                            space-y-4 
                            text-center text-[24px] 
                            [@media(max-width:860px)]:text-[20px] 
                            [@media(max-width:540px)]:text-[18px]">
                        <h2>Pilihanmu Disimpan</h2>
                        <p>Terima Kasih telah memilih <br> calon kandidat</p>
                    </div>
                </div>
                <div class="flex items-center justify-center">
                    <nuxt-link to="/result_vote-page">
                        <button class="
                                text-[22px] 
                                [@media(max-width:860px)]:text-[18px] [@media(max-width:540px)]:px-2 
                                p-1 px-4 
                                bg-blue-500 rounded text-white">
                            Hasil Voting
                        </button>
                    </nuxt-link>
                </div>
            </div>
        </div>
    </div>
</template>