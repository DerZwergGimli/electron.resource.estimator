<template>
  <div class="space-y-1 form-control">
    <label class="input-group input-group-xs">
      <span class="w-14"
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
        :class="
          'input input-bordered input-xs w-full max-w-xs' +
          (first_value == 0 ? ' input-error' : '')
        "
        type="number"
        :value="first_value"
        @input="(event) => $emit('changed_first', event.target.value)"
      />
      <span class="w-32">
        {{ type === "cpu" ? "Sockets" : type === "ram" ? "Slots" : "Slots" }}
      </span>
    </label>

    <label class="input-group input-group-xs">
      <span class="w-14"
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
        :class="
          'input input-bordered input-xs w-full max-w-xs' +
          (second_value == 0 ? ' input-error' : '')
        "
        type="number"
        :value="second_value"
        @input="(event) => $emit('changed_second', event.target.value)"
      />
      <span class="w-32">
        {{ type === "cpu" ? "Cores" : type === "ram" ? "GB" : "GB" }}
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
  first_value: {
    type: Number,
    default: 0,
  },
  second_value: {
    type: Number,
    default: 0,
  },
});
</script>
