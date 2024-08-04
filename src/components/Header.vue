<template>
    <header class="header">
        <div class="container">
            <div class="row">
                <div class="d-flex justify-content-center align-items-center">
                    <img src="/logo.png" class="logo" alt="Logo">
                </div>
            </div>
        </div>
    </header>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
    }
</script>

<style lang="scss" scoped>

.header{
    background-color: rgb(44, 197, 198);
    .logo{
        height: 150px;
        @media screen and (max-width: 768px) {
            height: 100px;
        }
        @media screen and (max-width: 468px) {
            height: 80px;
        }
    }
}

</style>