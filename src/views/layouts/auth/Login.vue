<template>
  <div class="w-full h-screen">
    <div class="flex shadow rounded-md h-screen">
      <div class="bg-white dark:bg-gray-900 w-full">
        <form @submit.prevent="submitForm">
          <div class="form-body lg:max-w-xl mx-auto lg:p-20 p-8 lg:mt-20 mt-5 space-y-8">
            <div class="space-y-3">
              <h2 class="dark:text-white font-semibold text-gray-800 text-4xl">
                Welcome, to Windzo<span class="text-primary">.</span>
              </h2>
            </div>
            <div class="space-y-5">
              <div class="relative z-0 w-full mb-6 group">
                <input id="username" type="text" name="floating_email" v-model="username"
                  class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-primary focus:outline-none focus:ring-0 focus:border-primary peer"
                  placeholder=" " required />
                <label for="floating_email"
                  class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:left-0 peer-focus:text-primary peer-focus:dark:text-primary peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6">username</label>
              </div>
              <div class="relative z-0 w-full mb-6 group">
                <input type="password" name="floating_password" id="password" v-model="password"
                  class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-primary focus:outline-none focus:ring-0 focus:border-primary peer"
                  placeholder=" " required />
                <label for="floating_password"
                  class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:left-0 peer-focus:text-primary peer-focus:dark:text-primary peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6">Password</label>
              </div>
              <div v-show="inCorrect" class="incorrect text-red-500 text-sm dark:text-white">
                Login yoki parol xato
              </div>
            </div>
            <div class="flex justify-between">
              <div class="flex items-start">
                <div class="flex items-center h-5">
                  <input id="remember" type="checkbox" value="" autocomplete="off"
                    class="accent-primary focus:ring-4 cursor-pointer w-4 h-4 border border-gray-300 rounded dark:bg-gray-700 bg-gray-50 focus:ring-3 focus:ring-primary/30" />
                </div>
                <label for="remember"
                  class="ml-2 text-sm cursor-pointer font-normal dark:text-white text-gray-500">Remember for 30
                  days</label>
              </div>
            </div>

            <button @click="login()" @dblclick="incorrect()"
              class="text-white bg-primary hover:bg-primary/80 p-3 w-full rounded-md">
              Login, to continue
            </button>
            <p class="dark:text-white text-center text-gray-700">
              Don't have an account?<button type="button" @click="$router.push('/auth/register')"
                class="ml-2 text-primary">
                Register here
              </button>
            </p>
          </div>
        </form>
      </div>
      <div class="bg-wave dark:bg-gray-900 bg-white w-2/5 lg:block hidden"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const username = ref("");
const password = ref("");
const inCorrect = ref(false);
function incorrect() {
  let incorrect = document.querySelector(".incorrect");
  incorrect.classList.add("show");
  setTimeout(() => {
    incorrect.classList.remove("show");
  }, "1000");
}
function login() {
  fetch("http://fazl.karimjonov.uz/api/admin/login", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      user_name: username.value,
      password: password.value,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.success) {
        localStorage.setItem('token', data.data.token)
        window.location.href = "/";
      } else {
        inCorrect.value = true;
        incorrect();
      }
    })
    .catch((error) => {
      console.error(error);
    });
}
</script>
<style>
.incorrect.show {
  animation-name: shake, glow-red;
  animation-duration: 0.7s, 0.35s;
  animation-iteration-count: 1, 2;
}

@keyframes shake {

  0%,
  20%,
  40%,
  60%,
  80% {
    transform: translateX(8px);
  }

  10%,
  30%,
  50%,
  70%,
  90% {
    transform: translateX(-8px);
  }
}

/* custom pattern https://superdesigner.co/tools/css-backgrounds */
.bg-wave {
  background: radial-gradient(circle at top left,
      transparent 25%,
      #4f46e5 25.5%,
      #4f46e5 36%,
      transparent 37%,
      transparent 100%),
    radial-gradient(circle at bottom right,
      transparent 34%,
      #4f46e5 34.5%,
      #4f46e5 45.5%,
      transparent 46%,
      transparent 100%);
  background-size: 6em 6em;
  opacity: 1;
}
</style>
