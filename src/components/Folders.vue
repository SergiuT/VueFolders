<template>
  <div>
  <a-table
    v-if="this.item.toString() === '1'"
    :columns="columns"
    :row-key="record => record.guid"
    :data-source="filteredList"
    :pagination="pagination"
    :row-selection="rowSelectionChange"
  >
  </a-table>
  <a-table
    v-if="this.item === '2'"
    :columns="columns"
    :row-key="record => record.guid"
    :data-source="filteredList"
    :pagination="pagination"
    :row-selection="rowSelectionChange"
  >
  </a-table>
  <a-table
    v-if="this.item === '3'"
    :columns="columns"
    :row-key="record => record.guid"
    :data-source="filteredList"
    :pagination="pagination"
    :row-selection="rowSelectionChange"
  >
  </a-table>
  </div>
</template>

<script>
import moment from 'moment';

const rowSelection = {
  onChange: (selectedRowKeys, selectedRows) => {
    console.log(`selectedRowKeys: ${selectedRowKeys}`, 'selectedRows: ', selectedRows);
  },
  onSelect: (record, selected, selectedRows) => {
    console.log(record, selected, selectedRows);
  },
  onSelectAll: (selected, selectedRows, changeRows) => {
    console.log(selected, selectedRows, changeRows);
  },
};

const columns = [
  {
    title: 'Name',
    dataIndex: 'name',
    key: 'name',
    sorter: (a, b) => a.name.length - b.name.length,
    width: '20%',
  },
  {
    title: 'Last Accessed',
    dataIndex: 'lastAccessedDate',
    width: '20%',
    sorter: (a, b) => b.lastAccessedDate - a.lastAccessedDate,
  },
  {
    title: 'Sharing',
    dataIndex: 'sharing',
    width: '20%'
  },
  {
    title: 'Size',
    dataIndex: 'size',
    width: '20%',
    sorter: (a, b) => a.size - b.size
  },
];

export default {
  name: 'Folders',
  props: [
    'item',
    'searchField',
    'data',
    'addingNewFolder',
    'folderName'
  ],
  data() {
    return {
      allData: [],
      pagination: this.data.length,
      loading: false,
      columns,
      rowSelection
    };
  },
  mounted() {
    this.format_data();
  },
  computed: {
    rowSelectionChange() {
      console.log('hdsauikdhasduashdkjashdsakjdhas');
      
      return this.rowSelection
    },
    filteredList() {
      const dataToBeFiltered = this.item.toString() === '1' ?
           this.allData : this.item === '2' ?
             this.allData.filter(item => {
                return moment().diff(item.lastAccessedDate, 'hours') < 24
             }) : this.item === '3' ?
               this.allData.filter(item => item.isDeleted === true) : [];

      const filter = this.searchField.trim().toUpperCase();
      return filter ? dataToBeFiltered.filter(item =>
      {
        return item.name.toUpperCase().indexOf(filter) !== -1;
      }) : dataToBeFiltered;
    }
  },
  methods: {
    format_data() {
      this.data.map(item => {
        let date = item.lastAccessedDate;
        let formatedDate = date.substr(0, date.indexOf(' '));

        let updatedItem = {
          guid: item.guid,
          isDeleted: item.isDeleted,
          size: (item.sizeBytes / (1024*1024)).toFixed(2),
          name: item.name,
          sharing: item.sharing === 1 ? 'Public' : item.sharing === 2 ? 'Shared' : item.sharing === 3 ? 'Private' : '',
          lastAccessedDate: moment(formatedDate).format('MM-DD-YYYY')
        };
        
        this.allData.push(updatedItem)
      })
    },
  },
}
</script>

<style scoped>
</style>