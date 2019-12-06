<template>
    <div>

        <button class="btn btn-outline-secondary m-5" v-on:click="createNewGroup()"><i class="fas fa-plus-square"></i>CREATE
            NEW GROUP
        </button>

        <span v-if="this.isLoading" class="spinner-border text-dark" role="status">
                                        <span class="sr-only">Loading...</span>
                                    </span>


        <div v-else class="card">
            <div class="card-body">


                <div class="table-responsive">
                    <table class="table table-hover">
                        <thead>
                        <tr>
                            <th scope="col">Group name</th>
                            <th scope="col">Group public key</th>
                            <th scope="col">DELETE</th>
                            <th scope="col">UPDATE</th>
                        </tr>
                        </thead>
                        <tbody>


                        <tr v-for="group in groups" v-bind:key="group">
                            <td><i class="fas fa-user-friends"></i>{{group['group_name']}}</td>
                            <td><i class="fas fa-key"></i>{{group['group_public_key']}}</td>
                            <td>
                                <button v-if="group['user_id']===user_id" class="btn btn-outline-danger"
                                        @click="deleteGroup(group['_id'])"><i class="fas fa-trash"></i></button>
                            </td>
                            <td>
                                <button v-if="group['user_id']===user_id" class="btn btn-outline-primary"
                                        @click="toEditPage(group['_id'])"><i class="fas fa-edit"></i></button>
                            </td>

                        </tr>

                        </tbody>
                    </table>

                </div>

            </div>
        </div>

    </div>
</template>

<script>
    import axios from 'axios'
    import {UserSession} from "../../services/user_session";

    export default {
        name: "AllKeys",
        data() {
            return {
                groups: [],
                user_id: UserSession.getUserId(),
                isLoading: true
            }
        },
        created() {
            this.getAllData();
        },
        methods: {


            createNewGroup() {
                this.$router.push('/keys/create');
            },

            getAllData() {
                axios.get("https://sustkeys.herokuapp.com/sust_keys").then((res) => {
                    console.log(res)
                    this.groups = res.data;

                    this.isLoading = false;
                })
                    .catch((err) => {
                        this.isLoading = false;

                        console.log(err)
                    });
            },
            deleteGroup(id) {
                this.isLoading = true;
                axios.delete(`https://sustkeys.herokuapp.com/sust_keys/${id}`, {
                    headers: {
                        "Authorization": `jwt ${UserSession.getUserToken()}`
                    }
                },).then((res) => {
                    this.isLoading = false;

                    // this.getAllData();
                    console.log(res);
                }).catch((err) => {
                    this.isLoading = true;

                    this.$toasted.info("Group deleted successfully", {duration: 60 * 60, position: 'top-center'})

                    this.getAllData();
                    console.log(err)
                })

            },
            toEditPage(id) {
                this.$router.push(`/keys/${id}/edit`)
            }
        }
    }
</script>

<style scoped>

</style>
