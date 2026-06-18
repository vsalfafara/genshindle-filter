<template>
  <div class="flex flex-col items-center mx-auto max-w-225">
    <div class="flex flex-col gap-4 p-2">
      <div class="flex flex-col gap-2">
        <h3 class="text-lg font-semibold">Rarity</h3>
        <div class="flex gap-2">
          <Label
            v-for="rarity in rarities"
            :key="rarity.value"
            class="hover:bg-accent/50 flex justify-center items-start gap-3 rounded-lg border p-3 has-aria-checked:border-blue-600 has-aria-checked:bg-blue-50 dark:has-aria-checked:border-blue-900 dark:has-aria-checked:bg-blue-950"
          >
            <Checkbox
              :value="rarity.value"
              @update:model-value="
                (checked) =>
                  handleCheckedChange(checked, rarity.value, 'rarity')
              "
              class="hidden data-[state=checked]:border-blue-600 data-[state=checked]:bg-blue-600 data-[state=checked]:text-white dark:data-[state=checked]:border-blue-700 dark:data-[state=checked]:bg-blue-700"
            />
            <img class="h-10" :src="`/${rarity.label}.webp`" alt="" />
          </Label>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <h3 class="text-lg font-semibold">Element</h3>
        <div class="flex gap-2">
          <Label
            v-for="element in elements"
            :key="element.value"
            class="hover:bg-accent/50 flex justify-center items-start gap-3 rounded-lg border p-3 has-aria-checked:border-red-600 has-aria-checked:bg-red-50 dark:has-aria-checked:border-red-900 dark:has-aria-checked:bg-red-950"
          >
            <Checkbox
              :id="element.value.toString()"
              @update:model-value="
                (checked) =>
                  handleCheckedChange(checked, element.value, 'element')
              "
              class="hidden data-[state=checked]:border-red-600 data-[state=checked]:bg-red-600 data-[state=checked]:text-white dark:data-[state=checked]:border-red-700 dark:data-[state=checked]:bg-red-700"
            />
            <img class="h-10 w-10" :src="`/${element.label}.svg`" alt="" />
          </Label>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <h3 class="text-lg font-semibold">Weapon</h3>
        <div class="flex gap-2">
          <Label
            v-for="weapon in weapons"
            :key="weapon.value"
            class="hover:bg-accent/50 flex justify-center items-start gap-3 rounded-lg border p-3 has-aria-checked:border-red-600 has-aria-checked:bg-red-50 dark:has-aria-checked:border-red-900 dark:has-aria-checked:bg-red-950"
          >
            <Checkbox
              :id="weapon.value.toString()"
              @update:model-value="
                (checked) =>
                  handleCheckedChange(checked, weapon.value, 'weapon')
              "
              class="hidden data-[state=checked]:border-red-600 data-[state=checked]:bg-red-600 data-[state=checked]:text-white dark:data-[state=checked]:border-red-700 dark:data-[state=checked]:bg-red-700"
            />
            <img class="h-10 w-10" :src="`/${weapon.label}.webp`" alt="" />
          </Label>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <h3 class="text-lg font-semibold">Nation</h3>
        <div class="flex gap-2">
          <Label
            v-for="nation in nations"
            :key="nation.value"
            class="hover:bg-accent/50 flex justify-center items-start gap-3 rounded-lg border p-3 has-aria-checked:border-red-600 has-aria-checked:bg-red-50 dark:has-aria-checked:border-red-900 dark:has-aria-checked:bg-red-950"
          >
            <Checkbox
              :id="nation.value.toString()"
              @update:model-value="
                (checked) =>
                  handleCheckedChange(checked, nation.value, 'nation')
              "
              class="hidden data-[state=checked]:border-red-600 data-[state=checked]:bg-red-600 data-[state=checked]:text-white dark:data-[state=checked]:border-red-700 dark:data-[state=checked]:bg-red-700"
            />
            <p>{{ nation.label }}</p>
          </Label>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <h3 class="text-lg font-semibold">Version</h3>
        <div class="flex gap-2">
          <Input
            v-model="filters.ltVersion"
            type="number"
            placeholder="Before Version"
            min="1"
            max="6.6"
            step="0.1"
            class="w-33"
          />
          <Input
            v-model="filters.gtVersion"
            type="number"
            placeholder="After Version"
            min="1"
            max="6.6"
            step="0.1"
            class="w-33"
          />
        </div>
      </div>
    </div>
    <div class="flex flex-col gap-2">
      <h3 class="text-lg font-semibold">Characters</h3>
      <div
        class="grid grid-cols-3 md:grid-cols-6 lg:grid-cols-12 gap-y-2 gap-x-4"
      >
        <TooltipProvider
          v-for="character in filteredCharacters"
          :key="character.name"
        >
          <Tooltip>
            <TooltipTrigger as-child>
              <img
                :src="`characters/${character.name}.png`"
                class="max-h-16 max-w-16 h-full w-full"
              />
            </TooltipTrigger>
            <TooltipContent>
              <p>{{ character.version }}</p>
            </TooltipContent>
          </Tooltip>
        </TooltipProvider>
      </div>
    </div>
    <div class="flex flex-col gap-2">
      <h3 class="text-lg font-semibold">Stats</h3>
      <div class="flex gap-6">
        <div class="flex flex-col">
          <h3 class="font-semibold">Elements</h3>
          <pre>{{ JSON.stringify(statsElement, null, 4) }}</pre>
        </div>
        <div class="flex flex-col">
          <h3 class="font-semibold">Weapons</h3>
          <pre>{{ JSON.stringify(statsWeapon, null, 4) }}</pre>
        </div>
        <div class="flex flex-col">
          <h3 class="font-semibold">Nation</h3>
          <pre>{{ JSON.stringify(statsNation, null, 4) }}</pre>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Checkbox } from "./components/ui/checkbox";
