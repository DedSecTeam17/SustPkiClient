<template>

    <div>


        <div class="row mt-4">

            <div class="col-md-4 offset-md-4">

                <div class="card">

                    <div class="card-body">
                        <div class="needs-validation">
                            <form @submit.prevent="submit" class="needs-validation" novalidate>
                                <div class="form-group">

                                    <input :class="{'is-invalid':$v.group_name.$error,'is-valid':!$v.group_name.$invalid}"
                                           v-model.trim="$v.group_name.$model" class="form-control"
                                           placeholder="group name"
                                           id="g_name" name="g_name"
                                           type="text">
                                    <p class="invalid-feedback" v-if="!$v.group_name.required">Group name required</p>
                                </div>
                                <div class="form-group">

                                    <input :class="{'is-invalid':$v.group_pk.$error,'is-valid':!$v.group_pk.$invalid}"
                                           v-model.trim="$v.group_pk.$model" class="form-control"
                                           placeholder="public key"
                                           id="g_pk" name="g_pk"
                                           type="text">
                                    <p class="invalid-feedback" v-if="!$v.group_name.required">Group name required</p>
                                </div>


                                <button type="submit" class="btn btn-info btn-block  text-center">
                                    <span v-if="this.isLoading" class="spinner-border text-light" role="status">
                                        <span class="sr-only">Loading...</span>
                                    </span>
                                    <span v-else>Update</span>

                                </button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

</template>

<script>
    import {required} from 'vuelidate/lib/validators'
    import axios from 'axios';
    import {UserSession} from "../../services/user_session";


    export default {
        name: "edit",
        data() {
            return {
                email: '',
                group_name: '',
                group_pk: '',
                showError: false,
                error_data: "",
                isLoading: false
            }
        },
        validations: {

            group_name: {
                required
            },
            group_pk: {
                required
            },


        },
        created() {
            this.getData();
        },
        methods: {
            submit() {

                this.$v.$touch()
                if (this.$v.$invalid) {
                    // console.log('invalid')
                } else {
                    // console.log('valid')
                    this.isLoading = true
                    this.editGroup();
                }
            },
            getData() {
                axios.get(`https://sustkeys.herokuapp.com/sust_keys/${this.$route.params["group_id"]}`, {
                    headers: {
                        "Authorization": `jwt ${UserSession.getUserToken()}`
                    }
                }).then((res) => {

                    this.group_name = res.data['group_name'];
                    this.group_pk = res.data['group_public_key'];


                })
                    .catch((err) => {
                        console.log(err)
                    })
            },
            goBack() {
                window.history.back();
            },
            editGroup() {
                axios.put(`https://sustkeys.herokuapp.com/sust_keys/${this.$route.params["group_id"]}`,

                    {
                        "group_public_key": this.group_pk, "group_name": this.group_name

                    }, {
                        headers: {
                            "Authorization": `jwt ${UserSession.getUserToken()}`
                        }
                    },).then((res) => {
                    this.$toasted.info("Group updated successfully", {duration: 60 * 60, position: 'top-center'})

                    console.log(res);
                    this.goBack();
                }).catch((err) => {
                    console.log(err)
                })

            }
        }
    }
</script>

<style scoped>

</style>
