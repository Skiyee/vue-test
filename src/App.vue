<script setup lang="ts">
import { createAlova } from 'alova';
import fetchAdapter from 'alova/fetch';
import vueHook from 'alova/vue';

import { createServerTokenAuthentication, useForm, usePagination } from 'alova/client';

const { onAuthRequired, onResponseRefreshToken } = createServerTokenAuthentication({
  refreshTokenOnSuccess:{
    isExpired: () => {
      // DO SOMETHING Bc
      return false
    },
    handler: async () => {
      // DO SOMETHING Bc
    }
  }
})

const alovaInst = createAlova({
  statesHook: vueHook,
  requestAdapter: fetchAdapter(),
  responded: onResponseRefreshToken({
    onSuccess(response) {
      return response.json();
    }
  })
})

interface Test {
  id:number
}

const testMethod = (data?: any) => alovaInst.Post<Test[]>('/test', data)

const { data } = useForm(form => testMethod(form), {
  initialForm: {
    username: 'admin',
    password: '123456'
  }
});

const { data } = usePagination(testMethod, {
  data: (data) => data,
  total: () => 1
})


</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
  </header>


</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

</style>
