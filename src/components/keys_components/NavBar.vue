<template>
    <div v-if="isAuth" class="container-fluid">
        <nav class="navbar navbar-light bg-light">
            <a class="navbar-brand" href="#"><i class="fab fa-keycdn" style="font-size: xx-large"></i></a>
            <button @click="logOut()" class="btn btn-outline-secondary"><i class="fas fa-sign-out-alt"></i> Log out
            </button>
        </nav>
    </div>
</template>

<script>
    import {UserSession} from "../../services/user_session";

    export default {
        name: "NavBar",
        data() {
            return {
                isAuth: UserSession.isAuth()
            }
        },
        methods : {
            logOut(){
                UserSession.destroyToken();
                UserSession.destroyId();

                this.$router.push('/login');
            }
        }
    }
</script>

<style scoped>

</style>
