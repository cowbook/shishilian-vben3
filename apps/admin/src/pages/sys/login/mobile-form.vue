<script lang="ts" setup>
import { reactive, ref, computed, unref } from 'vue'
import { CountDownInput } from '@vben/components'
import { useI18n } from '@vben/locale'
import {
  useLoginState,
  useFormRules,
  useFormValid,
  LoginStateEnum,
} from './use-login'
import LoginFormTitle from './login-form-title.vue'

const { t } = useI18n()
const { handleBackLogin, getLoginState } = useLoginState()
const { getFormRules } = useFormRules()

const formRef = ref()
const loading = ref(false)

const formData = reactive({
  mobile: '',
  sms: '',
})

const { validForm } = useFormValid(formRef)

const show = computed(() => unref(getLoginState) === LoginStateEnum.MOBILE)

async function handleLogin() {
  const data = await validForm()
  if (!data) return
  console.log(data)
}

async function sendCode() {
  console.log('send!');
  return true;
}



</script>
<template>
  <template v-if="show">
    <login-form-title class="enter-x" />
    <vben-form
      class="rd-2"
      :model="formData"
      :rules="getFormRules"
      ref="formRef"
    >
      <vben-form-item name="mobile" class="enter-x" inline :show-label="false">
        <vben-input
          size="large"
          v-model:value="formData.mobile"
          :placeholder="t('sys.login.mobile')"
          class="fix-auto-fill"
        />
      </vben-form-item>
      <vben-form-item name="sms" class="enter-x" inline :show-label="false">

        <!-- DB: 添加发送短信按钮 -->
        <vben-grid class="enter-x" :cols="3">
          <vben-grid-item :md="8" :xs="24">

            <count-down-input
                size="large"
                :sendCodeApi = "sendCode"
                class="fix-auto-fill"
                v-model:value="formData.sms"
                :placeholder="t('sys.login.smsCode')"
              />
          
          </vben-grid-item>

          <vben-grid-item :md="8" :xs="24">

            <vben-button block @click="setLoginState(LoginStateEnum.QR_CODE)">
              {{t('sys.login.qrSignInFormTitle')}}
            </vben-button>

          </vben-grid-item>
        </vben-grid>
       
      </vben-form-item>

      <div class="enter-x">
        <vben-button
          type="primary"
          size="large"
          block
          @click="handleLogin"
          :loading="loading"
        >
          {{ t('sys.login.loginButton') }}
        </vben-button>
        <vben-button block class="mt-4" @click="handleBackLogin">
          {{ t('sys.login.backSignIn') }}
        </vben-button>
      </div>
    </vben-form>
  </template>
</template>
