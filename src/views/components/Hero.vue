<template>
    <section class="section-hero section-shaped my-0">
        <div class="shape shape-style-1 shape-primary">
            <!-- <span class="span-150"></span>
            <span class="span-50"></span>
            <span class="span-50"></span>
            <span class="span-75"></span>
            <span class="span-100"></span>
            <span class="span-75"></span>
            <span class="span-50"></span>
            <span class="span-100"></span>
            <span class="span-50"></span>
            <span class="span-100"></span> -->
        </div>
        <div class="container shape-container d-flex align-items-center">
            <div class="col px-0">
                <div class="row justify-content-center align-items-center">
                    <div class="col-lg-7 text-center pt-lg">
                        <img src="img/brand/judge.png" style="width: 200px;" class="img-fluid">
                        <h3 class="display-4 text-white mt-4 mb-5">法客訟專案展示</h3>
                        <form role="form">
                            <base-input alternative
                                        class="mb-3"
                                        placeholder="姓名"
                                        addon-left-icon="fa fa-search"
                                        v-model="query"
                                        v-on:keyup.enter="onEnter">
                            </base-input>
                            <b-alert
                                style="margin-top: 1em"
                                variant="danger"
                                dismissible
                                fade
                                :show="showDismissibleAlert"
                                @dismissed="showDismissibleAlert=false"
                                >
                                查無此人
                            </b-alert>
                            <div class="text-center">
                                <base-button
                                    v-b-modal.modal-visualization
                                    outline
                                    type="neutral"
                                    class="mb-3 mb-sm-0"
                                    icon="ni ni-atom">
                                    開始分析
                                </base-button>
                            </div>
                        </form>
                        <!-- <div class="btn-wrapper">
                            <base-button tag="a"
                                         href="https://demos.creative-tim.com/vue-argon-design-system/documentation"
                                         class="mb-3 mb-sm-0"
                                         type="info"
                                         icon="fa fa-code">
                                Who are we
                            </base-button>
                            <base-button tag="a"
                                         href="https://www.creative-tim.com/product/vue-argon-design-system"
                                         class="mb-3 mb-sm-0"
                                         type="white"
                                         icon="ni ni-cloud-download-95">
                                Overview
                            </base-button>
                        </div> -->
                    </div>
                </div>
                <div class="row align-items-center justify-content-around stars-and-coded">
                    <div class="col-sm-4">
                        <span class="text-white alpha-7 ml-3">Support us on</span>
                        <a href="https://github.com/judge-tw/legaltech/issues" target="_blank" title="Support us on Github">
                            <img src="img/brand/github-white-slim.png" style="height: 22px; margin-top: -3px">
                        </a>
                    </div>
                    <div class="col-sm-4 mt-4 mt-sm-0 text-right">
                        <span class="text-white alpha-7">資料來源</span>
                        <a href="https://www.lawsnote.com/" target="_blank" title="Lawsnore">
                            <img src="img/brand/lawsnote.svg" class="ml-3" style="height: 30px;">
                        </a>
                        <a href="https://psue.moj.gov.tw/psiqs/" target="_blank" title="Lawsnore">
                            <img src="img/brand/moj.png" class="ml-3" style="height: 32px;">
                        </a>
                    </div>
                </div>
            </div>
        </div>

        <b-modal
            centered
            hide-backdrop
            size="lg"
            id="modal-visualization"
            :title="query">
            <template slot="modal-header">
                <h5>
                    <a :href="'https://sunshine.jrf.org.tw/search/judges?utf8=%E2%9C%93&judge=&q=' + this.query" target="_blank">{{this.query}}</a>
                    法官分析結果
                </h5>
            </template>
            <!-- <b-card
                title="Card Title"
                img-src="https://picsum.photos/600/300/?image=25"
                img-alt="Image"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
            >
                <b-card-text>
                Some quick example text to build on the card title and make up the bulk of the card's content.
                </b-card-text>

                <b-button href="#" variant="primary">Go somewhere</b-button>
            </b-card> -->
            <template slot="default" slot-scope="{ }">
                <b-container fluid class="text-light text-center">
                    <b-row align-h="center">
                        <b-col>
                            <transition name="loading">
                                <div v-if="loading">
                                    <b>資料處理中</b>
                                    <div>
                                        <b-spinner style="width: 3rem; height: 3rem;" type="grow" label="Loading..."></b-spinner>
                                    </div>
                                </div>
                            </transition>
                            <transition name="loading">
                                <div v-if="loaded">
                                    <div>
                                        <b>判決上訴維持率</b>
                                        <b-row align-h="center">
                                            <apexchart type=donut width=380 :options="rejectChartOptions" :series="rejects" />
                                        </b-row>
                                        <br><br>

                                        <b>案件結案效率分佈（百分比）</b>
                                        <b-row align-h="center">
                                            <apexchart type=line width="900px" height=350 :options="percentChartOptions" :series="percentSeries" />
                                        </b-row>
                                        <br><br>

                                        <b>案件結案效率分佈（件數）</b>
                                        <b-row align-h="center">
                                            <apexchart type=line width="900px" height=350 :options="chartOptions" :series="series" />
                                        </b-row>
                                    </div>
                                </div>
                            </transition>
                        </b-col>
                    </b-row>
                </b-container>
            </template>
            <template slot="modal-footer" slot-scope="{ ok }">
                <!-- <b-button size="md" variant="danger" @click="cancel()">
                    離開
                </b-button> -->
                <b-button size="md" variant="success" @click="ok()">
                    重新搜尋
                </b-button>
            </template>
        </b-modal>
    </section>
