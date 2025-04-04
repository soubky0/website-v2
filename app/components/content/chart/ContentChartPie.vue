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

  const sortedData = computed(() => {
    if (!series?.length || !labels?.length) {
      return { sortedSeries: [], sortedLabels: [] }
    }

    const combined = series.map((value, index) => ({
      value,
      label: labels[index],
    }))

    combined.sort((a, b) => Number(b.value) - Number(a.value))

    return {
      sortedSeries: combined.map((item) => item.value),
      sortedLabels: combined
        .map((item) => item.label)
        .filter((label): label is string => label !== undefined),
    }
  })

  const mergedOptions = computed<ApexOptions>(() =>
    defu(options, {
      chart: { type: 'pie' as const },
      labels: sortedData.value.sortedLabels,
    }),
  )
</script>

<template>
  <ContentChart :series="sortedData.sortedSeries" :options="mergedOptions" />
</template>
