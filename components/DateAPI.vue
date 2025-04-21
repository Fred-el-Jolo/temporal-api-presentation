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
  <pre><code>new Date('{{props.input}}')</code></pre>
  <div>
    Equivalent Local date: {{date?.toDateString()}}
  </div>
  <table>
  <thead>
    <tr>
      <th style="text-align: left;">Component</th>
      <th style="text-align: left;">UTC</th>
      <th style="text-align: left;">Local</th>
      <th style="text-align: left;">UTC value</th>
      <th style="text-align: left;">Local value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left;">Year</td>
      <td style="text-align: left;">getUTCFullYear()</td>
      <td style="text-align: left;">getFullYear()</td>
      <td style="text-align: center;">{{ utcFullYear }}</td>
      <td style="text-align: center;">{{ fullYear }}</td>
    </tr>
    <tr>
      <td style="text-align: left;">Month</td>
      <td style="text-align: left;">getUTCMonth()</td>
      <td style="text-align: left;">getMonth()</td>
      <td style="text-align: center;">{{ utcMonth }}</td>
      <td style="text-align: center;">{{ month }}</td>
    </tr>
    <tr>
      <td style="text-align: left;">Day of month</td>
      <td style="text-align: left;">getUTCDate()</td>
      <td style="text-align: left;">getDate()</td>
      <td style="text-align: center;">{{ utcDayOfMonth }}</td>
      <td style="text-align: center;">{{ dayOfMonth }}</td>
    </tr>
    <tr>
      <td style="text-align: left;">Day of week</td>
      <td style="text-align: left;">getUTCDay()</td>
      <td style="text-align: left;">getDay()</td>
      <td style="text-align: center;">{{ utcDayOfWeek }}</td>
      <td style="text-align: center;">{{ dayOfWeek }}</td>
    </tr>
    <tr>
      <td style="text-align: left;">Hours</td>
      <td style="text-align: left;">getUTCHours()</td>
      <td style="text-align: left;">getHours()</td>
      <td style="text-align: center;">{{ utcHours }}</td>
      <td style="text-align: center;">{{ hours }}</td>
    </tr>
  </tbody>
</table>
</template>
