<script setup lang="ts">
import {
  PrioritizedListData,
  type PrioritizedListItemData
} from '@/types/settings/prioritized-list-data';
import {
  BLOCKCHAIN_ACCOUNT_PRIO_LIST_ITEM,
  ENS_NAMES_PRIO_LIST_ITEM,
  ETHEREUM_TOKENS_PRIO_LIST_ITEM,
  GLOBAL_ADDRESSBOOK_PRIO_LIST_ITEM,
  HARDCODED_MAPPINGS_PRIO_LIST_ITEM,
  PRIVATE_ADDRESSBOOK_PRIO_LIST_ITEM,
  type PrioritizedListId
} from '@/types/settings/prioritized-list-id';

const currentAddressNamePriorities = ref<PrioritizedListId[]>([]);
const { addressNamePriority } = storeToRefs(useGeneralSettingsStore());
const { resetAddressesNames } = useAddressesNamesStore();

const finishEditing = async () => {
  resetCurrentAddressNamePriorities();
  resetAddressesNames();
};

const resetCurrentAddressNamePriorities = () => {
  set(currentAddressNamePriorities, get(addressNamePriority));
};

const availableCurrentAddressNamePriorities =
  (): PrioritizedListData<PrioritizedListId> => {
    const itemData: Array<PrioritizedListItemData<PrioritizedListId>> = [
      BLOCKCHAIN_ACCOUNT_PRIO_LIST_ITEM,
      ENS_NAMES_PRIO_LIST_ITEM,
      ETHEREUM_TOKENS_PRIO_LIST_ITEM,
      GLOBAL_ADDRESSBOOK_PRIO_LIST_ITEM,
      HARDCODED_MAPPINGS_PRIO_LIST_ITEM,
      PRIVATE_ADDRESSBOOK_PRIO_LIST_ITEM
    ];

    return new PrioritizedListData(itemData);
  };

onMounted(() => {
  resetCurrentAddressNamePriorities();
});
const { t } = useI18n();
</script>

<template>
  <div>
    <div class="text-subtitle-1 mb-3">
      {{ t('address_book.hint.priority.title') }}
    </div>
    <SettingsOption
      #default="{ error, success, update }"
      setting="addressNamePriority"
      @finished="finishEditing()"
    >
      <PrioritizedList
        :value="currentAddressNamePriorities"
        :all-items="availableCurrentAddressNamePriorities()"
        :item-data-name="
          t('address_name_priority_setting.data_name').toString()
        "
        :disable-add="true"
        :disable-delete="true"
        :status="{ error, success }"
        @input="update($event)"
      />
    </SettingsOption>
  </div>
</template>
