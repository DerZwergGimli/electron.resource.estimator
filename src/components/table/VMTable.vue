<template>
  <div class="overflow-x-auto">
    <table class="table w-full table-zebra">
      <!-- head -->
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>OS</th>
          <th>Preset</th>
          <th>CPU <i class="bi bi-cpu"></i></th>
          <th>RAM <i class="bi bi-memory"></i></th>
          <th>Storage <i class="bi bi-device-hdd"></i></th>
          <th>Amount</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <!-- body -->
        <tr v-for="(vm, index) in vms" :key="index">
          <th>{{ index }}</th>
          <th>
            <input
              class="input input-bordered input-xs w-full max-w-xs"
              type="text"
              :value="vm.name"
              @input="(event) => (vm.name = event?.target?.value || 0)"
            />
          </th>
          <th>
            <input
              class="input input-bordered input-xs w-full max-w-xs"
              type="text"
              :value="vm.os"
              @input="(event) => (vm.os = event.target.value)"
            />
          </th>
          <th>
            <select
              class="select select-bordered select-xs w-full max-w-xs"
              @change="evt_change_preset($event, vm.uuids)"
            >
              <option
                v-for="preset in store.presets_vm"
                :key="preset.id"
                :value="preset.name"
              >
                {{ preset.name }}
              </option>
            </select>
          </th>
          <th>
            <VMResouce
              :value="vm.vcpu.rec"
              @changed="(value) => (vm.vcpu.rec = parseInt(value))"
              type="cpu"
            ></VMResouce>
          </th>
          <th>
            <VMResouce
              :value="vm.vram.rec"
              @changed="(value) => (vm.vram.rec = parseInt(value))"
              type="ram"
            ></VMResouce>
          </th>
          <th>
            <VMResouce
              :value="vm.vstorage.rec"
              @changed="(value) => (vm.vstorage.rec = parseInt(value))"
              type="storage"
            ></VMResouce>
          </th>
          <th>
            <div class="flex flex-row">
              <label class="input-group input-group-xs">
                <span class="basis-1/4"><i class="bi bi-123"></i></span>
                <input
                  :class="
                    'w-full input input-bordered input-xs' +
                    (vm.uuids.length == 0 ? ' input-error' : '')
                  "
                  type="number"
                  :value="vm.uuids.length"
                  @input="
                    (event) => {
                      store.check_uuid_length(
                        vm.name,
                        parseInt(event.target.value)
                      )
                    }
                  "
                />
                <span class="basis-1/4">Total</span>
              </label>
            </div>
          </th>
          <th>
            <i
              class="btn btn-sm bi bi-trash"
              @click="$emit('clk_remove_item', vm.uuids)"
            ></i>
          </th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { createToast } from 'mosha-vue-toastify'
import { TOAST_SUCCESS } from '../../extra/toast-config'
import { defineProps, PropType } from 'vue'
import VMResouce from './table_elements/VMResource.vue'
import { VM } from '../../store/types/VM'
import { useAppStorage } from '../../store/AppStorage'

const store = useAppStorage()
store.init()

defineProps({
  vms: {
    type: Array as PropType<VM[]>,
    default: null,
  },
})

function evt_change_preset(event: any, vm_uuids: string[]) {
  let preset_name: string = event.target.value
  let selected_preset = store.presets_vm.find(
    (preset) => preset.name == preset_name
  )

  store.vmsList.map((vm) => {
    if (vm.uuids == vm_uuids) {
      vm.os = selected_preset?.os ?? 'error'
      vm.vcpu.rec = selected_preset?.cpu ?? 0
      vm.vram.rec = selected_preset?.ram ?? 0
      vm.vstorage.rec = selected_preset?.storage ?? 0
    }
  })
  createToast('Preset updated!', TOAST_SUCCESS)
}
</script>

<script lang="ts">
export default {
  name: 'VMTable',
}
</script>
