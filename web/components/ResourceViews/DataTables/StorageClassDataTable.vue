<template>
  <DataTable
    :title="`StorageClasses`"
    :headers="headers"
    :items="storageclasses"
    :on-click="onClick"
  />
</template>

<script lang="ts">
import { V1StorageClass } from "@kubernetes/client-node";
import { computed, inject, defineComponent } from "@nuxtjs/composition-api";
import DataTable from "./DataTable.vue";
import StorageClassStoreKey from "../../StoreKey/StorageClassStoreKey";
import {} from "../../lib/util";

export default defineComponent({
  components: {
    DataTable,
  },
  setup() {
    const store = inject(StorageClassStoreKey);
    if (!store) {
      throw new Error(`${StorageClassStoreKey} is not provided`);
    }

    const onClick = (storageclass: V1StorageClass) => {
      store.select(storageclass, false);
    };
    const storageclasses = computed(() => store.storageclasses);
    const search = "";
    const headers = [
      {
        text: "Name",
        value: "metadata.name",
        sortable: true,
      },
      { text: "Provisioner", value: "provisioner", sortable: true },
      { text: "Parameters", value: "parameters", sortable: true },
      { text: "Reclaim-Policy", value: "reclaimPolicy", sortable: true },
      {
        text: "VolumeBindingMode",
        value: "volumeBindingMode",
        sortable: true,
      },
      {
        text: "CreationTime",
        value: "metadata.creationTimestamp",
        sortable: true,
      },
      {
        text: "UpdateTime",
        value: "metadata.managedFields[0].time",
        sortable: true,
      },
    ];
    return {
      storageclasses,
      headers,
      search,
      onClick,
    };
  },
});
</script>
