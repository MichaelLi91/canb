<template>
    <div class="h-full bg-primary py-4">
        <div class="container">
            <h3 class="text-center text-white">Step 1</h3>
            <div class="py-4"></div>
            <div class="row justify-content-center">
                <div class="col-sm-12 col-lg-10">
                    <div class="card border-0 shadow-sm rounded">
                        <div class="row no-gutters">
                            <div class="col-sm-4 border-right">
                                <div class="card-body">
                                    <strong>デバイス選択</strong>
                                </div>
                                <div class="list-group list-group-sm border-0 mx-0">
                                    <a 
                                        @click.prevent="SelectDevice(index)"
                                        href="javascript.void(0)"
                                        :class="{'active':selectedDevice==index}"
                                        v-for="(category, index) in categories" 
                                        :key="index" class="list-group-item list-group-item-action">
                                        {{category.name}}  <i class="material-icons text-sm float-right">arrow_right</i>
                                    </a>
                                </div>                                
                            </div>
                            <div class="col-sm-8 mt-4 text-center" v-if="selectedDevice == null">
                                <div class="py-4"></div>
                                <h1 class="material-icons display-4">phone_iphone</h1>
                                <h6>デバイスを選択してください</h6>
                            </div>                            
                            <transition name="fade">
                                <div class="col-sm-4 border-right"  v-if="selectedDevice !== null">
                                    <div class="card-body">
                                        <strong>サイズ選択</strong>
                                    </div>
                                    <div class="list-group list-group-sm border-0 mx-0">
                                        <a 
                                            @click.prevent="SelectSize(index)"
                                            href="javascript.void(0)"
                                            :class="{'active':selectedSize==index}"
                                            v-for="(category, index) in categories[selectedDevice].children"
                                            :key="index" class="list-group-item list-group-item-action">
                                            {{category.name}}  <i class="material-icons text-sm float-right">arrow_right</i>
                                        </a>
                                    </div>                                
                                </div>
                             </transition>
                            <div class="col-sm-4 mt-4 text-center" v-if="selectedSize == null && selectedDevice !== null">
                                <div class="py-4"></div>
                                <h1 class="material-icons display-4">aspect_ratio</h1>
                                <h6>デザインパターン選択</h6>
                            </div>
                            <div class="col-sm-4 border-right"  v-if="selectedSize !==null">
                                <div class="card-body">
                                    <strong>デザインパターン選択</strong>
                                </div>
                                <div class="list-group list-group-sm border-0 mx-0">
                                    <a 
                                        @click.prevent="SelectDevelopType('templates')"
                                        href="javascript.void(0)"
                                        :class="{'active':designMode=='templates'}"
                                        class="list-group-item list-group-item-action">
                                        デザインを使う  <i class="material-icons text-sm float-right">arrow_right</i>
                                    </a>
                                    <a 
                                        @click.prevent="SelectDevelopType('design')"
                                        href="javascript.void(0)"
                                        :class="{'active':designMode=='design'}"
                                        class="list-group-item list-group-item-action">
                                        自分でカスタマイズ  <i class="material-icons text-sm float-right">arrow_right</i>
                                    </a>
                                </div>                                
                            </div>
                        </div>
                        <div class="card-footer bg-white">
                        <button @click="Purchase()" class="btn btn-primary float-right" 
                        :disabled="!NextButtonActive">スタート</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import categories from "../../json/devices.json";
export default {
    data() {
        return {
            selectedDevice:null,
            selectedSize:null,
            designMode:null,
            loading:false,
            error:null,
            types:[],
            captures:[],
            categories:categories
        }
    },
    computed: {
        NextButtonActive(){
            let {selectedDevice,selectedSize,designMode} = this;
            if(selectedDevice==null||selectedSize==null||designMode==null)
                return false;
            else
                return true;
            }
    },
    methods: {
        SelectDevice(i){
            this.selectedDevice = i;
            this.selectedSize = null;
        },
        SelectSize(i){
            this.selectedSize = i;
        },
        SelectDevelopType(type){
            this.designMode = type;
        },
        Purchase(){
            let {selectedDevice,selectedSize,designMode,categories} = this;
            let parent  = categories[selectedDevice];
            let child   = parent.children[selectedSize];
            let Route   = designMode == "design" ? `/create/${selectedDevice}/${selectedSize}` : `/templates/${selectedSize}/${selectedSize}`;
            this.$router.push(Route);
        },
    },
}
</script>
