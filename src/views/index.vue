<script setup lang="ts">
import { ref } from "vue"
import crypto from "crypto-js"
import Input from "@/components/form/input.vue"
import FileUploader from "@/components/form/file-uploader.vue"

const $password = ref("")
const encrptFiles = ref<FileList | never[]>([])

const createObjectURL = URL.createObjectURL
const revokeObjectURL = URL.revokeObjectURL
</script>

<template>
  <main>
    <div>
      <FileUploader class="encrypt-uploader" multiple
        label="내 디바이스에서 업로드"
        v-model="encrptFiles"
      />
    </div>

    <section>
      <hgroup>
        <h2>파일 암호화</h2>
        <p>
          업로드한 파일에 암호를 설정하고, 파일을 안전하게 보관하세요.
        </p>
      </hgroup>

      <Input label="암호 입력" v-model="$password" />

      <template v-for="file in encrptFiles">
        <a :href="`data:text/plain;charset=utf-8,${crypto.AES.encrypt(createObjectURL(file), $password).toString()}`" :download="$password">암호화된 파일 다운로드</a>
        <img :src="createObjectURL(file)" alt="" @load="e => revokeObjectURL(e.currentTarget!.src)">
      </template>
    </section>
  </main>
</template>

<style scoped>
main {
  display: flex;
  font-size: 2rem;
  margin: 0 2rem;
  gap: 3rem;
}

section {
  flex:  1 1  auto;
}

h2 {
  font-size: 3.5rem;
  font-weight: 700;
}

.encrypt-uploader {
  flex: 0 0 auto;
  position: relative;
  display: grid;
  align-items: center;
  justify-content: center;
  border: none;
  padding: 1em;
  width: 40rem;
  height: 20rem;
  /* box-sizing: border-box; */

  z-index: 1;
  font-size: 2rem;
  border-radius: 4rem;
  background-color: #1a1a1a;
}
.encrypt-uploader.dragged { background-color: black; }
.encrypt-uploader > :deep(input) {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content-visibility: hidden;
  z-index: 1;
}
</style>
