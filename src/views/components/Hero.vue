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
                                            <!-- <b-col align-h="center" align-v="center">
                                                <apexchart type=radialBar width=380 :options="prChartOptions" :series="prSeries" />
                                            </b-col> -->
                                            <!-- <b-col align-h="center" align-v="center"> -->
                                            <apexchart type=donut width=380 :options="rejectChartOptions" :series="rejects" />
                                            <!-- <h5 align-h="center">PR: 70</h5> -->
                                            <!-- </b-col> -->
                                        </b-row>
                                        <h4 align-h="center">PR: {{prValue}}</h4>
                                        <br><br>

                                        <b>案件結案效率分佈（百分比）</b>
                                        <b-row align-h="center">
                                            <apexchart type=line :width="windowWidth" height=350 :options="percentChartOptions" :series="percentSeries" />
                                        </b-row>
                                        <br><br>

                                        <b>案件結案效率分佈（件數）</b>
                                        <b-row align-h="center">
                                            <apexchart type=line height=350 :options="chartOptions" :series="series" />
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
import pr from './json/percentile_rank.json'

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

        if (this.isMobile()) {
            var chartWidth = window.innerWidth*0.9;
        } else {
            var chartWidth = window.innerWidth*0.5;
        }

        return {
            pr: pr,
            prValue: 0,
            windowWidth: chartWidth,
            parentWidth: 0,
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
            // prSeries: [67],
            // prChartOptions: {
            //     plotOptions: {
            //         radialBar: {
            //         hollow: {
            //             size: '60%',
            //         }
            //         },
            //     },
            //     labels: ['PR']
            // },
            rejects: [0, 0, 0],
            rejectChartOptions: {
                labels: ['判決維持', '判決撤銷', '部分維持'],
                responsive: [
                    {
                        breakpoint: 480,
                        options: {
                            chart: {
                                width: '100%'
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
                    stacked: false
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
        isMobile: function() {
    	    var check = false;
            (function(a){if(/(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino/i.test(a)||/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substr(0,4))) check = true;})(navigator.userAgent||navigator.vendor||window.opera);
            return check;
        },
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

            if (!this.assessments.hasOwnProperty(this.query)) {
                this.showDismissibleAlert = true;
                this.$bvModal.hide('modal-visualization');
                return
            }

            if (this.pr.hasOwnProperty(this.query)) {
                this.prValue = this.pr[this.query][0];
            } else {
                this.prValue = 'No value';
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
            
            let xAxis = [];
            let yAxis = [];
            if (this.records.hasOwnProperty(this.query)) {
                let arr = this.records[this.query];
                var threshold = Math.round((Math.max(...arr) - 1)/4 - 1);
                var hist = d3.histogram().value(d => d).domain([Math.min(...arr), Math.max(...arr)]).thresholds(threshold);
                let dist = hist(arr);
                dist.forEach(function(cases, index) {
                    xAxis.push((index+1)*4);
                    yAxis.push(cases.length);
                })
            } else {
                [0, 0, 0, 0, 0].forEach(function(cases, index) {
                    xAxis.push((index+1)*4);
                    yAxis.push(0);
                })
            }

            // let arr = this.records[this.query];
            // var threshold = Math.round((Math.max(...arr) - 1)/4 - 1);
            // var hist = d3.histogram().value(d => d).domain([Math.min(...arr), Math.max(...arr)]).thresholds(threshold);
            // let dist = hist(arr);
            // let xAxis = [];
            // let yAxis = [];
            // dist.forEach(function(cases, index) {
            //     xAxis.push((index+1)*4);
            //     yAxis.push(cases.length);
            // })

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
            this.prompt();
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
