<template>
  <a-layout id="components-layout-demo-side" style="min-height: 100vh">
    <a-layout-sider v-model="collapsed" collapsible>
      <h3 style="color: white; marginLeft: 10px; marginTop: 20px">OneDrive</h3>
      <a-menu style='marginTop: 30px' theme="dark" :default-selected-keys="['1']" mode="inline">
        <a-menu-item key="1" @click="handleChange($event)">
          <a-icon type="file" />
          <span>All Files</span>
        </a-menu-item>
        <a-menu-item key="2" @click="handleChange($event)">
          <a-icon type="clock-circle" />
          <span>Accessed Last Day</span>
        </a-menu-item>
        <a-menu-item key="3" @click="handleChange($event)">
          <a-icon type="delete" />
          <span>Recycle Bin</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header style="background: #fff; padding: 0">
        <a-input-search 
          placeholder="Search everything" 
          style="width: 200px; marginLeft: 20px" 
          v-model="searchData"
        />
        <a-button 
          type="primary" 
          style="marginLeft: 20px"
          @click="onClickNew"
        >
          + New
        </a-button>
        <a-modal
          title="Add New Folder"
          :visible="visible"
          :confirm-loading="confirmLoading"
          @ok="handleOk"
          @cancel="handleCancel"
        >
          <a-input placeholder="Folder name" v-model="folderName"/>
        </a-modal>
      </a-layout-header>
      <a-layout-content style="margin: 0 16px">
        <a-breadcrumb style="margin: 16px 0">
          <a-breadcrumb-item>Files</a-breadcrumb-item>
          <a-breadcrumb-item v-if="this.item_key.toString() === '1'">All Files</a-breadcrumb-item>
          <a-breadcrumb-item v-if="this.item_key === '2'">Accessed Last Day</a-breadcrumb-item>
          <a-breadcrumb-item v-if="this.item_key === '3'">Deleted Files</a-breadcrumb-item>
        </a-breadcrumb>
        <div :style="{ padding: '24px', background: '#fff', minHeight: '360px' }">
          <Folders 
            :item="item_key" 
            :addingNewFolder="addingNewFolder" 
            @selected="onSelectedRow" 
            :searchField="searchData" 
            :data="dataUpdate"
            :folderName="folderName"
          />
        </div>
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<script>
import Folders from './Folders';
import json from '../data.json';

export default {
  name: 'MainPage',
  data() {
    return {
      data: json,
      collapsed: false,
      item_key: 1,
      searchData: '',
      folderName: '',
      visible: false,
      confirmLoading: false,
      addingNewFolder: false,
    };
  },
  components: {
    Folders
  },
  computed: {
    dataUpdate() {
      return this.data;
    }
  },
  methods: {
    handleChange(event) {
      this.item_key = event.key;
    },
    onSelectedRow(itemData) {
      console.log(itemData)
    },
    onClickNew() {
      this.visible = true;
    },
    handleOk() {
      this.confirmLoading = true;
      
      this.addingNewFolder = true;

      setTimeout(() => {
        this.visible = false;
        this.confirmLoading = false;
      }, 1000);
    },
    handleCancel() {
      this.visible = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ant-layout {
  width: 100%;
}
#components-layout-demo-top-side-2 .logo {
  width: 120px;
  height: 31px;
  background: rgba(255, 255, 255, 0.2);
  margin: 16px 28px 16px 0;
  float: left;
}
</style>
