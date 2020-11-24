<template>
    <dropzone id="foo" ref="el"  :options="options" ></dropzone>
</template>

<script>
    import Dropzone from 'nuxt-dropzone'
    import 'nuxt-dropzone/dropzone.css'
    export default {

        name: "MainUpload",
        components:{
            Dropzone
        },
        data(){
            return{
               options: {
                    url: 'https://fruit-recognizer.herokuapp.com/upload',
                   maxFiles:5,
            headers:{ 'Cache-Control': 'null',
                'X-Requested-With': 'null',
                'Access-Control-Allow-Origin':'*'}



                },

            }
        },
        mounted() {
            const instance = this.$refs.el.dropzone
        }
    }
</script>

<style scoped>

</style>
