<template>
    <div>
        <div>
            <h1>Fruit Sorter</h1>
        </div>
        <div class="DropZone">
            <dropzone id="foo" ref="el"  :options="options" ></dropzone>
        </div>
        <div class="FormHolder">
            <form class="Form"   @submit="sendUrl" method="get">

                    <label >Or submit a url</label>
                        <input type="text" name="url" id="url" v-model="url" />
                    <input type="submit" value="Submit">


            </form>
        </div>

    </div>

</template>

<script>
    import axios from 'axios';
    import Dropzone from 'nuxt-dropzone'
    import 'nuxt-dropzone/dropzone.css'
    export default {

        name: "MainUpload",
        components:{
            Dropzone
        },
        data(){
            return{
                url:null,
               options: {
                    url: 'https://fruit-recognizer.herokuapp.com/upload',
                   maxFiles:5,
            headers:{
                    "Cache-Control": "null",
                    "Access-Control-Allow-Origin": "*",
                    "Access-Control-Allow-Headers": "X-Requested-With, Content-Type",
                    "Access-Control-Allow-Methods": "POST, GET"
                    }
                },

            }
        },
        mounted() {
            const instance = this.$refs.el.dropzone
        },
        methods:{
            sendUrl(e){
                if(this.url){
                    axios.get('https://fruit-recognizer.herokuapp.com/classify-url?url='+this.url)
                }
                else{
                   console.log("url empty")
                }
                e.preventDefault()
            }
        }
    }
</script>

<style lang="scss" scoped>
.FormHolder{
    display: flex;
    justify-content: center;
   width: 100%;
    .Form{
        display: inline-flex;
        flex-wrap: wrap;
        gap: 20px;
    }
}
</style>
