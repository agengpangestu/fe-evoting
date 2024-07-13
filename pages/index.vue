<template>
  <div class="flex items-center justify-center w-screen h-screen p-10 bg-white ">
    <div class="
            flex flex-col justify-center 
            w-[500px] h-max 
            p-8 space-y-10 
            bg-white 
            rounded 
            shadow-md drop-shadow">
      <div class="
              text-[30px] text-center 
              [@media(max-width:540px)]:text-[20px]">
        <h1>Selamat Datang</h1>
      </div>
      <div class="
              text-[26px] text-center 
              [@media(max-width:540px)]:text-[18px]">
        <h2>DI Website Pemilihan Ketua</h2>
        <h2>BEM UNSA</h2>
      </div>
      <div class="
              text-[24px] text-center 
              [@media(max-width:540px)]:text-[16px]">
        <div class="mb-5">
          <h3>Klik tombol pendaftaran untuk</h3>
          <h3>menuju halaman pendaftaran</h3>
        </div>
        <nuxt-link class="p-1 px-3 text-white bg-blue-500 rounded-md" to="/form-page">
          Pendaftaran
        </nuxt-link>
      </div>

    </div>
  </div>
</template>