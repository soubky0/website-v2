<script setup lang="ts">
  import { defu } from 'defu'
  import type { ApexOptions } from 'apexcharts'

  const props = withDefaults(
    defineProps<{
      series: ApexAxisChartSeries
      labels: string[]
      options?: ApexOptions
    }>(),
    {
      series: () => [],
      labels: () => [],
      options: () => ({}),
    },
  )

  const sortedData = computed(() => {
    if (!props.series?.length || !props.labels?.length) {
      return { sortedSeries: [], sortedLabels: [] }
    }

    const combined = props.series.map((value, index) => ({
      value,
      label: props.labels[index],
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
    defu(props.options, {
      chart: { type: 'pie' as const },
      labels: sortedData.value.sortedLabels,
    }),
  )
</script>

<template>
  <ContentChart :series="sortedData.sortedSeries" :options="mergedOptions" />
</template>
