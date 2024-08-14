<template>
    <div class="inner account_wrap">
		<h2 class="title_type01">로그인</h2>
		<div class="under_title">
            <div class="box">
                <!-- 일반회원 -->
                <div role="tabpanel" class="tab-pane inn_small active">
                    <p class="caption text_center">회원 로그인을 하시면 <br />혜택 및 정보를 제공 받으실 수 있습니다.</p>
                    <table class="login_table">
                        <caption>아이디, 비밀번호 입력 테이블</caption>
                        <colgroup>
                            <col width="99%">
                            <col width="">
                        </colgroup>
                        <tbody>
                            <tr>
                                <td colspan="2">
                                    <input v-model="user_name" id="user_name" name="user_name" class="input_type1 eng_num" type="text" title="이메일 아이디" placeholder="아이디" hname="아이디" @keydown.enter="" required="">
                                </td>
                            </tr>
                            <tr>
                                <td colspan="2">
                                    <input v-model="user_passowrd" id="user_passowrd" name="user_passowrd" class="input_type1" type="password" title="비밀번호" placeholder="비밀번호" required="" hname="비밀번호" @keydown.enter="" >
                                </td>
                            </tr>
                            <tr>
                                <td>
                                    <div class="flex flex_a_center flex_end">
                                        <nuxt-link to="/sign/find/id" class="">아이디 찾기</nuxt-link>
                                        <nuxt-link to="/sign/find/password" class="">패스워드 찾기</nuxt-link>
                                    </div>
                                    <p v-if="" class="alert color_point text_center" id="alertCheck">아이디 또는 비밀번호를 확인하신 후 로그인해주세요.</p>
                                    <p v-if="" class="alert color_point text_center" id="alertCheck">[시스템 오류] 로그인 오류가 발생했습니다.</p>
                                </td>
                            </tr>
                            <tr>
                                <td colspan="2" class="btn_wrap btn_wrap_lg">
                                    <a class="flex_1 btn btn_dark" @click.prevent="" href="javascript:;">로그인</a>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    <div class="sns_wrap">
                        <h5 class="tit text_center">SNS로 간편로그인</h5>
                        <ul class="btn_wrap">
                            <li>
                                <a @click.prevent="fnNaver" href="javascript:;" title="네이버로 간편로그인"></a>
                            </li>
                            <li>
                                <a @click.prevent="fnKakao" href="javascript:;" title="카카오로 간편로그인"></a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
		</div>
		<div class="box">
			<div class="list flex_between_position">
				<div class="dot_list star">
					<span>아직 회원이 아니신가요?
						<br>지금 회원가입을 하시면 다양하고 특별한 혜택과 서비스가 준비되어 있습니다.&ZeroWidthSpace;</span>
				</div>
				<div class="btn_wrap btn_wrap_md">
                    <nuxt-link to="/auth/sign/01" class="btn btn_light">회원가입</nuxt-link>
				</div>
			</div>
		</div>
        <!-- 비회원 -->
        <div class="box">
            <div class="list flex_between_position">
                <p class="dot_list star">
                    <span>비회원 주문조회는 차량용품 구매건에 한해 가능합니다.</span>
                    <span>비회원구매시 입력했던 주문자명, 휴대폰번호를 입력해주세요.</span>
                    <span>주문 비밀번호는 상품 구매 시 결제화면에서 입력하신 비밀번호를 입력해주세요.</span>
                </p>
                <div class="btn_wrap btn_wrap_md">
                    <a @click.prevent="isActive = !isActive" class="btn btn_light" href="javascript:;">비회원 주문조회</a>
                </div>
            </div>
            <div class="non_member_wrap inn_small" :class="{active : isActive}">
                <div class="login_box">
                    <div class="login_id login_mg">
                        <label for="p_username">주문자명</label>
                        <input type="text" v-model="noMem.name" title="주문자명 입력" placeholder="주문자명">
                    </div>
                    <div class="login_pw login_mg">
                        <label for="p_password">휴대폰번호</label>
                        <input type="text" v-model="noMem.call" title="휴대폰번호 입력" placeholder="휴대폰번호" autocomplete="new-password">
                    </div>
                    <div class="login_order_pw login_mg flex flex_a_center">
                        <label for="order_pw">주문비밀번호</label>
                        <input type="password"  v-model="noMem.orderPassword" title="주문비밀번호 입력" placeholder="주문비밀번호" autocomplete="new-password">
                        <a @click.prevent="popShow()" class="order_pw_find btn btn_light" href="javascript:;">주문번호로 찾기</a>
                    </div>
                </div>
                <!-- 주문번호로 찾기 팝업 -->
                <div v-show="store.$state.popShow" class="modal modal_order_history">
                    <div class="modal_top">
                        <h3 class="modal_title text_center">주문번호로 찾기</h3>
                        <a @click.prevent="popHide()" class="btn_close" href="javascript:;" title="닫기"></a>
                    </div>
                    <div class="modal_content">
                        <p class="caption text_center">주문번호는 구매자의 휴대폰 또는 이메일을 확인해주세요.</p>
                        <div class="login_id login_mg">
                            <label for="purchaser_name">주문자명</label>
                            <input type="text" v-model="noMem.name">
                        </div>
                        <div class="ligin_pw login_mg">
                            <label for="tel_num">휴대폰번호</label>
                            <input type="text" v-model="noMem.call">
                        </div>
                        <div class="ogin_order_pw login_mg">
                            <label for="order_pw">주문번호</label>
                            <input type="text" v-model="noMem.orderNum">
                        </div>
                        <div class="error_text color_point" id="error_info">구매자이름, 휴대폰번호, 주문번호를 확인해주세요.</div>
                    </div>
                    <div class="modal_bottom btn_wrap">
                        <button type="submit" class="btn btn_light flex_1">확인</button>
                        <button type="button" @click="popHide()" class="btn btn_white flex_1">취소</button>
                    </div>
                    <!-- //비회원 -->
                </div>
                <!-- //주문번호로 찾기 팝업 -->
                <div class="btn_wrap btn_wrap_lg">
                    <button type="submit" class="flex_1 btn btn_light">주문조회</button>
                </div>
            </div>
        </div>
	</div>
</template>

<script setup>
</script>

<style scoped src="@/assets/css/sign/login.css"></style>
