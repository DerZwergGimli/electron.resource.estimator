<template>
  <div class="space-y-1 form-control flex flex-row">
    <label class="input-group input-group-xs">
      <span class="basis-1/4"
        ><i
          :class="
            type === 'cpu'
              ? 'bi bi-cpu'
              : type === 'ram'
              ? 'bi bi-memory'
              : 'bi bi-device-hdd'
          "
        ></i
      ></span>
      <input
        type="number"
        placeholder="Type here"
        :class="
          'w-full input input-bordered input-xs ' +
          (value == 0 ? ' input-error' : '')
        "
        :value="value"
        @input="(event) => $emit('changed', event.target.value)"
      />
      <span class="basis-1/4">
        {{ type === "cpu" ? "Cores" : "GB" }}
      </span>
    </label>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from "vue";

defineProps({
  type: {
    type: String,
    default: "none",
  },
  value: {
    type: Number,
    default: 0,
  },
});
</script>

<script lang="ts">
export default {
  name: "VMResource",
};
</script>
