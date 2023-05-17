<template>
  <el-container>
    
  </el-container>
</template>

<script>
import { useClientStore } from '@/store/client';
import { useUserStore } from '@/store/user';
export default {
  name: 'HomeView',
  components:{
  },
  setup(){
    const clientStore = useClientStore();
    const userStore = useUserStore();

    return {clientStore, userStore}
  },
  computed:{
    getClients(){
      return this.clientStore.getClients
    },
    filteredClients(){
      return this.clientStore.getClients.filter(
        client => 
          !this.search || 
           this.search.split(' ').every(w => 
            client.firstName.toLowerCase().includes(w.toLowerCase()) ||
            client.lastName.toLowerCase().includes(w.toLowerCase()) ||
            client.car.toLowerCase().includes(w.toLowerCase()) ||
            client.employee.firstName.toLowerCase().includes(w.toLowerCase())
            )
        )
    },
    indexStart() {
      return (this.current - 1) * this.pageSize;
    },
    indexEnd() {
      return this.indexStart + this.pageSize;
    },
    paginated(){
      return this.filteredClients.slice(this.indexStart, this.indexEnd);
    }
  },
  data() {
    return {
      search: '',
      current: 1,
      pageSize: 2,
      addDialogToggle: false,
      detailsDialogToggle: false,
      editDialogToggle: false,
      deleteDialogToggle: false,
    }
  },
  methods:{
    clickDetails(id){
      this.detailsDialogToggle = !this.detailsDialogToggle;
      this.clientStore.selectedClient = this.clientStore.getClientByID(id);
    },
    clickEdit(id){
      this.editDialogToggle = !this.editDialogToggle;
      this.clientStore.selectedClient = this.clientStore.getClientByID(id);
    },
    clickDelete(id){
      this.deleteDialogToggle = !this.deleteDialogToggle;
      console.log(id)
      this.clientStore.selectedClient = this.clientStore.getClientByID(id);
      this.clientStore.deleteClient();
    },
  }
}
</script>

<style scoped>
.logged-out{
  display: block;
  position: fixed;
  font-size: 28px;
  color: #9fd3c7;
  font-weight: bold;
  height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%,0);
}
.search-header{
  display: flex;
  align-items: center;
  margin-bottom: 24px;
  justify-content: center;
}
.searchbar{
  width: 40vw;
}
.table{
  display: flex;
}
.button-add{
  display: flex;
  justify-content: end;
  margin: 4px;
  margin-right: 28px;
}
.button{
    background-color: #9fd3c7;
    border-color: #9fd3c7;
    width: fit-content;
    padding: 10px;
    --el-button-hover-bg-color: #bde6dc;
    --el-button-hover-color: #bde6dc;
    --el-button-hover-text-color: white;
}
.button:hover{
    background-color: #bde6dc;
    border-color: #bde6dc;
    color: white;
}
</style>
