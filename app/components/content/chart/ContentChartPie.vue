<script setup lang="ts">
  import { defu } from 'defu'
  import type { ApexOptions } from 'apexcharts'

  const {
    series = [],
    labels = [],
    options = {},
  } = defineProps<{
    series: ApexAxisChartSeries
    labels: string[]
    options?: ApexOptions
  }>()

  const mergedOptions = computed<ApexOptions>(() =>
    defu(options, { chart: { type: 'pie' as const }, labels }),
  )
</script>

<template>
  <ContentChart :series :options="mergedOptions" />
</template>