import { Label } from "./components/ui/label";
import { Input } from "./components/ui/input";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "./components/ui/tooltip";
import {
  rarities,
  elements,
  weapons,
  nations,
  filterCharacters,
  type Character,
} from "./data/data";
import { ref, watch } from "vue";

const filters = ref({
  rarity: [],
  element: [],
  weapon: [],
  nation: [],
  gtVersion: undefined,
  ltVersion: undefined,
});

const filteredCharacters = ref<any>(filterCharacters(filters.value));

const statsElement = ref({});
const statsWeapon = ref({});
const statsNation = ref({});

const handleCheckedChange = (
  checked: string | boolean,
  value: string | number,
  filter: keyof typeof filters.value,
) => {
  const versionFilters = ["gtVersion", "ltVersion"];
  if (!versionFilters.includes(filter)) {
    if (checked) {
      // @ts-ignore
      filters.value[filter].push(value);
    } else {
      // @ts-ignore
      filters.value[filter] = filters.value[filter].filter(
        (val) => val !== value,
      );
    }
  }
};

watch(
  () => filters,
  () => {
    filteredCharacters.value = filterCharacters(filters.value);

    statsElement.value = filteredCharacters.value.reduce(
      (acc: any, curr: Character) => {
        return {
          ...acc,
          [curr.element]: (acc[curr.element] || 0) + 1,
        };
      },
      {},
    );
    statsWeapon.value = filteredCharacters.value.reduce(
      (acc: any, curr: Character) => {
        return {
          ...acc,
          [curr.weapon]: (acc[curr.weapon] || 0) + 1,
        };
      },
      {},
    );
    statsNation.value = filteredCharacters.value.reduce(
      (acc: any, curr: Character) => {
        return {
          ...acc,
          [curr.nation]: (acc[curr.nation] || 0) + 1,
        };
      },
      {},
    );
  },
  { deep: true },
);
</script>
