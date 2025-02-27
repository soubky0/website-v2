<script setup lang="ts">
  import { defu } from 'defu'
  import type { ApexOptions } from 'apexcharts'
  const {
    labels = [],
    series = [],
    horizontal,
    options = {},
    stacked,
  } = defineProps<{
    labels: string[]
    series: ApexAxisChartSeries
    horizontal?: boolean
    options?: ApexOptions
    stacked?: boolean
  }>()

  const xAxisLabels = computed(() => labels)

  // @ts-expect-error todo: fix types
  const mergedOptions = computed<ApexOptions>(() =>
    defu(options, {
      chart: { type: 'bar', stacked },
      xaxis: { categories: xAxisLabels.value },
      plotOptions: {
        bar: {
          horizontal: horizontal || false,
        },
      },
      dataLabels: {
        formatter(val: any) {
          return val ? `${val}` : ''
        },
      },
    }),
  )
</script>

<template>
  <ContentChart :series :options="mergedOptions" />
</template>
