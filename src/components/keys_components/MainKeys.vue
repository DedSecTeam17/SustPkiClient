<template>
    <div >
        <NavBar></NavBar>
        <div class="container-fluid">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
    import NavBar from "./NavBar";
    export default {
        name: "index",
        components: {NavBar}
    }
</script>

<style scoped>

</style>
