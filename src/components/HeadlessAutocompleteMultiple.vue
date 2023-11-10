<template>
  <div class="relative w-full">
    <Combobox v-model="selectedModel" multiple nullable>
      <div class="relative w-full">
        <div
          class="relative w-full overflow-hidden text-left bg-white border-2 border-gray-300 rounded-lg outline-none cursor-default focus-within:ring-2 focus-within:ring-blue-600 sm:text-sm"
          v-auto-animate
        >
          <ComboboxInput
            class="w-full py-2 pl-3 pr-10 text-sm leading-5 text-gray-900 border-none outline-none"
            :display-value="(person) => person.label"
            placeholder="Select"
            @change="setQuery($event.target.value)"
          />
          <button
            v-if="selectedModel.length >= 1"
            @click="clearFilters()"
            class="absolute z-20 right-10 top-2.5"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
              <path
                fill="#888888"
                d="M19 6.41L17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41z"
              />
            </svg>
          </button>
          <ComboboxButton class="absolute inset-y-0 right-0 z-10 flex items-center pr-2">
            <IconChevronDown class="w-5 h-5 text-gray-400" />
          </ComboboxButton>
        </div>
        <TransitionRoot
          leave="transition ease-in duration-100"
          leave-from="opacity-100"
          leave-to="opacity-0"
          @after-leave="query = ''"
        >
          <ComboboxOptions
            class="absolute z-50 w-full py-1 mt-1 overflow-auto text-base bg-white rounded-md shadow-lg max-h-60 ring ring-black ring-opacity-5 focus:outline-none sm:text-sm"
          >
            <div
              v-if="filteredPeople.length === 0 && query !== ''"
              class="relative px-4 py-2 text-gray-700 cursor-default select-none"
            >
              No results 🙀
            </div>

            <ComboboxOption
              v-for="person in filteredPeople"
              :key="person.id"
              v-slot="{ selected, active }"
              as="div"
              :value="person"
            >
              <div
                class="relative py-2 pl-4 pr-10 cursor-pointer select-none"
                :class="{
                  'bg-blue-200 text-blue-900': active,
                  'text-gray-900': !active
                }"
              >
                <span class="block w-full text-sm break-words">
                  {{ person.label }}
                </span>
                <span
                  v-if="selected"
                  class="absolute inset-y-0 right-0 flex items-center top-0 pr-3"
                  :class="{ 'text-white': active, 'text-blue-900': !active }"
                >
                  <svg
                    class="w-4 h-4"
                    aria-hidden="true"
                    xmlns="http://www.w3.org/2000/svg"
                    width="32"
                    height="32"
                    viewBox="0 0 24 24"
                  >
                    <path
                      fill="#333"
                      d="M9 18.25a.74.74 0 0 1-.53-.25l-5-5a.75.75 0 1 1 1.06-1L9 16.44L19.47 6a.75.75 0 0 1 1.06 1l-11 11a.74.74 0 0 1-.53.25Z"
                    />
                  </svg>
                </span>
              </div>
            </ComboboxOption>
          </ComboboxOptions>
        </TransitionRoot>
      </div>
      <div>
        <div
          v-if="selectedModel.length >= 1"
          class="absolute text-sm right-0 -top-6 -translate-y-0.5 z-30"
        >
          <Menu as="div" class="relative inline-block text-left">
            <MenuButton
              class="text-gray-900 rounded-md hover:ring-2 hover:ring-blue-100 text-xs px-2 py-0.5 bg-blue-100 duration-300 hover:bg-blue-100/90 outline-none focus-visible:ring-2 focus-visible:ring-offset-2 font-normal"
            >
              {{ selectedModel.length }} item<span v-if="selectedModel.length >= 2">s</span>
              selected
            </MenuButton>

            <transition
              enter-active-class="transition duration-100 ease-out"
              enter-from-class="transform scale-95 opacity-0"
              enter-to-class="transform scale-100 opacity-100"
              leave-active-class="transition duration-75 ease-in"
              leave-from-class="transform scale-100 opacity-100"
              leave-to-class="transform scale-95 opacity-0"
            >
              <MenuItems
                class="absolute translate-y-2 right-0 w-64 overflow-hidden origin-top-right divide-y divide-gray-100 rounded-md bg-white shadow-lg ring-2 ring-blue-300 focus:outline-none"
              >
                <div
                  v-auto-animate
                  class="overflow-x-hidden p-1 overflow-y-auto max-h-[200px] border-b border-gray-900/10"
                >
                  <MenuItem
                    as="div"
                    v-for="person in selectedModel"
                    :key="person.label"
                    v-slot="{ active }"
                  >
                    <ComboboxOption :value="person" as="div">
                      <div
                        class="relative flex items-start justify-between w-full p-2 pr-0.5 text-sm text-left border-b rounded-md cursor-pointer select-none last-of-type:border-none"
                        :class="{
                          'text-white bg-blue-600': active,
                          'text-gray-800': !active
                        }"
                      >
                        <span class="block w-full break-words">
                          {{ person.label }}
                        </span>
                        <span
                          class="flex items-center justify-center w-6 h-6 rounded-md shrink-0"
                          :class="{ 'bg-blue-700 fill-white ': active, 'bg-gray-200': !active }"
                        >
                          <svg
                            class="w-3 h-3"
                            aria-hidden="true"
                            xmlns="http://www.w3.org/2000/svg"
                            width="12"
                            height="12"
                            viewBox="0 0 24 24"
                          >
                            <path
                              d="M6.4 19L5 17.6l5.6-5.6L5 6.4L6.4 5l5.6 5.6L17.6 5L19 6.4L13.4 12l5.6 5.6l-1.4 1.4l-5.6-5.6L6.4 19Z"
                            />
                          </svg>
                        </span>
                      </div>
                    </ComboboxOption>
                  </MenuItem>
                </div>
                <div class="flex items-center justify-center px-1 py-1">
                  <MenuItem v-slot="{ active }">
                    <template v-if="selectedModel.length > 0">
                      <ComboboxOption
                        class="relative flex items-center justify-center w-full p-2 text-sm font-medium text-center rounded-md cursor-pointer select-none"
                        :class="{
                          'text-blue-100 bg-blue-600': active,
                          'text-gray-800': !active
                        }"
                        @click="clearFilters()"
                      >
                        Clear filters
                      </ComboboxOption>
                    </template>
                  </MenuItem>
                </div>
              </MenuItems>
            </transition>
          </Menu>
        </div>
      </div>
    </Combobox>
  </div>
</template>

<script setup>
import { ref, computed } from "vue"
import {
  Combobox,
  ComboboxInput,
  ComboboxButton,
  ComboboxOptions,
  ComboboxOption,
  TransitionRoot
} from "@headlessui/vue"
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue"

import { OPTIONS } from "@/components/options"
import IconChevronDown from "./icons/IconChevronDown.vue"
let selectedModel = ref([])
let query = ref("")

function clearFilters() {
  console.log(selectedModel)
  selectedModel.value = []
}

function setQuery(index) {
  query.value = index
  console.log(index)
}

let filteredPeople = computed(() =>
  query.value === ""
    ? OPTIONS
    : OPTIONS.filter((person) =>
        person.label
          .toLowerCase()
          .replace(/\s+/g, "")
          .includes(query.value.toLowerCase().replace(/\s+/g, ""))
      )
)
</script>
