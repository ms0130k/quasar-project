<template>
  <h5>OTP</h5>
  <template v-for="(otp, index) in otpTokens" :key="index">
    <div class="row">
      <div class="col-6">{{ otp.label }}: {{ otp.token }}</div>
      <q-btn icon="content_copy" @click="copyToClipboard(otp.token)" />
    </div>
  </template>
  <h5>계정</h5>
  <div class="row">
    <div class="col-6">donghyeonkim@autoeveramerica.com</div>
    <q-btn icon="content_copy" @click="copyToClipboard('donghyeonkim@autoeveramerica.com')" />
    <div class="col-6">비번은 복잡한 것 아니면 1234</div>
    <q-btn icon="content_copy" @click="copyToClipboard('Sosyxal:284%')" />
  </div>
  <hr>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import * as OTPAuth from 'otpauth';
import { copyToClipboard, QBtn } from 'quasar';

const otpInfoList = [
  {
    label: 'IMS(JIRA)',
    secret: 'NO2EYBI3LQTM6RKS',
  },
  {
    label: 'ECM(Confluence)',
    secret: 'PLS34YDHMV3TDHES',
  },
  {
    label: 'heroku',
    secret: 'GCLMM2AVGWAVD7SZCVGXVOUGUMIV7DX2',
  },
  {
    label: 'microsoft',
    secret: 'rzrxzmt55y76ldmk',
  },
];

// OTP 객체 리스트 생성
let totpList = otpInfoList.map(oi => {
  return new OTPAuth.TOTP(Object.assign(oi, {
    algorithm: 'SHA1',
    digits: 6,
    period: 30,
  }));
});

// 반응형 데이터로 토큰 저장
const otpTokens = ref(totpList.map((totp, index) => ({
  label: otpInfoList[index].label,
  token: totp.generate()
})));

// 0.5초마다 토큰 업데이트
let intervalId: any;
onMounted(() => {
  intervalId = setInterval(() => {
    otpTokens.value = totpList.map((totp, index) => ({
      label: otpInfoList[index].label,
      token: totp.generate()
    }));
  }, 500);
});

onUnmounted(() => {
  clearInterval(intervalId);
});
</script>
