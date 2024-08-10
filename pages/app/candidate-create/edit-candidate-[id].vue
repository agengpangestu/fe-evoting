<script setup>
import axios from 'axios';
import Cookie from 'js-cookie';
import { useRoute } from 'vue-router';

import SelectComp from '~/components/SelectComp.vue';

const route = useRoute();
const candidateId = ref(parseInt(route.params.id));

const successNotif = (value) => {
    useNuxtApp().$toast.info(value);
};
const errorNotif = (err) => {
    useNuxtApp().$toast.warn(err);
};

const formData = reactive({
    candidateName: '',
    candidateVisi: '',
    candidateMisi: '',
    candidateAvatar: null,
    candidateRole: '',
    faculty: '',
    major: '',
    class: '',
    group: '',
    electionID: null,
    level: '',
    createdBy: null,
    loading: false
});
const RoleCandidate = reactive({
    list: [
        { id: 0, value: "KETUA", name: "Ketua" },
        { id: 1, value: "WAKIL_KETUA", name: "Wakil Ketua" }
    ],
    menu: false,
    load: false,
    selected: null,
});
const Level = reactive({
    list: [{ name: "ORGANISASI" }],
    menu: false,
    load: false,
    selected: null
});
const Schedule = reactive({
    list: [],
    menu: false,
    load: false,
    selected: null
});
const Faculty = reactive({
    list: [
        {
            id: 0, value: "Ilmu_Sosial_dan_Ilmu_Politik", name: "Ilmu Sosial dan Ilmu Politik",
            major:
                [
                    { id: 0, value: "Ilmu_Administrasi_Negara", name: "Ilmu Administrasi Negara" },
                    { id: 1, value: "Komunikasi", name: "Komunikasi" },
                ]
        },
        {
            id: 1, value: "Ekonomi", name: "Ekonomi",
            major:
                [
                    { id: 2, value: "Akuntansi", name: "Akuntansi" },
                    { id: 3, value: "Manajemen", name: "Manajemen" },
                ]
        },
        {
            id: 2, value: "Hukum", name: "Hukum",
            major: [
                { id: 4, value: "Hukum", name: "Hukum" },
            ]
        },
        {
            id: 3, value: "Bahasa_dan_Sastra", name: "Bahasa dan Sastra",
            major: [
                { id: 5, value: "Bahasa_Inggris", name: "Bahasa Inggris" },
            ]
        },
        {
            id: 4, value: "Teknologi_Industri", name: "Teknologi Industri",
            major: [
                { id: 6, value: "Teknik_Mesin", name: "Teknik Mesin" },
            ]
        },
        {
            id: 5, value: "Teknik_Elektro_dan_Informatika", name: "Teknik Elektro dan Informatika",
            major: [
                { id: 7, value: "Teknik_Elektro", name: "Teknik Elektro" },
                { id: 8, value: "Teknik_Informatika", name: "Teknik Informatika" },
                { id: 8, value: "Sistem_Komputer", name: "Sistem Komputer" },
            ]
        },
        {
            id: 6, value: "Teknik_Sipil_dan_Perencanaan", name: "Teknik Sipil dan Perencanaan",
            major: [
                { id: 9, value: "Teknik_Sipil", name: "Teknik Sipil" },
                { id: 10, value: "Arsitektur", name: "Arsitektur" },
            ]
        },
    ],
    menu: false,
    load: false,
    selected: null
});
const Major = reactive({
    list: [
        { id: 0, facultyId: 0, value: "Ilmu_Administrasi_Negara", name: "Ilmu Administrasi Negara", faculty: "Ilmu_Sosial_dan_Ilmu_Politik" },
        { id: 1, facultyId: 0, value: "Komunikasi", name: "Komunikasi", faculty: "Ilmu_Sosial_dan_Ilmu_Politik" },
        { id: 2, facultyId: 1, value: "Akuntansi", name: "Akuntansi", faculty: "Ekonomi" },
        { id: 3, facultyId: 1, value: "Manajemen", name: "Manajemen", faculty: "Ekonomi" },
        { id: 4, facultyId: 2, value: "Hukum", name: "Hukum", faculty: "Hukum" },
        { id: 5, facultyId: 3, value: "Bahasa_Inggris", name: "Bahasa Inggris", faculty: "Bahasa_dan_Sastra" },
        { id: 6, facultyId: 4, value: "Teknik_Mesin", name: "Teknik Mesin", faculty: "Teknologi_Industri" },
        { id: 7, facultyId: 5, value: "Teknik_Elektro", name: "Teknik Elektro", faculty: "Teknik_Elektro_dan_Informatika" },
        { id: 8, facultyId: 5, value: "Teknik_Informatika", name: "Teknik Informatika", faculty: "Teknik_Elektro_dan_Informatika" },
        { id: 8, facultyId: 5, value: "Sistem_Komputer", name: "Sistem Komputer", faculty: "Teknik_Elektro_dan_Informatika" },
        { id: 9, facultyId: 6, value: "Teknik_Sipil", name: "Teknik Sipil", faculty: "Teknik_Sipil_dan_Perencanaan" },
        { id: 10, facultyId: 6, value: "Arsitektur", name: "Arsitektur", faculty: "Teknik_Sipil_dan_Perencanaan" },
    ],
    test: [],
    menu: false,
    load: false,
    selected: null
});
const Class = reactive({
    list: [
        { id: 0, value: "REGULER_PAGI", name: "Reguler Pagi" },
        { id: 1, value: "REGULER_MALAM", name: "Reguler Malam" },
    ],
    menu: false,
    load: false,
    selected: null
});



