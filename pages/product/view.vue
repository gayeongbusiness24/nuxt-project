<template>
    <div class="inner">
        <!-- 상단 -->
        <div class="prd_top flex">
            <div class="thumbnail_wrap">
                <!-- 상품 이미지 슬라이드 -->
                <swiper class="thumb_wrap swiper"
                    :modules="modules"
                    :slidesPerView=1
                    :thumbs="{ swiper: thumbsSwiper }"
                    :pagination="{
                        clickable: true,
                        el: '.swiper_pagination',
                        type: 'bullets',
                    }">
                    <swiper-slide v-for="result in product.imageList"
                        :class="{sold_out : product.sold_out == 'Y' || (product.stock == 0 && product.prdGbn == '1')}">
                        <a class="img_wrap" href="javascript:;">
                            <img v-if="result.imageSrc" :src="runtimeConfig.public.cdnUrl + result.imageSrc"
                                onerror="this.src='/noimage.jpg'" :alt="result.name" class="img_full">
                            <img v-else src="/noimage.jpg" class="img_full">
                        </a>
                    </swiper-slide>
                </swiper>
                <swiper class="thumb_small_wrap swiper"
                    @swiper="setThumbsSwiper"
                    :spaceBetween="10"
                    :slidesPerView="4"
                    :freeMode="true"
                    :watchSlidesProgress="true"
                    :modules="modules">
                    <swiper-slide v-for="result in product.imageList"
                        :class="{sold_out : }"
                        >
                        <a href="javascript:;" class="img_wrap">
                            <img v-if="result.imageSrc" :src="runtimeConfig.public.cdnUrl + result.imageSrc"
                                onerror="this.src='/noimage.jpg'" :alt="result.name" class="img_full">
                            <img v-else src="/noimage.jpg" class="img_full">
                        </a>
                    </swiper-slide>
                </swiper>
                <div class="swiper_btn_wrap">
                    <div class="swiper_pagination text_center"></div>
                </div>
            </div>
            <div class="summery_wrap">
                <div class="summery">
                    <div class="prd_sub">
                        <div class="flex flex_end flex_a_center">
                            <span>상품코드: {{ product.code }}</span>
                            <a @click.prevent="changeChoice()" href="javascript:;" class="btn_heart btn btn_white">
                                <em class="text_center">{{ choice === true ? '💜' : '🤍' }}</em>
                            </a>
                            <!-- <nuxt-link @click.prevent="changeChoice" :class="choice ? 'iconLikeOn' : 'iconLikeOff'"></nuxt-link> -->
                            <a @click.prevent="fnShare" class="btn_share" href="javascript:void(0);" title="공유하기"></a>
                        </div>
                        <span class="icon_sale" v-if="product.price < product.orgprice"></span>
                        <span v-if="product.prdIntro" class="prd_desc ellipsis_2">{{ he.decode(product.prdIntro) }}</span>
                        <strong v-if="product.name" class="prd_name ellipsis_2">{{ he.decode(product.name) }}</strong>
                    </div>
                </div>
                <div class="detail_wrap">
                    <div class="price_wrap flex_between_position">
                        <span class="det_tit">판매가</span>
                        <div class="flex flex_a_end">
                            <span v-if="product.orgprice > product.price" class="percent color_point"><b>{{
                                Math.round(100 - (product.price / product.orgprice) * 100) }}%</b></span>
                            <span class="price">
                                <span v-if="product.orgprice > product.price" class="origin_price">{{
                                    formPrice(product.orgprice) }} 원</span>
                                <b>{{ formPrice(product.price) }}</b> 원
                            </span>
                        </div>
                    </div>
                    <div class="delivery flex_between_position">
                        <span class="det_tit">배송방법</span>
                        <div v-if="product.prdType == 0">택배</div>
                        <div v-else>방문</div>
                    </div>
                    <div class="delivery flex_between_position">
                        <span class="det_tit">배송비</span>
                        <div>
                            <div v-if="product.newDeliveryType !== null">
                                <div v-if="product.newDeliveryType == '0' || product.newDeliveryType == '1'">
                                    <div v-if="product.newDeliveryType == '0' && product.dpId == null">
                                        <span v-if="company && company.newDeliveryPrice"><b>{{ company.newDeliveryPrice
                                        }}</b>원</span>
                                        <span v-else>무료</span>
                                    </div>
                                    <div v-else-if="newDelivery[0].deliveryPrice === 0">
                                        <span>무료</span>
                                    </div>
                                    <div v-else>
                                        <span><b>{{ newDelivery[0].deliveryPrice | formatNumber }}</b>원</span>
                                    </div>
                                </div>
                                <div v-else-if="product.newDeliveryType === '2'">
                                    <span>무료</span>
                                </div>
                            </div>
                            <div v-else>
                                <div v-if="">
                                    <span><b>{{ companyResult.deliveryPrice | formatNumber }}</b>원</span>
                                </div>
                                <div v-else-if="">
                                    <span>무료</span>
                                </div>
                                <div v-else-if="">
                                    <span>착불</span>
                                </div>
                                <div v-else-if="">
                                    <span>배송비 고정 / <b>{{  }}</b>원</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- 고객 만족도 -->
                <div v-if="" class="user_share">
                    <table>
                        <colgroup>
                            <col width="54%">
                            <col width="23%">
                            <col width="23%">
                        </colgroup>
                        <tbody>
                            <tr>
                                <td colspan="3">
                                    <span class="td_title">고객 만족도</span>
                                    <span
                                        :class="{ active:  }"></span>
                                    <span class="star_number">{{  }}</span>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <!-- 구매하기 버튼 -->
                <div :class="isActive01 === true ? 'show' : 'none'" class="mo_pop">
                    <div class="mo_pop_inn">
                        <a @click.prevent="fnToggleModal" href="javascript:void(0);" class="btn_close" id="pro_btn" title="닫기"></a>
                        <div class="select_inn" :class="{ none:  }">
                            <!-- 필수옵션 -->
                            <div v-if="product.requireOptList.length > 0" class="option_wrap flex flex_between">
                                <span an class="select_content_tit">필수옵션</span>
                                <div>
                                    <div v-for="(result, index) in product.requireOptList" class="option_list">
                                        <input type="text" @click="showOptionList(result.optionId, '필수옵션')"
                                            readonly="readonly" :placeholder="result.optionDisplayNm">
                                    </div>
                                </div>
                            </div>
                            <!-- //필수옵션 -->
                            <!-- 선택옵션 -->
                            <div v-if="product.selectOptList2 && product.selectOptList2.length > 0"
                                class="option_wrap flex flex_between">
                                <span class="select_content_tit">선택옵션</span>
                                <div v-for="(result, index) in product.selectOptList2" class="option_list">
                                    <input type="text" @click="showOptionList(result.optionId, '선택옵션')" readonly="readonly"
                                        :placeholder="result.optionDisplayNm">
                                </div>
                            </div>
                            <!-- //선택옵션 -->
                            <!-- 텍스트옵션 -->
                            <div v-if="product.textOption" class="flex_between_position">
                                <span class="select_content_tit pt0">텍스트옵션</span>
                                <div>
                                    <p class="select_content_txt pt0" id="optionDescription_">{{ product.textOption }}</p>
                                    <input type="text" class="textOption x_1" value="" id="textOption_">
                                    <a href="javascript:;" class="btn btn_light">추가</a>
                                </div>
                            </div>
                            <!-- //텍스트옵션 -->
                        </div>
                        <!-- 필수/선택옵션 -->
                        <div class="box_wrap">
                            <div v-show="optionList.length > 0"
                                v-for="(option, index) in optionList.slice(0, 1)" class="box">
                                <h5 class="option_category text_bold flex_between_position">
                                    {{ option.optionDisplayNm }}<a @click.prevent="optionList = []" class="btn_close" title="닫기"></a>
                                </h5>
                                <!-- 10개 이상일때 검색 옵션 -->
                                <div class="search_area" style="display: none;">
                                    <input type="text" value="" class="option_search" id="option_search_61683"
                                        placeholder="옵션을 검색해주세요." onkeyup="searchOption(this)">
                                    <a href="javascript:;" onclick="javascript:;"></a>
                                </div>
                                <!-- //10개 이상일때 검색 옵션 -->
                                <div class="scroll_y">
                                    <ul v-for="(result, index) in option.optionDetailList" class="selectoption_box_list">
                                        <li class="sold_out flex_between_position"
                                            v-if="">
                                            <a>{{ result.optionCount }} (품절)</a>
                                            <span class="option_price">{{ formPrice(result.opt2Price) }}원</span>
                                        </li>
                                        <li v-else class="flex_between_position">
                                            <a>{{ result.optionCount }}</a>
                                            <span class="option_price">{{ formPrice(result.opt2Price) }}원</span>
                                        </li>
                                    </ul>
                                </div>
                            </div>
                            <!-- 옵션의 텍스트옵션 -->
                            <div v-if="" class="box">
                                <h3>
                                    텍스트옵션
                                    <span class="on">
                                        <a @click.prevent="" class="icon_delete_btn"
                                            title="닫기"></a>
                                    </span>
                                </h3>
                                <div class="select_content_box textOption_box textOption_box">
                                    <p class="select_content_txt pt0">{{  }}</p>
                                    <input type="text" v-model="" class="textOption">
                                    <a @click.prevent="addTextOption" class="textoption_btn">추가</a>
                                </div>
                            </div>
                            <!-- 선택한 옵션 상세 목록 -->
                            <div class="selected_list">
                                <ul class="scroll_y">
                                    <li v-for="(result, index) in choiceProductList" :key="result" class="box">
                                        <div class="on">
                                            <div class="price">
                                                <div class="price_change_btn">
                                                    <div class="flex_between_position">
                                                        <strong class="selected_option" v-if="result.optionDisplayNm"> {{
                                                            result.optionDisplayNm }} : {{ result.selectOptionText }}</strong>
                                                        <a v-if="result.optionDisplayNm"
                                                            @click.prevent="choiceProductList.splice(index, 1), calculate()" class="btn_close" title="닫기"></a>
                                                    </div>
                                                    <div class="textuser_option_2" v-if="result.option2TextUserInput">텍스트옵션
                                                        : {{ result.option2TextUserInput }}</div>
                                                    <div class="selected_option"> 배송방법 : 택배</div>
                                                    <div class="selected_option flex flex_a_center flex_between">
                                                        <div class="count_inner flex flex_a_center">
                                                            <a @click.prevent="result.option2Cnt > 1 ? minus(result) : ''"
                                                                class="btn_minus" title="빼기">minus</a>
                                                            <input @input="onChange($event, result)"
                                                                v-model="result.option2Cnt" type="text" name="option2Cnt"
                                                                value="1" class="optionAmount">
                                                            <a @click.prevent="result.option2Cnt >= 1 && result.option2Cnt < result.amount ? plus(result) : ''"
                                                                class="btn_plus" title="더하기">plus</a>
                                                        </div>
                                                        <span class="option_price"><b>{{ formPrice(result.option2Cnt * result.opt2Price) }}</b> 원</span>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </li>
                                </ul>
                            </div>
                        </div>
                        <!-- total price -->
                        <div class="total_price flex flex_a_center flex_between">
                            <span class="total_inner flex flex_a_center">총 상품 금액
                                <em @click="isClicked = !isClicked" class="btn_notice" title="주의사항">?</em>
                                <div v-show="isClicked" class="box_notice">총 상품금액에
                                    <em class="color_point">배송비는 포함되어 있지 않습니다.</em> 결제 시 배송비가 추가될 수 있습니다.
                                </div>
                            </span>
                            <span><b class="color_point">{{ formPrice(totalPrice) }}</b> 원</span>
                        </div>
                        <!-- //total price -->
                        <div v-if="product" class="btn_wrap btn_wrap_lg">
                            <a @click.prevent="changeChoice()" href="javascript:;" class="flex_1 btn_heart btn btn_white">
                                <em class="text_center" title="찜하기">{{ choice === true ? '💜' : '🩶' }}</em>
                            </a>
                            <a @click.prevent="cart('product')" class="flex_1 btn btn_white"
                                v-if="product.sold_out ==! 'Y'">장바구니</a>
                            <a v-if="product.sold_out == 'Y' || (product.stock == 0 && product.prdGbn == '1')"
                                class="flex_1 btn btn_gray disabled">품절</a>
                            <a v-else @click.prevent="pay('product')" class="flex_1 btn btn_dark">구매하기</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- //상단 -->
        <!-- 하단 -->
        <div class="prd_bottom">
            <div class="tab">
                <ul class="flex">
                    <li v-for="(result, index) in tabs" :class="{ active: currentTab === index }" class="flex_1">
                        <a @click.prevent="fnTab(index)" class="text_center">
                            {{ result }}
                        </a>
                    </li>
                </ul>
            </div>
            <div class="view_cont">
                <!-- 1: 상품정보 -->
                <div v-show="currentTab == 0" class="tab_content">
                    <div class="detail_view_inner"
                        :class="isToggled ? 'view_detail_on' : 'view_detail_off'">
                        <div v-html="getMFlag() == 'M' ? product.mprdmemo : product.prdmemo"></div>
                        <div v-html="getMFlag() == 'M' ? product.mprodtxt : product.prodtxt"></div>
                    </div>
                    <div class="btn_wrap btn_wrap_lg">
                        <a @click.prevent="isToggled = !isToggled" :class="isToggled ? 'hide_info_btn' : 'more_info_btn'"
                            class="btn btn_white" href="javascript:;">상품정보 {{ !isToggled ? '더보기' : '접기' }}</a>
                    </div>
                    <!-- 함께 비교하면 좋은 상품 -->
                    <div class="hashtag_wrap box">
                        <h3 class="title_type04">관련 해시태그</h3>
                        <div class="keyword_category flex flex_wrap">
                            <a @click.prevent="fnAddSearchKeyword(result)" v-for="result in product.tagList"
                                class="btn btn_white">#{{ he.decode(result) }}</a>
                            <p v-if="!product.tagList" class="no_data">관련 해시태그가 존재하지 않습니다.</p>
                        </div>
                    </div>
                    <!-- //함께 비교하면 좋은 상품 -->
                </div>
                <!-- //1: 상품정보 -->
                <!-- 2: 구매정보 -->
                <div v-show="currentTab == 1" class="tab_content">
                    <div class="detail_view_inner">
                        <div v-html="getMFlag() == 'M' ? product.mdelimemo : product.delimemo"></div>
                    </div>
                    <!-- 판매자정보 서비스 세트일경우만 안나옴 -->
                    <!-- 이용안내 및 판매점정보 -->
                    <div class="detail_view_inner">
                        <div class="box">
                            <div v-if=""></div>
                            <nuxt-link :to="" target="_blank">
                                <h4 class="title_type04">판매자정보</h4>
                            </nuxt-link>
                            <table v-if="company" class="table">
                                <tbody>
                                    <tr>
                                        <th>가맹점명</th>
                                        <td>{{  }}</td>
                                    </tr>
                                    <tr>
                                        <th>대표자명</th>
                                        <td>{{  }}</td>
                                    </tr>
                                    <tr>
                                        <th>사업자 번호</th>
                                        <td>{{  }}</td>
                                    </tr>
                                    <tr>
                                        <th>통신판매신고번호</th>
                                        <td>{{  }}</td>
                                    </tr>
                                    <tr>
                                        <th>주소</th>
                                        <td>{{  }} {{  }}</td>
                                    </tr>
                                    <tr>
                                        <th>대표번호</th>
                                        <td>
                                            {{  }}
                                            <em
                                                v-if="">(
                                                {{  }} )</em>
                                            <em v-else>( 24시간 )</em>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
                <!-- //2: 구매정보 -->
                <!-- 3: 상품후기 -->
                <div v-show="currentTab == 2" class="tab_content text_center">
                    <div class="review_total box">
                        <h5 class="title_type04 border_bottom text_bold text_left">상품후기 (<b class="">1</b>)</h5>
                        <!-- 상품후기 이미지 목록 -->
                        <div class="review_total_list_wrap">
                            <ul v-if="reviewImgList.length > 0" class="flex">
                                <li v-for="(result, index) in reviewImgList" :key="result" class="review_total_list flex_1">
                                    <a @click.prevent="fnViewImg()" href="javascript:;" class="img_wrap">
                                        <img :src="result.imageSrc" :alt="result.name" class="img_full">
                                    </a>
                                </li>
                            </ul>
                            <p v-else class="no_data">등록된 포토 후기가 없습니다.</p>
                        </div>
                        <!-- 상품 후기 목록 -->
                        <div class="result_list_container">
                            <ul v-if="reviewList.length > 0" class="result_list_wrap">
                                <li v-for="(result, index) in reviewList" :key="result"
                                    :class="{on: currentActive === index}" class="flex result_list">
                                    <div class="result_list_left flex flex_column text_left">
                                        <span class="id text_bold">{{ result.r.id }}</span>
                                        <span class="date">{{ result.r.date }}</span>
                                    </div>
                                    <div class="result_list_right text_left">
                                        <div class="result_list_right_r flex">
                                            <div class="result_list_text">
                                                <strong class="result_list_title">{{ result.r.title }}</strong>
                                                <div class="result_list_option_wrap flex">
                                                    <div v-if="result.r.option.value !== ''" class="result_list_option_box flex">
                                                        <span v-for="(result01, index01) in result.r.option" :key="index01" class="result_list_option">{{ result01.name }}</span>
                                                        <em>:&nbsp;</em>
                                                        <span v-for="(result01, index01) in result.r.option" :key="index01" class="result_list_option">{{ result01.value }}</span>
                                                    </div>
                                                    <div v-if="result.r.option01.value !== ''" class="result_list_option_box flex">
                                                        <span v-for="(result01, index01) in result.r.option01" :key="index01" class="result_list_option">{{ result01.name }}</span>
                                                        <em>:&nbsp;</em>
                                                        <span v-for="(result01, index01) in result.r.option01" :key="index01" class="result_list_option">{{ result01.value }}</span>
                                                    </div>
                                                </div>
                                                <p class="result_list_desc">
                                                    <a @click.prevent="fnViewR(index)" class="result_list_desc_p" href="javascript:;">{{ result.r.desc }}</a>
                                                    <a @click.prevent="fnViewR(index)" class="btn_more">{{ currentActive !== index ? "더보기" : "접기" }}</a>
                                                </p>
                                            </div>
                                            <div class="result_list_img" v-if="result.r.imageSrc[0] !== ''">
                                                <a @click.prevent="fnViewR(index)" href="javascript:;" class="result_list_thumb_img img_wrap">
                                                    <img :src="result.r.imageSrc[0]" :alt="`${result.r.title} 이미지`" class="img_full">
                                                </a>
                                                <div v-if="result.r.imageSrc.length > 0" class="result_list_img_slide">
                                                    <swiper class="swiper"
                                                        :modules="modules"
                                                        :slidesPerView=1
                                                        :navigation="{
                                                            nextEl: '.swiper-button-next01',
                                                            prevEl: '.swiper-button-prev01',
                                                        }">
                                                        <swiper-slide v-for="(result02, index02) in result.r.imageSrc">
                                                            <a @click.prevent="fnViewR(index)" href="javascript:;" class="img_wrap">
                                                                <img :src="result02" :alt="result.title + index" class="img_full">
                                                            </a>
                                                        </swiper-slide>
                                                    </swiper>
                                                    <div v-if="result.r.imageSrc.length > 1" class="swiper-button-prev01 swiper-button-prev" tabindex="0" title="이전"></div>
                                                    <div v-if="result.r.imageSrc.length > 1" class="swiper-button-next01 swiper-button-next" tabindex="0" title="다음"></div>
                                                </div>
                                            </div>
                                        </div>
                                        <div v-if="result.a.store !== ''" class="result_list_answer">
                                            <div class="result_list_answer_top">
                                                <span class="store text_bold">{{ result.a.store }}</span>
                                                <span class="date">{{ result.a.date }}</span>
                                            </div>
                                            <p class="result_list_answer_desc">{{ result.a.desc }}</p>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                            <p v-else class="no_data">등록된 후기가 없습니다.</p>
                        </div>
                    </div>
                    <div class="paging">
                        <ol class="flex flex_center text_center">
                            <li class="btn_prev"><a href="javascript:;" title="이전"></a></li>
                            <li v-for="(result, index) in pageCount" :class="{on: currentPage === index}" class="btn_number">
                                <a @click.prevent="fnPaging(index)" href="javascript:;">{{ result }}</a>
                            </li>
                            <li class="btn_next"><a href="javascript:;" title="다음"></a></li>
                        </ol>
                    </div>
                </div>
                <!-- //3: 상품후기 -->
                <!-- 4: 상품문의 -->
                <div v-if="product" v-show="currentTab == 3" class="tab_content">
                    <div class="detail_view_inner">
                        <QnaList />
                    </div>
                </div>
                <!-- //4: 상품문의 -->
            </div>
        </div>
        <!-- //하단 -->
    </div>

    <!-- 공유하기 팝업 -->
    <div v-show="isShare" class="modal_share modal">
        <div class="modal_top">
            <h3 class="modal_title text_center">공유하기</h3>
            <a @click.prevent="fnShare" href="javascript:void(0);" class="btn_close" title="닫기"></a>
        </div>
        <div class="modal_content">
            <div class="icon_wrap flex_center_position">
                <input type="hidden" id="copyUrl" value="http://opb.osangtech.co.kr/product/category/product_view2/95767"
                    name="" title="url 입력박스" readonly="">
                <a href="javascript:;" class="icon icon_link" title="링크복사"></a>
                <a href="javascript:;" class="icon icon_kakao" title="카카오톡"></a>
                <a href="javascript:;" class="icon icon_naver" title="네이버"></a>
            </div>
        </div>
    </div>

    <div v-show="imgShown" class="modal_review_img modal">
        <div class="modal_top">
            <h3 class="modal_title text_center">포토후기 보기</h3>
            <a @click.prevent="fnViewImg" href="javascript:void(0);" class="btn_close" title="닫기"></a>
        </div>
        <div class="modal_content">
            <div class="thumb_img_wrap">
                <div class="thumb_big_wrap">
                    <swiper class="thumb_wrap swiper"
                        :modules="modules"
                        :slidesPerView=1
                        :navigation="{
                            nextEl: '.swiper-button-next02',
                            prevEl: '.swiper-button-prev02',
                        }"
                        :thumbs="{ swiper: modalThumbsSwiper }">
                        <swiper-slide v-for="result in reviewImgList" :key="index">
                            <a href="javascript:;" class="img_wrap">
                                <img :src="result.imageSrc" :alt="result.name" class="img_full">
                            </a>
                        </swiper-slide>
                    </swiper>
                    <div @click="handleNav(false)" class="swiper-button-prev02 swiper-button-prev" title="이전"></div>
                    <div @click="handleNav(true)" class="swiper-button-next02 swiper-button-next" title="다음"></div>
                </div>
                <div v-if="imgShown" class="thumb_small_wrap">
                    <swiper class="swiper"
                        @swiper="setModalThumbsSwiper"
                        :spaceBetween="15"
                        :slidesPerView="5"
                        :freeMode="true"
                        :watchSlidesProgress="true"
                        :modules="modules"
                        :slideToClickedSlide="true">
                        <swiper-slide v-for="(result, index) in reviewImgList">
                            <a @click.prevent="handleSlide(index)" href="javascript:;" class="img_wrap">
                                <img :src="result.imageSrc" :alt="result.name" class="img_full">
                            </a>
                        </swiper-slide>
                    </swiper>
                </div>
            </div>
        </div>
    </div>

