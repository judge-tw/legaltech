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
                                        addon-left-icon="fa fa-search">
                            </base-input>
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
                        <a href="https://github.com/RichoHan/legaltech_hackathon/issues" target="_blank" title="Support us on Github">
                            <img src="img/brand/github-white-slim.png" style="height: 22px; margin-top: -3px">
                        </a>
                    </div>
                    <div class="col-sm-4 mt-4 mt-sm-0 text-right">
                        <span class="text-white alpha-7">Coded by</span>
                        <a href="https://www.creative-tim.com" target="_blank" title="Creative Tim - Premium Bootstrap Themes and Templates">
                            <img src="img/brand/creativetim-white-slim.png" class="ml-3" style="height: 30px;">
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
            title="分析結果">
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

                                        <b>案件結案效率分佈</b>
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
            <template slot="modal-footer" slot-scope="{ ok, cancel }">
                <b-button size="md" variant="danger" @click="cancel()">
                    離開
                </b-button>
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

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

import loading from "./Loading.vue";

Vue.use(BootstrapVue)
Vue.component('apexchart', VueApexCharts)


export default {
    data: function() {
        return {
            loading: true,
            loaded: false,
            rejects: [25, 75],
            rejectChartOptions: {
                labels: ['駁回判決', '維持判決'],
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
                ]
            },
            series: [
                {
                    name: '個人表現',
                    type: 'column',
                    data: [23, 11, 22, 27, 13, 22, 37, 21, 44, 22, 30, 11]
                }, {
                    name: '整體平均分佈',
                    type: 'line',
                    data: [30, 25, 36, 30, 45, 35, 64, 52, 59, 36, 39, 10]
                }],
            chartOptions: {
                chart: {
                    stacked: false,
                },
                stroke: {
                    width: [0, 2, 5],
                    curve: 'smooth'
                },
                plotOptions: {
                    bar: {
                        columnWidth: '50%'
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
                labels: [
                    3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14
                ],
                markers: {
                    size: 0
                },
                xaxis: {
                    title: {
                        text: '延遲時間區間',
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
                            return y;
                        }
                    }
                }
            }
        }
    },
    methods: {
        prompt() {
            this.loading = false;
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
