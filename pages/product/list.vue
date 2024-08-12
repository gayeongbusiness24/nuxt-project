<template>
    <div class="inner product_wrap">
        <!-- 상품 목록 -->
        <h2 class="title_type01">상품 목록</h2>
        <div class="list_box under_title">
            <div class="tab">
                <ul v-if="" class="flex flex_wrap">
                    <li :class="{ active:  }">
                      <a class="text_center" href="javascript:void(0);">전체</a>
                    </li>
                    <li v-for="(elem, index) in " :class="{ active:  }">
                        <a class="text_center" href="javascript:void(0);">{{ elem.name }}</a>
                    </li>
                </ul>
            </div>
            <div class="prd_list_wrap">
                <div class="prd box">
                  <h4 class="prd_box_title">추천 상품</h4>
                  <div class="prd_in">
                    <div v-show="currentTab == 0" class="recommend_list">
                        <swiper
                            class="swiper"
                            :modules="modules"
                            :observer=true
                            :observerParents=true
                            :navigation="{
                                nextEl: '.swiper-button-next01',
                                prevEl: '.swiper-button-prev01',
                            }"
                            :scrollbar="{
                                el: '.swiper-scrollbar01',
                                draggable: true,
                            }"
                            :breakpoints="{
                                '100':{
                                    slidesPerView: 'auto',
                                    slidesPerGroup: 1,
                                    spaceBetween:15,
                                },
                                '481':{
                                    slidesPerView: 'auto',
                                    slidesPerGroup: 1,
                                    spaceBetween:20,
                                },
                                '1025':{
                                    slidesPerView: 4,
                                    slidesPerGroup: 4,
                                    spaceBetween:25,
                                },
                            }"
                            @swiper="onSwiper01">
                            <swiper-slide v-for="(elem, index) in prdList"
                              :class="{soldout : elem.soldout === 'Y'}" class="product_li">
                                <a :href="elem.link">
                                    <div class="img img_wrap">
                                        <img
                                          v-if=""
                                          :src=""
                                          :alt="elem.name" class="img_full"
                                        />
                                        <img v-else src="/noimage.jpg" class="img_full" />
                                        <span class="thumbnail_bg"></span>
                                    </div>
                                    <div class="detail">
                                        <span class="prd_category">공급사명</span>
                                        <span class="prd_name">{{ he.decode(elem.name) }}</span>
                                        <div class="price_all">
                                            <span v-if="elem.orgprice > elem.price" class="org_price">{{ formPrice(elem.orgprice) }} 원</span>
                                            <div class="flex flex_a_center">
                                                <span v-if="elem.orgprice > elem.price" class="percent">{{ Math.round( 100 - (elem.price / elem.orgprice) * 100, ) }}%</span>
                                                <span class="price"><b>{{ formPrice(elem.price) }}</b> 원</span>
                                            </div>
                                        </div>
                                    </div>
                                </a>
                            </swiper-slide>
                        </swiper>
                        <div v-show="prdList.length > 1">
                            <div tabindex="0" class="swiper_button_inside swiper-button-prev01 swiper-button-prev" title="이전"></div>
                            <div tabindex="0" class="swiper_button_inside swiper-button-next01 swiper-button-next" title="다음"></div>
                            <div class="swiper-scrollbar01 swiper-scrollbar"></div>
                        </div>
                    </div>
                  </div>
                </div>
                <div class="prd box">
                  <h4 class="prd_box_title">{{  }} 카테고리에 <b class="color_point">{{  }}</b>개의 상품이 있어요.</h4>
                  <div class="summery flex_between_position">
                      <div class="count">전체
                          <b class="color_point text_bold">[{{  }}]</b>
                      </div>
                      <div class="flex">
                          <!-- select -->
                          <div class="select">
                              <span @click="fnselectOpt" class="selected_opt">{{ selectedOptLabel }}</span>
                              <ol v-show="showSel_0" class="select_group scroll_y">
                                  <li v-for="(option, index) in optList00" :key="option.value" :class="{on: selectedVal === index}" class="option">
                                      <input type="radio" :id="option.value" :value="option.value" v-model="selectedVal" @change="">
                                      <label :for="option.value">{{ option.label }}</label>
                                  </li>
                              </ol>
                          </div>
                          <div class="select">
                              <span @click="fnselectOpt01" class="selected_opt">{{ selectedOptContainer }}</span>
                              <ol v-show="showSel_1" class="select_group scroll_y">
                                  <li v-for="(option, index) in optList01" :key="option.value" :class="{on: selectedVal01 === index}" class="option">
                                      <input type="radio" :id="option.value" :value="option.value" v-model="selectedVal01" @change="">
                                      <label :for="option.value">{{ option.label }}</label>
                                  </li>
                              </ol>
                          </div>
                          <!-- //select -->
                      </div>
                  </div>
                  <div class="prd_in">
                    <ul
                        v-show="currentTab == 0"
                        v-if=""
                        class="flex flex_wrap"
                    >
                      <li v-for="(elem, index) in elemList"
                          :class="{soldout : elem.soldout === 'Y'}" class="product_list_li">
                          <a href="javascript:void(0);">
                              <div class="img img_wrap">
                                  <img
                                    v-if=""
                                    :src=""
                                    :alt="elem.name" class="img_full"
                                  />
                                  <img v-else src="/noimage.jpg" class="img_full" />
                                  <span class="thumbnail_bg"></span>
                              </div>
                              <div class="detail">
                                  <span class="prd_category">공급사명</span>
                                  <span class="prd_name">{{ he.decode(elem.name) }}</span>
                                  <div class="price_all">
                                      <span
                                        v-if="elem.orgprice > elem.price"
                                        class="org_price"
                                        >{{ formPrice(elem.orgprice) }} 원</span
                                      >
                                      <div class="flex flex_a_center">
                                        <span v-if="elem.orgprice > elem.price" class="percent">{{ Math.round( 100 - (elem.price / elem.orgprice) * 100, ) }}%</span>
                                        <span class="price"><b>{{ formPrice(elem.price) }}</b> 원</span>
                                      </div>
                                  </div>
                              </div>
                          </a>
                      </li>
                    </ul>
                    <ul v-show="currentTab == 0" v-else class="flex flex_wrap">
                        <div class="no_data">
                            <span>상품이 없습니다.</span>
                        </div>
                    </ul>
                  </div>
                    <div class="paging">
                        <ol class="flex flex_center text_center">
                            <li class="btn_prev"><a href="javascript:;" title="이전"></a></li>
                            <li v-for="(elem, index) in pagingList" :class="{on: currentPaging === index}" class="btn_number">
                                <a @click.prevent="fnPage(index)" href="javascript:;">{{ elem }}</a>
                            </li>
                            <li class="btn_next"><a href="javascript:;" title="다음"></a></li>
                        </ol>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script setup>
