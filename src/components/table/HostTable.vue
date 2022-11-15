<template>
  <div class="overflow-x-auto">
    <table class="table w-full table-zebra">
      <!-- head -->
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Manufacturer</th>
          <th>CPU <i class="bi bi-cpu"></i></th>
          <th>RAM <i class="bi bi-memory"></i></th>
          <th>Storage <i class="bi bi-device-hdd"></i></th>
          <th>RAID</th>
          <th>Amount</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <!-- body -->
        <tr v-for="(host, index) in hosts" :key="index">
          <th>{{ index }}</th>
          <th>
            <input
              class="input input-bordered input-xs w-full max-w-xs"
              type="text"
              :value="host.name"
              @input="(event) => (host.name = event.target.value)"
            />
          </th>
          <th>
            <input
              class="input input-bordered input-xs w-full max-w-xs"
              type="text"
              :value="host.manufacturer"
              @input="(event) => (host.manufacturer = event.target.value)"
            />
          </th>
          <th>
            <HostResources
              :first_value="host.cpu.sockets"
              :second_value="host.cpu.cores"
              type="cpu"
              @changed_first="(value) => (host.cpu.sockets = parseInt(value))"
              @changed_second="(value) => (host.cpu.cores = parseInt(value))"
            ></HostResources>
            <div class="flex flex-row space-x-2">
              <i class="bi bi-calculator"></i>
              <p class="text-sm pt-1">
                {{ host.cpu.sockets * host.cpu.cores * 2 }} vCores
              </p>
            </div>
          </th>
          <th>
            <HostResources
              :first_value="host.ram.slots"
              :second_value="host.ram.size"
              type="ram"
              @changed_first="(value) => (host.ram.slots = parseInt(value))"
              @changed_second="(value) => (host.ram.size = parseInt(value))"
            ></HostResources>
            <div class="flex flex-row space-x-2">
              <i class="bi bi-calculator"></i>
              <p class="text-sm pt-1">
                {{ host.ram.slots * host.ram.size }} vRAM [GB]
              </p>
            </div>
          </th>
          <th>
            <HostResources
              :first_value="host.storage.amount"
              :second_value="host.storage.size"
              type="storage"
              @changed_first="
                (value) => (host.storage.amount = parseInt(value))
              "
              @changed_second="(value) => (host.storage.size = parseInt(value))"
            ></HostResources>
            <div class="flex flex-row space-x-2">
              <i class="bi bi-calculator"></i>
              <p class="text-sm pt-1">
                Netto Storage =
                {{
                  calculate_storage_netto(
                    host.storage.amount,
                    host.storage.size,
                    2
                  )
                }}
                GB
              </p>
            </div>
          </th>
          <th>
            <select
              v-model="host.storage.raid"
              class="select select-bordered select-xs max-w-xs"
            >
              <option
                v-for="raid in Object.values(RAIDEnums)"
                :key="raid"
                :selected="host.storage.raid === raid"
              >
                {{ raid }}
              </option>
            </select>
            <div class="flex flex-row space-x-1">
              <i class="bi bi-calculator"></i>
              <p class="text-sm pt-1">
                {{
                  caluclate_raid(
                    host.storage.amount,
                    host.storage.size,
                    host.storage.raid,
                    1
                  )
                }}
                GB
              </p>
            </div>
          </th>

          <th>
            <input
              class="input input-bordered input-xs w-full max-w-xs"
              type="number"
              :value="host.uuids.length"
              @input="
                (event) => {
                  store.check_uuid_length(
                    host.name,
                    parseInt(event.target.value)
                  )
                }
              "
            />
          </th>
          <th>
            <i
              class="btn btn-sm bi bi-trash"
              @click="$emit('clk_remove_item', host.uuids)"
            ></i>
          </th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { defineProps, PropType } from 'vue'
import { Host } from '../../store/types/Host'
import { Icon } from '@iconify/vue'
import {
  calculate_storage_netto,
  caluclate_raid,
} from '../../extra/calculator_storage'
import { RAIDEnums } from '../../store/types/enums'
import HostResources from '../../components/table/table_elements/HostResources.vue'
import { useAppStorage } from '../../store/AppStorage'
const store = useAppStorage()
store.init()

defineProps({
  hosts: {
    type: Array as PropType<Host[]>,
    default: null,
  },
})
</script>

<script lang="ts">
export default {
  name: 'HostTable',
}
</script>
