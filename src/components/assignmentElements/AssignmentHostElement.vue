<template>
  <div class="flex flex-col" v-if="host">
    <div class="flex flex-row self-center pb-2">
      <p class="text-2xl font-bold">{{ host.name }}</p>
      <span class="badge badge-sm">{{ host_index }}</span>
    </div>

    <div class="flex flex-row justify-evenly m-2 p-2 rounded-lg">
      <HostUsageElement
        :host_uuid="host_uuid"
        :virtual_hw_conf="VirtualHardwareEnums.vcpu"
        :hw_conf="HardwareEnums.cpu"
        :host="host"
      ></HostUsageElement>
      <HostUsageElement
        :host_uuid="host_uuid"
        :virtual_hw_conf="VirtualHardwareEnums.vram"
        :hw_conf="HardwareEnums.ram"
        :host="host"
      ></HostUsageElement>
      <HostUsageElement
        :host_uuid="host_uuid"
        :virtual_hw_conf="VirtualHardwareEnums.vstorage"
        :hw_conf="HardwareEnums.storage"
        :host="host"
      ></HostUsageElement>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineComponent, defineProps, PropType } from 'vue'
import { Host } from '../../store/types/Host'
import { VM } from '../../store/types/VM'
import { Assignment } from '../../store/types/Assignment'
import {
  VirtualHardwareEnums,
  HardwareEnums,
  SystemRecommendationEnums,
} from '../../store/types/enums'
import { get_used_cpu } from '../../extra/calculator'
import HostUsageElement from '../../components/assignmentElements/HostUsageElement.vue'

defineProps({
  host_uuid: {
    type: String,
    default: '',
  },
  host: {
    type: Object as PropType<Host>,
    default: undefined,
  },
  system_recommendation: {
    type: String as PropType<SystemRecommendationEnums>,
    default: 'rec',
  },
  host_index: {
    type: Number,
    default: 0,
  },
})

defineComponent({
  HostUsageElement,
})
</script>

<script lang="ts">
export default {
  name: 'AssignmentHostElement',
}
</script>