const selectRoleCandidate = (item) => {
    RoleCandidate.selected = item;
};
const selectedLevel = (item) => {
    Level.selected = item;
};
const selectSchedule = (item) => {
    Schedule.selected = item;
    fetchSchedule(item);
};
const selectFacultyAndMajor = (item) => {
    Faculty.selected = item;
    Major.test = Faculty.selected?.major;
};
const selectMajor = (item) => {
    Major.selected = item;
};
const selectClass = (item) => {
    Class.selected = item;
};

const fetchSchedule = () => {
    axios.get(`${import.meta.env.VITE_APP_ENV}/schedules`)
        .then((result) => {
            Schedule.list = result.data.data;
        }).catch((err) => {
            console.log(err);
        });
};

const fetchData = () => {
    axios.get(`${import.meta.env.VITE_APP_ENV}/candidates/${candidateId.value}/candidate`)
        .then((result) => {
            let rawData = result.data.data;
            formData.candidateName = rawData.candidateName;
            formData.candidateVisi = rawData.candidateVisi;
            formData.candidateMisi = rawData.candidateMisi;
            formData.group = rawData.group;
            formData.candidateAvatar = rawData.candidateAvatar;
            RoleCandidate.selected = rawData.candidateRole;
            Schedule.selected = rawData.Election;
            Level.selected = rawData.level;
            Faculty.selected = rawData.candidateFaculty;
            Major.selected = rawData.candidateMajor;
            Class.selected = rawData.candidateClass;
        }).catch((err) => {
            console.log(err);
        });
};
const postData = () => {
    let body = new FormData();

    const userId = Cookie.get('name_user.id');

    body.append('candidateName', formData.candidateName);
    body.append('candidateVisi', formData.candidateVisi);
    body.append('candidateMisi', formData.candidateMisi);
    body.append('candidateAvatar', formData.candidateAvatar);
    body.append('group', formData.group);
    body.append('candidateRole', RoleCandidate.selected?.value || RoleCandidate.selected);
    body.append('candidateFaculty', Faculty.selected?.value || Faculty.selected);
    body.append('candidateMajor', Major.selected?.value || Major.selected);
    body.append('candidateClass', Class.selected?.value || Class.selected);
    body.append('electionID', Schedule.selected?.electionID);
    body.append('level', Level.selected?.name || Level.selected);
    body.append('createdBy', parseInt(userId));

    formData.loading = true;

    axios.patch(`${import.meta.env.VITE_APP_ENV}/candidates/${candidateId.value}/candidate-update`, body
        , {
            headers: { "Content-Type": "multipart/form-data" }
            // headers: { 'Content-Type': 'application/json' },

        }
    )
        .then((result) => {
            successNotif(result.data.message ? 'Berhasil melakukan edit kandidat' : result.data.message);
            formData.loading = false;
        }).catch((err) => {
            console.log(err.response);
            successNotif(err.response?.data?.message);
            formData.loading = false
        });
    formData.loading = true
};