</template>
<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Thumbs, Pagination, Navigation, FreeMode } from 'swiper';
import 'swiper/css/thumbs';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import 'swiper/css/free-mode';
import 'swiper/css';
import { storeToRefs } from "pinia";
import he from 'he';
const modules = [Thumbs, Pagination, Navigation, FreeMode];
const { session, refresh, update, reset } = await useSession()
let tabs = ref(['상품정보', '구매정보', '상품후기', '상품문의'])
let currentTab = ref(0)
let isToggled = ref(false) // 상세 설명 더보기/접기
let choice = ref(false) // 찜 여부
let isClicked = ref(false)
let isActive01 = ref(false)
let product = ref({ // 상품 정보
    selectOptList: [], // 상품 선택옵션 목록
    requireOptList: [], // 상품 필수 옵션 목록
})
let company = ref(null) // 가맹점 정보
let optionList = ref([]) // 선택한 옵션의 하위 옵션 목록
let totalPrice = ref(0) // 총 금액
let currentActive = ref()
let imgShown = ref(false)
let reviewImgList = ref([
    {
        imageSrc: 'review_01.jpg',
        name: '상품후기 이미지 1',
    },
    {
        imageSrc: 'review_02',
        name: '상품후기 이미지 2',
    },
    {
        imageSrc: 'review_03',
        name: '상품후기 이미지 3',
    },
])
let reviewList = ref([
    {
        r:{
            id: 'aaa***',
            date: '2024.08.08',
            title: '만족합니다!',
            desc: '요즘 털쪄서 좀 작긴 한데 귀엽고 잘어울려요 만족ㅎㅎㅎ 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다. 긴 상품후기의 예시입니다.',
            imageSrc: [
                '//opb.osangtech.co.kr/upload/productImage/productImage_1713011215864.jpg',
                '//opb.osangtech.co.kr/upload/productImage/productImage_1706569704049.jpg',
                '//opb.osangtech.co.kr/upload/productImage/productImage_1710830254880.jpg',
            ],
            option:[
                {
                    name: '컬러',
                    value: '블루'
                },
                {
                    name: '사이즈',
                    value: 'L'
                },
            ],
            option01:[
                {
                    name: '선택옵션1',
                    value: '선택옵션1-1'
                },
                
            ],
        },
        a:{
            store: '강남점',
            date: '2024.07.06',
            desc: '안녕하세요 고객님. 소중한 후기 감사합니다.'
        },

    },
])
let pageCount = ref(['1','2','3','4','5','6','7','8','9','10'])
let currentPage = ref(0)

