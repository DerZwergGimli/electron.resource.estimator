<template>
  <div class="flex flex-col">
    <div
      v-if="hw_conf === HardwareEnums.cpu"
      class="radial-progress bg-primary text-primary-content border-4 border-primary"
      :style="
        '--value: ' +
        calculate_percentage(
          host.cpu.sockets * host.cpu.cores * 2,
          get_used(host_uuid, VirtualHardwareEnums.vcpu)
        )
      "
    >
      {{
        calculate_percentage(
          host.cpu.sockets * host.cpu.cores * 2,
          get_used(host_uuid, VirtualHardwareEnums.vcpu)
        )
      }}
      %
    </div>

    <div
      v-if="hw_conf === HardwareEnums.ram"
      class="radial-progress bg-primary text-primary-content border-4 border-primary"
      :style="
        '--value: ' +
        calculate_percentage(
          host.ram.slots * host.ram.size,
          get_used(host_uuid, VirtualHardwareEnums.vram)
        )
      "
    >
      {{
        calculate_percentage(
          host.ram.slots * host.ram.size,
          get_used(host_uuid, VirtualHardwareEnums.vram)
        )
      }}
      %
    </div>
    <div
      v-if="hw_conf === HardwareEnums.storage"
      class="radial-progress bg-primary text-primary-content border-4 border-primary"
      :style="
        '--value: ' +
        calculate_percentage(
          caluclate_raid(
            host.storage.amount,
            host.storage.size,
            host.storage.raid,
            1
          ),
          get_used(host_uuid, VirtualHardwareEnums.vstorage)
        )
      "
    >
      {{
        calculate_percentage(
          caluclate_raid(
            host.storage.amount,
            host.storage.size,
            host.storage.raid,
            1
          ),
          get_used(host_uuid, VirtualHardwareEnums.vstorage)
        )
      }}
      %
    </div>

    <div class="flex flex-row self-center">
      <i
        :class="
          'bi self-center pr-2' +
          (virtual_hw_conf === VirtualHardwareEnums.vcpu
            ? ' bi-cpu'
            : virtual_hw_conf === VirtualHardwareEnums.vram
            ? ' bi-memory'
            : ' bi-device-hdd')
        "
      ></i>
      <p>
        {{ get_used(host_uuid, virtual_hw_conf) }}
      </p>
      /
      <p v-if="hw_conf === HardwareEnums.cpu">
        {{ host.cpu.sockets * host.cpu.cores * 2 }}
      </p>
      <p v-if="hw_conf === HardwareEnums.ram">
        {{ host.ram.slots * host.ram.size }}
      </p>
      <p v-if="hw_conf === HardwareEnums.storage">
        {{
          caluclate_raid(
            host.storage.amount,
            host.storage.size,
            host.storage.raid,
            1
          )
        }}
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, PropType, unref } from 'vue'
import { get_used } from '../../extra/calculator'
import { caluclate_raid } from '../../extra/calculator_storage'
import { HardwareEnums, VirtualHardwareEnums } from '../../store/types/enums'
import { Host } from '../../store/types/Host'

defineProps({
  host_uuid: {
    type: String,
    default: 'none',
  },
  virtual_hw_conf: {
    type: Object as PropType<VirtualHardwareEnums>,
    default: VirtualHardwareEnums.vcpu,
  },
  hw_conf: {
    type: Object as PropType<HardwareEnums>,
    default: HardwareEnums.cpu,
  },
  host: {
    type: Object as PropType<Host>,
    default: null,
  },
})

function calculate_percentage(available: number, used: number) {
  console.log('used: ' + used + ' available: ' + available)
  return ((used / available) * 100).toFixed(0)
}
</script>

<script lang="ts">
export default {
  name: 'HostUsageElement',
}
</script>