const classComputed = computed(() => {
    if (Class.selected) return Class.selected.split('_').map(word => word.charAt(0) + word.slice(1).toLowerCase()).join(' ');
});
const majorComputed = computed(() => {
    if (Major.selected) return Major.selected.split('_').map(word => word.charAt(0) + word.slice(1).toLowerCase()).join(' ');
});
const facultyComputed = computed(() => {
    if (Faculty.selected) return Faculty.selected.split('_').map(word => word.charAt(0) + word.slice(1).toLowerCase()).join(' ');
});
const roleComputed = computed(() => {
    if (RoleCandidate.selected) return RoleCandidate.selected.split('_').map(word => word.charAt(0) + word.slice(1).toLowerCase()).join(' ');
});

onMounted(() => {
    fetchData();
    fetchSchedule();
});

</script>
<template>
    <div class="p-10">
        <div class="h-auto p-5 space-y-5 bg-white rounded-md shadow-md drop-shadow min-w-max">
            <div class="px-5">
                <h1 class="text-[22px]">Edit Kandidat</h1>
            </div>
            <form @submit.prevent="postData">
                <div class="flex justify-center space-x-5">
                    <div class="space-y-4">
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="nama">Nama Kandidat</label>
                            <input placeholder="Nama Kandidat"
                                class="w-[350px] remove-arrow outline outline-1 rounded pl-2 py-1"
                                v-model="formData.candidateName" type="text" name="nama" id="nama">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="group">Group</label>
                            <input placeholder="Group"
                                class="w-[350px] remove-arrow outline outline-1 rounded pl-2 py-1"
                                v-model="formData.group" type="text" name="group" id="group">
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="faculty">Fakultas</label>
                            <SelectComp v-model="Faculty.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] outline outline-1 cursor-pointer flex items-center bg-white"
                                    @click="Faculty.menu = !Faculty.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ Faculty.selected?.name ?? facultyComputed ?? "Pilih Fakultas" }}
                                    </p>
                                    <mdicon name="chevron-down" :class="`
                                            transition-all 
                                            delay-1 ${Faculty.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll" v-if="Faculty.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in Faculty.list" :key="`category-${i}`"
                                            @click="selectFacultyAndMajor(item)">
                                            {{ item.name }}
                                        </div>
                                        <div class="flex justify-center py-1" v-if="Faculty.next && !Faculty.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="Faculty.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>

                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="major">Jurusan</label>
                            <SelectComp v-model="Major.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] outline outline-1 cursor-pointer flex items-center bg-white"
                                    @click="Major.menu = !Major.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ Major.selected?.name ?? majorComputed ?? "Pilih Jurusan" }}
                                    </p>
                                    <mdicon name="chevron-down" :class="`
                                            transition-all 
                                            delay-1 ${Major.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll" v-if="Major.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in Major.test" :key="`category-${i}`"
                                            @click="selectMajor(item)">
                                            {{ item.name }}
                                        </div>
                                        <div class="flex justify-center py-1" v-if="Major.next && !Major.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="Major.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>

                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="class">Kelas</label>
                            <SelectComp v-model="Class.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] outline outline-1 cursor-pointer flex items-center bg-white"
                                    @click="Class.menu = !Class.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ Class.selected?.name ?? classComputed ?? "Pilih Kelas" }}
                                    </p>
                                    <mdicon name="chevron-down" :class="`
                                            transition-all 
                                            delay-1 ${Class.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll" v-if="Class.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in Class.list" :key="`category-${i}`"
                                            @click="selectClass(item)">
                                            {{ item.name }}
                                        </div>
                                        <div class="flex justify-center py-1" v-if="Class.next && !Class.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="Class.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="role">Role</label>
                            <SelectComp v-model="RoleCandidate.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] border-gray cursor-pointer flex items-center bg-white"
                                    @click="RoleCandidate.menu = !RoleCandidate.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ RoleCandidate.selected?.name ?? roleComputed ?? "Pilih Role" }}
                                    </p>
                                    <mdicon name="chevron-down"
                                        :class="`transition-all delay-1 ${RoleCandidate.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll"
                                        v-if="RoleCandidate.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in RoleCandidate.list" :key="`category-${i}`"
                                            @click="selectRoleCandidate(item)">
                                            {{ item.name }}
                                        </div>
                                        <div class="flex justify-center py-1"
                                            v-if="RoleCandidate.next && !RoleCandidate.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="RoleCandidate.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="jadwal">Jadwal</label>
                            <SelectComp v-model="Schedule.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] border-gray cursor-pointer flex items-center bg-white"
                                    @click="Schedule.menu = !Schedule.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ Schedule.selected?.electionName ?? "Pilih Jadwal" }}
                                    </p>
                                    <mdicon name="chevron-down"
                                        :class="`transition-all delay-1 ${Schedule.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll"
                                        v-if="Schedule.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in Schedule.list" :key="`category-${i}`"
                                            @click="selectSchedule(item)">
                                            {{ item.electionName }}
                                        </div>
                                        <div class="flex justify-center py-1" v-if="Schedule.next && !Schedule.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="Schedule.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="level">Level</label>
                            <SelectComp v-model="Level.menu" full>
                                <div class="py-2 pl-3 pr-2 w-[350px] border rounded-[8px] border-gray cursor-pointer flex items-center bg-white"
                                    @click="Level.menu = !Level.menu" role="activator">
                                    <p class="text-[15px] flex-1 font-medium text-grey1">
                                        {{ Level.selected?.name ?? Level.selected ?? "Pilih Level" }}
                                    </p>
                                    <mdicon name="chevron-down"
                                        :class="`transition-all delay-1 ${Level.menu ? 'rotate-180' : 'rotate-0'}`" />
                                </div>

                                <template v-slot:item>
                                    <div class="max-h-[250px] overflow-y-auto styled-scroll" v-if="Level.list.length">
                                        <div class="hover:bg-gray-100 p-2 font-medium rounded-[8px] text-[15px] text-grey1 cursor-pointer"
                                            v-for="(item, i) in Level.list" :key="`category-${i}`"
                                            @click="selectedLevel(item)">
                                            {{ item.name }}
                                        </div>
                                        <div class="flex justify-center py-1" v-if="Level.next && !Level.load">
                                        </div>
                                    </div>
                                    <p class="text-sm" v-else>...</p>
                                    <p v-if="Level.load" class="text-xs text-center">
                                        Sedang memuat data...
                                    </p>
                                </template>
                            </SelectComp>
                        </div>
                    </div>
                    <div class="space-y-4">
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="visi">Visi</label>
                            <textarea v-model="formData.candidateVisi"
                                class="w-[350px] outline outline-1 rounded pl-2 py-1" name="visi" id="visi" cols="30"
                                rows="10" />
                        </div>
                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="misi">Misi</label>
                            <textarea v-model="formData.candidateMisi"
                                class="w-[350px] outline outline-1 rounded pl-2 py-1" name="misi" id="misi" cols="30"
                                rows="10" />
                        </div>

                        <div class="flex flex-col space-y-2 text-[18px]">
                            <label for="uploadCardKandidat">Foto Kandidat</label>
                            <div class="relative inline-block">
                                <input class="
                                    w-[210px] 
                                    text-[16px] 
                                    file:absolute file:right-0 file:bg-blue-500 
                                    file:text-white file:border-none
                                    file:py-1 file:px-3 file:rounded-full
                                    file:shadow-md file:shadow-blue-500/25" type="file" name="uploadCardKandidat"
                                    id="uploadCardKandidat" @change="e => formData.candidateAvatar = e.target.files[0]">
                            </div>
                        </div>
                    </div>

                </div>
                <div class="flex justify-center pt-8">
                    <nuxt-link to="">
                        <button v-if="!formData.loading" type="submit"
                            class="bg-blue-500 text-white px-3 py-1.5 rounded">
                            <h4 class="font-medium">Submit</h4>
                        </button>
                        <button v-else type="submit" class="bg-blue-500 text-white px-3 py-1.5 rounded">
                            <h4 class="font-medium">Loading</h4>
                        </button>
                    </nuxt-link>
                </div>
            </form>
        </div>
    </div>
</template>
