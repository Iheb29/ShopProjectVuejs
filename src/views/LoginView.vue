<template>
  <div class="login py-5 mt-5">
    <div class="d-flex mt-5 py-5 p-3 m-4 justify-content-center">
      <div class="col-xl-10 col-md-12 col-lg-12">
        <div class="row shadow-lg rounded">
          <div class="col-lg-6 d-none d-lg-block">
            <img src="../../public/images/blackpants.png" alt="" />
          </div>
          <div class="col-lg-6 py-5">
            <div class="text-center">
              <h1 class="h4 text-gray-900 mb-5">Welcome Back !</h1>
            </div>
            <form @submit.prevent="Login">
              <div v-if="erroemessage != ''" class="alert alert-danger">
                {{ erroemessage }}
              </div>
              <div class="mb-4">
                <input
                  v-model="email"
                  type="text"
                  placeholder="Entrer Email Address..."
                  name="email"
                  class="form-control mb-2"
                  id="exampleInputEmail1"
                />
              </div>
              <div class="mb-5">
                <div class="input-group d-flex">
                  <input
                    v-model="password"
                    :type="type"
                    id="pass"
                    class="form-control mb-2"
                    name="password"
                    placeholder="Entrer Your Password"
                  />
                  <span class="input-group-btn">
                    <button
                      id="changer_visible"
                      class="btn btn-primary"
                      @click="
                        type == 'password'
                          ? (type = 'text')
                          : (type = 'password')
                      "
                      type="button"
                    >
                      <svg
                        v-if="type == 'password'"
                        width="24"
                        height="24"
                        fill="none"
                        viewBox="0 0 24 24"
                        class="eye hidden"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="1.5"
                          d="M19.25 12C19.25 13 17.5 18.25 12 18.25C6.5 18.25 4.75 13 4.75 12C4.75 11 6.5 5.75 12 5.75C17.5 5.75 19.25 11 19.25 12Z"
                        ></path>
                        <circle
                          cx="12"
                          cy="12"
                          r="2.25"
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="1.5"
                        ></circle>
                      </svg>
                      <svg
                        v-else
                        width="24"
                        height="24"
                        fill="none"
                        viewBox="0 0 24 24"
                        class="eyeOff"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="1.5"
                          d="M18.6247 10C19.0646 10.8986 19.25 11.6745 19.25 12C19.25 13 17.5 18.25 12 18.25C11.2686 18.25 10.6035 18.1572 10 17.9938M7 16.2686C5.36209 14.6693 4.75 12.5914 4.75 12C4.75 11 6.5 5.75 12 5.75C13.7947 5.75 15.1901 6.30902 16.2558 7.09698"
                        ></path>
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="1.5"
                          d="M19.25 4.75L4.75 19.25"
                        ></path>
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="1.5"
                          d="M10.409 13.591C9.53033 12.7123 9.53033 11.2877 10.409 10.409C11.2877 9.5303 12.7123 9.5303 13.591 10.409"
                        ></path>
                      </svg>
                    </button>
                  </span>
                </div>
              </div>
              <button
                style="font-size: 19px"
                type="submit"
                name="submit"
                class="btn btn-primary btn-block w-100 rounded-pill mb-5"
              >
                Login
              </button>
            </form>
            <hr />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UserLogin from "../services/service.js";
import { UserStore } from "../store/ServicePinia.js";
export default {
  //composition api
  setup() {
    const store = UserStore();
    return {
      store,
    };
  },
  data() {
    return {
      email: "",
      password: "",
      erroemessage: "",
      type: "password",
      users: [],
    };
  },
  created() {
    UserLogin.getUsers().then((res) => {
      this.users = res.data;
    });
  },
  methods: {
    Login() {
      this.erroemessage = "";
      let IndexUser = this.users.findIndex(
        (u) => u.email == this.email && u.password == this.password
      );
      if (IndexUser == -1) {
        this.erroemessage = "User Not Found";
      } else {
        this.erroemessage = "";
        this.store.AddUser(this.users[IndexUser]);
        let test = new SpeechSynthesisUtterance(
          "welcome " + this.users[IndexUser].nom
        );
        speechSynthesis.speak(test);
        setTimeout(() => {
          this.$router.push({ name: "home" });
          //this.$router.go();
        }, 3000);
        //navigate
      }
      // console.log(IndexUser);
    },
  },
};
</script>

<style>
.fin {
  font-size: 22px;
}
</style>