let currentTab = ref(0);
import he from 'he'
import { Navigation, Scrollbar } from 'swiper'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css/navigation'
import 'swiper/css/scrollbar'
import 'swiper/css'
const modules = [Navigation, Scrollbar]

let pagingList = ref(['1','2','3','4','5'])
let currentPaging = ref(0)
let prdList = ref([
    {
        imageSrc: '',
        name: '스토어 추천 상품 1',
        price: '30000',
        orgprice: '45000',
    },
    {
        imageSrc: '',
        name: '스토어 추천 상품 2',
        price: '98000',
        orgprice: '108000',
    },
    {
        imageSrc: '',
        name: '스토어 추천 상품 3',
        price: '59800',
        orgprice: '65500',
    },
])
  
const selectedVal = ref(optList00[0].value);
const selectedOptLabel = ref(optList00[0].label);
const selectedVal01 = ref(optList01[0].value);
const selectedOptContainer = ref(optList01[0].label);
const showSel_0 = ref(false);
const showSel_1 = ref(false);

// select box
const optList00 = [
    { value: 0, label: "낮은금액순" },
    { value: 1, label: "높은금액순" },
    { value: 2, label: "인기순" },
    { value: 3, label: "최신순" },
]
const optList01 = [
    { value: 24, label: "24개씩" },
    { value: 48, label: "48개씩" },
    { value: 72, label: "72개씩" },
    { value: 100, label: "100개씩" },
]

function fnSelect(){
    const box = document.querySelector(".select");
    document.addEventListener('mouseup', function(e){
        if(!box.contains(e.target) && box !== e.target){
            showSel_0.value = false;
        }
    });
}
function fnselectOpt(){
    showSel_0.value = !showSel_0.value;
    fnSelect();
}
function fnselectOpt01(){
    showSel_1.value = !showSel_1.value;
    fnSelect();
}
function fnPage(index){
    currentPaging.value = index
}
  
</script>
  
<style scoped src="@/assets/css/product/product.css"></style>