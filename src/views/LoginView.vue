<template lang="pug">
.container-fluid
    .row
        .col-sm-12.col-md-8.col-lg-4.vh-100.justify-content-center.align-items-center.d-flex.mx-auto
            Login-Dialog(@login="login")
</template>

<script lang="ts">
import { defineComponent } from "vue";
import LoginDialog from "@/components/LoginAndRegister/LoginDialog.vue";
import type UserCredentials from "@/interfaces/userCredentials";
import { useUserStore } from "@/stores/user";
import Swal from "sweetalert2";
import Toast from "@/lib/swal-mixins/swal-toast";

export default defineComponent({
    name: "LoginView",
    data() {
        return {
            userStore: useUserStore(),
        };
    },
    components: { LoginDialog },
    methods: {
        async login(userCredentials: UserCredentials) {
            try {
                await this.userStore.loginWithCredentials(
                    userCredentials.email,
                    userCredentials.password
                );

                Toast.fire({
                    icon: "success",
                    title: "Logged in.",
                });

                this.$router.push("/");
            } catch (e) {
                console.log(e);
                Swal.fire(
                    "Firebase throwed error.",
                    "Firebase throwed error.",
                    "error"
                );
            }
        },
    },
});
</script>
