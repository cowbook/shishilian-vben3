<script lang="ts" setup>
defineOptions({ name: 'VbenInput' })
import { maps } from '../../index'
const Input = maps.get('Input')

//console.log('maps:',maps)

</script>
<template>
  <Input v-bind="$attrs">
    <template #[item]="data" v-for="item in Object.keys($slots)" :key="item">
      <slot :name="item" v-bind="data || {}"></slot>
    </template>
  </Input>
</template>

<style scoped></style>
