<script setup lang="ts">
  import { ref } from 'vue';
  import DataTable from 'primevue/datatable';
  import Column from 'primevue/column';
  //import TabPanel from 'primevue/tabpanel';
  import { mockDocumentData } from '../../mockData';
  import 'primevue/resources/themes/lara-light-indigo/theme.css';
  import 'primevue/resources/primevue.min.css'
  import 'primeicons/primeicons.css'
  import MultiSelect from 'primevue/multiselect';
  import InputText from 'primevue/inputtext';
  import { FilterMatchMode } from 'primevue/api';

  const searchResults = ref(mockDocumentData);

  const filters1 = ref({ 'global': {value: null, matchMode: FilterMatchMode.CONTAINS}});

  const columns = ref([
    { field: 'subject', header: 'TITLE' },
    { field: 'description', header: 'DESCRIPTION' },
    { field: 'dateOfclose', header: 'DATE OF Close' },
    { field: 'dateOfInterview', header: 'DATE OF INTERVIEW' },
    { field: 'Summary', header: 'SUMMARY' }
  ]);

  const selectedColumns = ref(columns.value);

  const onToggle = (val: string) => {
    selectedColumns.value = columns.value.filter((col) => val.includes(col));
  }     
  
</script> 

<template>
      <DataTable 
            :value="searchResults" 
            responsive-layout="scroll"
            v-model:filters="filters1"
            selectionMode="single"
            stateStorage="session"
            stateKey="dt-state-demo-session"
            dataKey="id"
            >
        <template #header>
          <div style="text-align: left">
            <MultiSelect
              :model-value="selectedColumns"
              :options="columns"
              option-label="header"
              placeholder="Select Columns"
              style="width: 20em"
              @update:model-value="onToggle" />
              <span>
                <i class="pi_pi-search" />
                <InputText v-model="filters1['global'].value" placeholder="Global Search" />
              </span>
          </div>
        </template>
        <Column 
        v-for="(col, index) of selectedColumns"
        :key="col.field + '_' + index" 
        :field="col.field" 
        :header="col.header" 
        :sortable="true"></Column>
      </DataTable>
</template>

<style lang="scss" scoped></style>
