<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-6 mx-auto">
        <div class="card">
          <div class="card-header bg-warning text-black text-center">
            <h2 style="font-weight: bold">สมัครสมาชิก</h2>
          </div>
          <div class="card-body">
            <div class="alert alert-danger" role="alert" v-if="firebaseError">
              {{ firebaseError }}
            </div>
            <div class="form-group">
              <label for="email" class="form-label">อีเมล์</label>
              <input v-model="email" class="form-control" type="text" required />
            </div>
            <div class="form-group my-3">
              <label for="password" class="form-label">รหัสผ่าน</label>
              <input v-model="password" class="form-control" type="password" required />
            </div>
            <div>
              <RouterLink to="/" class="ms-1" style="text-decoration: none"
                >เข้าสู่ระบบ</RouterLink
              >
            </div>
            <hr />
            <button
              class="btn btn-warning d-block mx-auto"
              @click="register"
              :disabled="isBtnLoad"
              type="button"
            >
              <span
                v-if="isBtnLoad"
                class="spinner-border spinner-border-sm me-2"
                role="status"
              ></span>
              {{ btnMessage }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const [email, password] = [ref(""), ref(""), ref("")];
const firebaseError = ref();
const [isBtnLoad, btnMessage] = [ref(false), ref("สมัครสมาชิก")];
const router = useRouter();

function register() {
  const auth = getAuth();
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  createUserWithEmailAndPassword(auth, email.value, password.value)
    .then(() => {
      router.push("/");
    })
    .catch((error) => {
      const errorMessage = error.message;
      [isBtnLoad.value, btnMessage.value] = [false, "สมัครสมาชิก"];
      firebaseError.value = errorMessage;
      // ..
    });
}
</script>
