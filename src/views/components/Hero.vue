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
            title="分析">
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
                                    <b>資料處理完成</b>
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
                <b-button size="md" variant="white" @click="prompt()">
                    Test
                </b-button>
            </template>
        </b-modal>
    </section>
</template>
<script>
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue'

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

import loading from "./Loading.vue";

Vue.use(BootstrapVue)

export default {
    data: function() {
        return {
            loading: true,
            loaded: false
        }
    },
    methods: {
        prompt() {
            this.loading = false;
            var self = this;
            setTimeout(function(){
                self.loaded = true;
            }, 1000);
        }
    },
    mounted() {
        this.$root.$on('bv::modal::show', (bvEvent, modalId) => {
            console.log('Modal is about to be shown')
        })
        this.$root.$on('bv::modal::shown', (bvEvent, modalId) => {
            console.log('Modal is shown')
            this.prompt()
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