</template>
<script>
import BootstrapVue from 'bootstrap-vue'
import Vue from 'vue'
import VueApexCharts from 'vue-apexcharts'
import *  as d3 from 'd3'

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

import loading from "./Loading.vue";
import assessments from './json/assessments.json'
import indictments from './json/indictments.json'

Vue.use(BootstrapVue)
Vue.component('apexchart', VueApexCharts)


export default {
    data: function() {
        // let globalArr = [1, 99, 90, 50, 12, 89, 51, 100, 5];

        let globalArr = [1];
        let records = new Object();
        for ( let i=0; i < indictments.length ; i++ ) {
            let indictment = indictments[i];
            var [year, month, day] = indictment['偵結日期'].split('-').map(Number);
            let start = new Date(year+1911, month, day);
            var [year, month, day] = indictment['裁判日期'].split('-').map(Number);
            let end = new Date(year+1911, month, day);
            let interval = Math.round((end - start)/(1000 * 60 * 60 * 24 * 7 * 4));

            globalArr.push(interval);

            indictment['法官'].forEach(function(judge) {
                if ( !records.hasOwnProperty(judge) ) {
                    records[judge] = [interval];
                } else {
                    records[judge].push(interval);
                }
            })
            // let judge = indictment['法官'][0];
            // if ( !records.hasOwnProperty(judge) ) {
            //     records[judge] = [interval];
            // } else {
            //     records[judge].push(interval);
            // }
        }
        // console.log(records);
        // console.log(Object.keys(records).length);

        var threshold = Math.round((Math.max(...globalArr) - Math.min(...globalArr))/4 - 1);
        var hist = d3.histogram().value(d => d).domain([Math.min(...globalArr), Math.max(...globalArr)]).thresholds(threshold);
        // let hist = d3.histogram().value(d => d).thresholds(threshold);
        let globalDist = hist(globalArr);
        let globalXAxis = [];
        let globalYAxis = [];
        globalDist.forEach(function(cases, index) {
            globalXAxis.push((index+1)*4);
            if (index === 0)
                globalYAxis.push(cases.length);
            else
                globalYAxis.push(cases.length);
        })

        let arr = records['劉容妤'];
        // console.log(arr)
        var threshold = Math.round((Math.max(...arr) - 1)/4 - 1);
        var hist = d3.histogram().value(d => d).domain([Math.min(...arr), Math.max(...arr)]).thresholds(threshold);
        let dist = hist(arr);
        // console.log(dist)
        let xAxis = [];
        let yAxis = [];
        dist.forEach(function(cases, index) {
            xAxis.push((index+1)*4);
            if (index === 0)
                // yAxis.push(cases.length -1);
                yAxis.push(cases.length);
            else
                yAxis.push(cases.length);
        })
        // console.log(yAxis)

        return {
            assessments: assessments,
            globalYAxis: globalYAxis,
            dismissSecs: 5,
            dismissCountDown: 0,
            showDismissibleAlert: false,
            loading: true,
            loaded: false,
            indictments: indictments,
            records: records,
            query: '劉容妤',
            rejects: [0, 0, 0],
            rejectChartOptions: {
                labels: ['判決維持', '判決廢棄', '判決部分維持'],
                responsive: [
                    {
                        breakpoint: 480,
                        options: {
                            chart: {
                                width: 300
                            },
                            legend: {
                                position: 'bottom'
                            }
                        }
                    }
                ],
                theme: {
                    mode: 'light',
                    palette: 'palette5',
                    monochrome: {
                        enabled: false,
                        color: '#255aee',
                        shadeTo: 'light',
                        shadeIntensity: 0.65
                    }
                }
            },
            percentSeries: [
                {
                    name: '個人表現',
                    type: 'column',
                    data: yAxis
                }
            ], percentChartOptions: {
                chart: {
                    stacked: false,
                },
                stroke: {
                    width: [0, 5, 5],
                    curve: 'smooth'
                },
                plotOptions: {
                    bar: {
                        columnWidth: '40%'
                    }
                },
                fill: {
                    opacity: [0.85, 0.25, 1],
                    gradient: {
                        inverseColors: false,
                        shade: 'light',
                        type: "vertical",
                        opacityFrom: 0.85,
                        opacityTo: 0.55,
                        stops: [0, 100, 100, 100]
                    }
                },
                labels: xAxis,
                markers: {
                    size: 0
                },
                xaxis: {
                    title: {
                        text: '結案時間區間',
                    },
                    labels: {
                        formatter: function (value) {
                            let year = parseInt(value/12)
                            if (year <= 0)
                                return value%12 + "個月";
                            return parseInt(value/12) + " 年 " + value%12 + " 個月";
                        }
                    }
                },
                yaxis: {
                    title: {
                        text: '百分比',
                    },
                    min: 0
                },
                tooltip: {
                    shared: true,
                    intersect: false,
                    y: {
                        formatter: function (y) {
                            // if (typeof y !== "undefined") {
                            //     return y.toFixed(0) + " 件";
                            // }
                            if (typeof y !== "undefined") {
                                return y + "%";
                            }
                            return y;
                        }
                    }
                },
                theme: {
                    mode: 'light',
                    palette: 'palette2',
                    monochrome: {
                        enabled: false,
                        color: '#255aee',
                        shadeTo: 'light',
                        shadeIntensity: 0.65
                    }
                }
            },
            series: [
                {
                    name: '個人表現',
                    type: 'column',
                    data: yAxis
                }
            ], chartOptions: {
                chart: {
                    stacked: false,
                },
                stroke: {
                    width: [0, 2, 5],
                    curve: 'smooth'
                },
                plotOptions: {
                    bar: {
                        columnWidth: '40%'
                    }
                },
                fill: {
                    opacity: [0.85, 0.25, 1],
                    gradient: {
                        inverseColors: false,
                        shade: 'light',
                        type: "vertical",
                        opacityFrom: 0.85,
                        opacityTo: 0.55,
                        stops: [0, 100, 100, 100]
                    }
                },
                labels: xAxis,
                markers: {
                    size: 0
                },
                xaxis: {
                    title: {
                        text: '結案時間區間',
                    },
                    labels: {
                        formatter: function (value) {
                            let year = parseInt(value/12)
                            if (year <= 0)
                                return value%12 + "個月";
                            return parseInt(value/12) + " 年 " + value%12 + " 個月";
                        }
                    }
                },
                yaxis: {
                    title: {
                        text: '件數',
                    },
                    min: 0
                },
                tooltip: {
                    shared: true,
                    intersect: false,
                    y: {
                        formatter: function (y) {
                            if (typeof y !== "undefined") {
                                return y.toFixed(0) + " 件";
                            }
                            // if (typeof y !== "undefined") {
                            //     return y + "%";
                            // }
                            return y;
                        }
                    }
                },
                theme: {
                    mode: 'light',
                    palette: 'palette8',
                    monochrome: {
                        enabled: false,
                        color: '#255aee',
                        shadeTo: 'light',
                        shadeIntensity: 0.65
                    }
                }
            }
        }
    },
    methods: {
        onEnter: function() {
            this.$bvModal.show('modal-visualization');
        },
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },
        prompt() {
            this.loading = false;

            if (!this.records.hasOwnProperty(this.query)) {
                this.showDismissibleAlert = true;
                this.$bvModal.hide('modal-visualization');
                return
            }

            // Get 維持率
            // console.log(assessments[this.query])
            let rejects = {
                '好': 0,
                '普通': 0,
                '爛': 0,
                '未知': 0
            }
            assessments[this.query].forEach(function(result) {
                // console.log(result);
                if (result.hasOwnProperty("k")) {
                    // console.log(result["k"]);
                    // console.log(result["d"]);
                    rejects[result["k"]] = result["d"];
                }
            });
            // console.log(rejects);
            this.rejects = [rejects['好'], rejects['爛'], rejects['普通']]

            this.showDismissibleAlert = false;

            let arr = this.records[this.query];
            var threshold = Math.round((Math.max(...arr) - 1)/4 - 1);
            var hist = d3.histogram().value(d => d).domain([Math.min(...arr), Math.max(...arr)]).thresholds(threshold);
            let dist = hist(arr);
            let xAxis = [];
            let yAxis = [];
            dist.forEach(function(cases, index) {
                xAxis.push((index+1)*4);
                yAxis.push(cases.length);
            })

            let ySum = yAxis.reduce((a,b)=>a+b);
            let globalYSum = this.globalYAxis.reduce((a,b)=>a+b);  
            let scalesYAxis = [];
            let scaledGlobalYAxis = [];

            yAxis.forEach(function(x) {
                scalesYAxis.push(Number((x/ySum).toFixed(3)));
            })

            this.globalYAxis.forEach(function(x) {
                scaledGlobalYAxis.push(Number((x/globalYSum).toFixed(3)));
            })

            this.series = [
                {
                    name: '個人表現',
                    type: 'column',
                    data: yAxis
                }
            ];
            this.chartOptions['labels'] = xAxis;

            this.percentSeries = [
                {
                    name: '個人表現',
                    type: 'column',
                    data: scalesYAxis
                },
                {
                    name: '整體分佈',
                    type: 'line',
                    data: scaledGlobalYAxis.splice(0, scalesYAxis.length)
                }
            ];
            this.percentChartOptions['labels'] = xAxis;

            var self = this;
            setTimeout(function(){
                self.loaded = true;
            }, 400);
        }
    },
    mounted() {
        this.$root.$on('bv::modal::show', (bvEvent, modalId) => {
        })
        this.$root.$on('bv::modal::shown', (bvEvent, modalId) => {
            this.prompt()
        })
        this.$root.$on('bv::modal::hidden', (bvEvent, modalId) => {
            this.loading = true;
            this.loaded = false;
        })
    }
};
</script>
<style>
.loading-leave { opacity: 1; }
.loading-leave-active { transition: opacity .5s }
.loading-leave-to { opacity: 0; }
.loading-enter { opacity: 0; }
.loading-enter-active  { transition: opacity .5s }
.loading-enter-to { opacity: 1; }
</style>
