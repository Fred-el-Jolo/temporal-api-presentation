nye<script setup lang="ts">
import { ref, computed } from 'vue'

interface props {
  input?: string
}

const props = defineProps<props>()


const date = computed(()=> {
  if (props.input && props.input.length > 0) {
    return new Date(props.input);
  }
});

const fullYear = computed(() => {
  return date.value?.getFullYear();
});

const month = computed(() => {
  return date.value?.getMonth();
});

const dayOfMonth = computed(() => {
  return date.value?.getDate();
});

const dayOfWeek = computed(() => {
  return date.value?.getDay();
});

const hours = computed(() => {
  return date.value?.getHours();
});

const utcFullYear = computed(() => {
  return date.value?.getUTCFullYear();
});

const utcMonth = computed(() => {
  return date.value?.getUTCMonth();
});

const utcDayOfMonth = computed(() => {
  return date.value?.getUTCDate();
});

const utcDayOfWeek = computed(() => {
  return date.value?.getUTCDay();
});

const utcHours = computed(() => {
  return date.value?.getUTCHours();
});
</script>

<template>
  <div class="header">
    {{date?.toISOString()}}
  </div>
  <table>
    <thead>
      <tr>
        <td>Component</td>
        <td>UTC</td>
        <td>Local</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Year</td>
        <td><div class="inline-code">getUTCFullYear()</div> {{ utcFullYear }}</td>
        <td><div class="inline-code">getFullYear()</div> {{ fullYear }}</td>
      </tr>
      <tr>
        <td>Month</td>
        <td><div class="inline-code">getUTCMonth()</div> {{ utcMonth }}</td>
        <td><div class="inline-code">getMonth()</div> {{ month }}</td>
      </tr>
      <tr>
        <td>Day of month</td>
        <td><div class="inline-code">getUTCDate()</div> {{ utcDayOfMonth }}</td>
        <td><div class="inline-code">getDate()</div> {{ dayOfMonth }}</td>
      </tr>
      <tr>
        <td>Day of Week</td>
        <td><div class="inline-code">getUTCDay()</div> {{ utcDayOfWeek }}</td>
        <td><div class="inline-code">getDay()</div> {{ dayOfWeek }}</td>
      </tr>
      <tr>
        <td>Hours</td>
        <td><div class="inline-code">getUTCHours()</div> {{ utcHours }}</td>
        <td><div class="inline-code">getHours()</div> {{ hours }}</td>
      </tr>
    </tbody>
  </table>
</template>
