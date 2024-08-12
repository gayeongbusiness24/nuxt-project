<template>
    <div class="main_mall main">
        <!-- 배너 -->
        <BannerCardList groupid="main_top"></BannerCardList>
        <!-- 소식 -->
        <MainNews />
        <!-- 상품 -->
        <MainBrand  groupid="main_prd"/>
        <!-- 띠배너 -->
        <MainLineBanner groupid="main_line" />
        <!-- 가맹점 -->
        <MainStore />
        <!-- 매거진 -->
        <MainMagazine />
    </div>
    <LazyCarSelect></LazyCarSelect>

    <!-- main modal -->
    <div v-show="isModalShow" class="modal_main modal">
        <div class="modal_content">
            <swiper class="main_modal swiper" 
                ref="MainModal" 
                :modules="modules" 
                :centeredSlides=true 
                :spaceBetween=30
                :loop=true 
                :slidesPerView=1 
                :autoplay="{ 
                    delay: 4000, 
                    disableOnInteraction: false,
                 }"
                :pagination="{ 
                    clickable: true, 
                    el: '.main_modal_pagination', 
                    type: 'fraction'
                }"
                :navigation="{ 
                    nextEl: '.swiper-button-next', 
                    prevEl: '.swiper-button-prev', 
                }" 
                @swiper="onSwiper">
                <swiper-slide v-for="result in popup.resultList" :key="result">
                    <a href="javascript:void(0);">
                        <img src="result.imagePath">
                        <div v-html="result.content" />
                    </a>
                </swiper-slide>
                <div class="controll_wrap flex_center_position">
                    <div class="controller">
                        <button @click="fnSwiper" :class="{ toggle: isActive }" class="play_pause"
                            :title="isActive ? '재생하기' : '정지하기'"></button>
                    </div>
                    <div class="main_modal_pagination"></div>
                </div>
                <div class="navigation_wrap flex">
                    <a class="swiper-button-prev" title="이전"></a>
                    <a class="swiper-button-next" title="다음"></a>
                </div>
            </swiper>
        </div>
        <div class="modal_bottom flex_between_position">
            <label class="checkbox"><input type="checkbox">일주일 동안 보지 않기</label>
            <a @click.prevent="closeModal" class="btn btn_white btn_sm">닫기</a>
        </div>
    </div>

    <!-- //main modal -->
</template>

<script setup>
import { Autoplay, Pagination, Navigation } from 'swiper';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css/autoplay';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import 'swiper/css';
import AOS from 'aos';
import 'aos/dist/aos.css';
const modules = [Autoplay, Pagination, Navigation];
let popup = reactive({
    resultList: [],
})
// main modal
let isModalShow = ref(true);
let isActive = ref(false);
const mainModal = ref(null)
AOS.init();
// 스크롤 이벤트 리스너 추가
window.addEventListener('scroll', () => {
    AOS.refresh();
});

const onSwiper = function (swiper) {
    mainModal.value = swiper
}

function fnSwiper() {
    isActive.value = !isActive.value;
    isActive.value ? mainModal.value.autoplay.stop() : mainModal.value.autoplay.start();
}

function closeModal() {
    isModalShow.value = false;
}
</script>

<style src="@/assets/css/main/main.css"></style>
