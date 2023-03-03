<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-6 mx-auto">
        <div class="card">
          <div class="card-header bg-warning text-black text-center">
            <h2 style="font-weight: bold">เข้าสู่ระบบ</h2>
          </div>
          <div class="card-body">
            <div class="alert alert-danger" role="alert" v-if="firebaseError">
              {{ firebaseError }}
            </div>
            <form>
              <div class="form-group">
                <label for="email" class="form-label">อีเมล์</label>
                <input
                  v-model="email"
                  class="form-control"
                  type="email"
                  id="email"
                  required
                />
              </div>
              <div class="form-group my-3">
                <label for="password" class="form-label">รหัสผ่าน</label>
                <input
                  v-model="password"
                  class="form-control"
                  type="password"
                  id="password"
                  required
                />
              </div>
              <div class="d-flex justify-content-between align-items-center">
                <div>
                  <RouterLink to="/register" class="ms-1" style="text-decoration: none"
                    >สมัครสมาชิก</RouterLink
                  >
                </div>
              </div>
            </form>
            <hr />
            <div class="text-center">
              <button
                class="btn btn-warning"
                @click="login"
                :disabled="isBtnLoad"
                type="submit"
                aria-label="Sign in"
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
  </div>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const [email, password] = [ref(""), ref("")];
const firebaseError = ref();
const [isBtnLoad, btnMessage] = [ref(false), ref("เข้าสู่ระบบ")];
const router = useRouter();

function login() {
  const auth = getAuth();
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  signInWithEmailAndPassword(auth, email.value, password.value)
    .then(() => {
      router.push("/");
    })
    .catch((error) => {
      const errorMessage = error.message;
      [isBtnLoad.value, btnMessage.value] = [false, "เข้าสู่ระบบ"];
      firebaseError.value = errorMessage;
      // ..
    });
}
</script>
<style scoped></style>
