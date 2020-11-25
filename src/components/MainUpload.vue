<template>
    <div>
        <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions"
                      v-on:vdropzone-max-files-exceeded="maxFileExceeded"
                      v-on:vdropzone-success="success"></vue-dropzone>

        <div class="FormHolder">
            <form class="Form"   @submit="sendUrl" method="get">
                <label >Or submit a url</label>
                <input type="text" name="url" id="url" v-model="url" />
                <input type="submit" value="Submit">
            </form>
        </div>

        <reactive-bar-chart :chart-data="chartData" :options="chartOptions"></reactive-bar-chart>
    </div>
</template>

<script>
    import vue2Dropzone from 'vue2-dropzone'
    import 'vue2-dropzone/dist/vue2Dropzone.min.css'
    import ReactiveBarChart from "../ReactiveBarChart.js";
    import axios from 'axios';
    import {baseUrl} from '../constants';

    export default {

        name: "MainUpload",

        components: {
            ReactiveBarChart,
            vueDropzone: vue2Dropzone
        },
        data(){
            return{
                url: '',
                chartData: null,
                dropzoneOptions: {
                    url: baseUrl + '/upload',
                    thumbnailWidth: 150,
                    maxFiles:1,
                },
                chartOptions: {
                    responsive: true,
                    maintainAspectRatio: false,
                    tooltips: {
                        enabled: true,
                        intersect: false
                    }
                },
            }
        },
        mounted() {
            const instance = this.$refs.myVueDropzone;
        },
        methods: {
            maxFileExceeded(file) {
                this.$refs.myVueDropzone.removeAllFiles();
                this.$refs.myVueDropzone.addFile(file);
            },
            success(file, response) {
                this.updateChart(response)
            },
            updateChart(res) {
                let data = [];
                let labels = [];
                for (const score of res.scores) {
                    labels.push(score[0]);
                    data.push(score[1]);
                }
                this.chartData = {
                    labels: labels,
                    datasets: [{
                        label: 'Probabilité',
                        data: data
                    }]
                }
            },
            sendUrl(e){
                if(this.url){
                    var file = { size: 200, name: "Icon", type: "image/png" };
                    this.$refs.myVueDropzone.removeAllFiles();
                    this.$refs.myVueDropzone.manuallyAddFile(file, this.url);
                    axios.get(baseUrl + '/classify-url?url=' + this.url)
                        .then(
                            response => {
                                this.updateChart(response.data)
                            }
                        )
                }
                else{
                    console.log("url empty")
                }
                e.preventDefault()
            }
        }
    }
</script>

<style scoped>

</style>
