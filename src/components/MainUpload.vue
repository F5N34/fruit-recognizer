<template>
    <div>
        <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions"
                      v-on:vdropzone-max-files-exceeded="maxFileExceeded"
                      v-on:vdropzone-success="success"></vue-dropzone>


        <reactive-bar-chart :chart-data="chartData" :options="chartOptions"></reactive-bar-chart>
    </div>
</template>

<script>
    import vue2Dropzone from 'vue2-dropzone'
    import 'vue2-dropzone/dist/vue2Dropzone.min.css'
    import ReactiveBarChart from "../ReactiveBarChart.js";

    export default {

        name: "MainUpload",

        components: {
            ReactiveBarChart,
            vueDropzone: vue2Dropzone
        },
        data(){
            return{
                chartData: null,
                result: '',
                dropzoneOptions: {
                    url: 'https://fruit-recognizer.herokuapp.com/upload',
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
                // eslint-disable-next-line no-console
                console.log('max file');
                this.$refs.myVueDropzone.removeAllFiles();
                this.$refs.myVueDropzone.addFile(file);
            },
            success(file, response) {
                // eslint-disable-next-line no-console
                console.log('success');
                this.result = response;
                let data = [];
                let labels = [];
                for (const score of response.scores) {
                    labels.push(score[0]);
                    data.push(score[1]);
                    console.log('pred:' + score[0]);
                    console.log('score:' + score[1]);
                }
                this.chartData = {
                    labels: labels,
                    //labels: [this.getRandomInt(), this.getRandomInt()],
                    datasets: [{
                        label: 'Probabilité',
                        data: data
                    }]
                }
            }
        }
    }
</script>

<style scoped>

</style>