async function fnTab(index) { // 상세설명/이용안내/상품문의 탭 변경
    currentTab.value = index
}

// 썸네일 슬라이드
const thumbsSwiper = ref(null);
const setThumbsSwiper = (swiper) => {
    thumbsSwiper.value = swiper;
};
// 팝업: 리뷰 이미지 슬라이드
const modalSwiper = ref(null);
const modalThumbsSwiper = ref(null);
const setModalThumbsSwiper = (swiper) => {
    modalThumbsSwiper.value = swiper;
};
// 공유하기
let isShare = ref(false);
function fnShare() {
    isShare.value = !isShare.value;
    isShare.value ? dimShow() : dimHide()
}
// 모바일
function fnToggleModal() {
    isActive01.value = !isActive01.value;
}

function fnViewR(index){
    currentActive.value = currentActive.value === index ? null : index;
}

function fnViewImg(){
    imgShown.value = !imgShown.value;
    imgShown.value ? dimShow() : dimHide();
}

const handleNav = (param) => {
    if (param) {
        modalThumbsSwiper.value.slideNext();
    } else {
        modalThumbsSwiper.value.slidePrev();
    }
};

function fnPaging(index){
    currentPage.value = index
}

</script>


<style scoped src="@/assets/css/product/product.css"></style>
<style scoped src="@/assets/css/product/product_view.css"></style>