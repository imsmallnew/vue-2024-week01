<script setup>
import { ref } from "vue"

let editorTitle = ref('')
let deleteId = ref('')
let errorText1 = ref('')
let errorText2 = ref('')
let errorText3 = ref('')
let errorText4 = ref('')

// Header
const headers = ref([
  {
    id: 1,
    value:'品項'
  },
  {
    id: 2,
    value:'描述'
  },
  {
    id: 3,
    value:'價格'
  },
  {
    id: 4,
    value:'庫存'
  },
  {
    id: 5,
    value:'操作功能'
  }
])

// Item list
const items = ref([
  {
    id: 1,
    label: '珍珠奶茶',
    desc: '香濃奶茶搭配QQ珍珠',
    price: 50,
    stock: 20,
    editing: false
  },
  {
    id: 2,
    label: '冬瓜檸檬',
    desc: '香濃奶茶搭配QQ珍珠',
    price: 45,
    stock: 18,
    editing: false
  },
  {
    id: 3,
    label: '翡翠檸檬',
    desc: '綠茶與檸檬的完美結合',
    price: 55,
    stock: 34,
    editing: false
  },
  {
    id: 4,
    label: '四季春茶',
    desc: '香醇四季春茶，回甘無比',
    price: 45,
    stock: 10,
    editing: false
  },
  {
    id: 5,
    label: '阿薩姆奶茶',
    desc: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    stock: 25,
    editing: false
  },
  {
    id: 6,
    label: '檸檬冰茶',
    desc: '檸檬與冰茶的清新組合',
    price: 45,
    stock: 20,
    editing: false
  },
  {
    id: 7,
    label: '芒果綠茶',
    desc: '芒果與綠茶的獨特風味',
    price: 55,
    stock: 18,
    editing: false
  },
  {
    id: 8,
    label: '抹茶拿鐵',
    desc: '抹茶與鮮奶的絕配',
    price: 60,
    stock: 20,
    editing: false
  },
])

const editItems = ref([
// {
//     id: new Date().getTime(),
//     label: '1',
//     desc: '2',
//     price: 0,
//     stock: 0,
//   }
])

// Add Stock btn
const addStock = (item) => {
  const index = items.value.findIndex(i => i.id === item.id)
  items.value[index].stock++;
}

// Minus Stcok btn
const minusStock= (item) => {
  const index = items.value.findIndex(i => i.id === item.id)
  if(items.value[index].stock > 0){
    items.value[index].stock--;
  }else{
    alert("庫存不可小於0!");
    return
  }
}

// Create btn
const createItem = () => {
  editorTitle = "Adding Area";
  editItems.value.push({
      id: new Date().getTime(),
      label: "",
      desc: "",
      price: 0,
      stock: 0,
      editing: false,
  })
}

// Edit btn
const editItem = (item) => {
  editorTitle = "Editing Area";
  if(editItems.value.length > 0){
    console.log("已有商品在編輯!")
    return
  }else{
    const index = items.value.findIndex(i => i.id === item.id);
    items.value[index].editing = true;
    editItems.value.push({
      id: item.id,
      label: item.label,
      desc: item.desc,
      price: item.price,
      stock: item.stock,
      editing: false,
    })
  }
}

// Delete btn
const deleteItem= (item) => {
  const index = items.value.findIndex(i => i.id === item.id);
  items.value.splice(index, 1);
  deleteId = "";
}

const clearText = () => {
    editorTitle = "";
    errorText1 = ""
    errorText2 = ""
    errorText3 = ""
    errorText4 = ""
}

const validateInput = () => {
  let item = editItems.value[0];
  errorText1 = item.label === "" ? "此欄位不可為空" : "";
  errorText2 = item.desc === "" ? "此欄位不可為空" : "";
  if(isNaN(item.price)){
    errorText3 = "此欄位須為數字"
  }else if(item.price < 0){
    errorText3 = "此欄位不可小於0"
  }else if(item.price === ""){
    errorText3 = "此欄位不可為空"
  }else{
    errorText3 = ""
  }

  if(isNaN(item.stock)){
    errorText4 = "此欄位須為數字"
  }else if(item.stock < 0){
    errorText4 = "此欄位不可小於0"
  }else if(item.stock === ""){
    errorText4 = "此欄位不可為空"
  }else{
    errorText4 = ""
  }

  if( errorText1 !=="" || errorText2 !=="" || errorText3 !=="" || errorText4 !=="" ){
    return false
  }else{
    return true
  }
}

// Update btn
const updateItem= () => {
  let item = editItems.value[0];
  if(!validateInput()){
    console.log("有錯誤無法更新");
    return
  }else{
    const index = items.value.findIndex(i => i.id === item.id);
    // update origin
    items.value[index] = item;
    // clear edit item
    editItems.value = [];
    clearText()
  }
}

// Cancel btn
const cancelItem = () => {
  if(editorTitle === "Adding Area"){
    editItems.value = [];
    clearText()
  }else{
    console.log("editItems.value.length:",editItems.value.length)
    if(editItems.value.length === 0){
      console.log("已無暫存資料可取消!")
      return
    }else{
      const index = items.value.findIndex(i => i.editing === true)
      items.value[index].editing = false;
      // clear edit item
      editItems.value = [];
    }
    clearText()
  }
}

// clone btn
const cloneItem= () => {
  let item = editItems.value[0];
  editItems.value[0].id = new Date().getTime();
  console.log("clone_item:",item)
  const index = items.value.findIndex(i => i.editing === true);
  items.value[index].editing = false;
  // copy item to items
  items.value.push(item);
  // clear edit item
  editItems.value = [];
  clearText()
}

// Confirm btn
const confirmItem = () => {
  if(!validateInput()){
    console.log("有錯誤無法更新");
    return
  }else{
    let item = editItems.value[0];
    items.value.push(item);
    // clear edit item
    editItems.value = [];
    clearText()
  }  
}

const cancelDelete = (item) => {
  const index = items.value.findIndex(i => i.id === item.id);
  items.value[index].editing = false;
  deleteId = "";
}

const showDoubleConfirm = (item) => {
  const index = items.value.findIndex(i => i.id === item.id);
  items.value[index].editing = true;
  deleteId = items.value[index].id;
}
</script>

<template>
  <table>
  <thead>
    <tr>
      <th scope="col" v-for="header in headers" :key="header.id"  style="text-align:left">{{ header.value }}</th>
    </tr>
  </thead>
  <tbody v-for="item in items" :key="item.id" >
    <tr v-bind:style="{ backgroundColor: item.editing ? '#FFCC00' : '#FFFFFF' }">
      <td width="20%">{{ item.label }}</td>
      <td width="20%"><small>{{ item.desc }}</small></td>
      <td width="10%">{{ item.price }}</td>
      <td width="20%">
        <button type="button" @click="minusStock(item)" :disabled="editItems.length > 0 || deleteId > 0">-</button>
        {{ item.stock }}
        <button type="button" @click="addStock(item)" :disabled="editItems.length > 0 || deleteId > 0">+</button>
      </td>
      <td width="20%">
        <button type="button" @click="editItem(item)" :disabled="editItems.length > 0 || deleteId > 0" v-bind:style="{ display: deleteId === item.id ? 'none' : 'inline-block' }">編輯</button>
        <button type="button" @click="showDoubleConfirm(item)" :disabled="editItems.length > 0 || deleteId > 0" v-bind:style="{ display: deleteId === item.id ? 'none' : 'inline-block' }">刪除</button>
        <button type="button" @click="cancelDelete(item)" :disabled="editItems.length > 0" v-if="deleteId === item.id">取消刪除</button>
        <button type="button" @click="deleteItem(item)" :disabled="editItems.length > 0" v-if="deleteId === item.id">確認刪除</button>
      </td>
    </tr>
  </tbody>
</table>
<hr />
<button type="button" @click="createItem()" :disabled="editItems.length > 0 || deleteId > 0">新增商品</button>

<h2 v-if="editorTitle">==========================={{ editorTitle }}===========================</h2>
<table v-if="editItems.length > 0">
  <thead>
    <tr>
      <th scope="col" v-for="header in headers" :key="header.id">{{ header.value }}</th>
    </tr>
  </thead>
  <tbody v-for="edit in editItems" :key="edit.id">
    <tr>
      <td><input type="text" v-model="edit.label" value="edit.label"></td>
      <td><input type="text" v-model="edit.desc" value="edit.desc"></td>
      <td><input type="text" v-model="edit.price" value="edit.price"></td>
      <td><input type="text" v-model="edit.stock" value="edit.stock"></td>
      <td>
        <button type="button" class="col" @click="cancelItem()" >取消</button>
        <button type="button" class="col" @click="confirmItem()" v-if="editorTitle === 'Adding Area'" :disabled="!validateInput()">確認新增</button>
        <button type="button" class="col" @click="updateItem()" v-if="editorTitle === 'Editing Area'" :disabled="!validateInput()">更新</button>
        <button type="button" class="col" @click="cloneItem()" v-if="editorTitle === 'Editing Area'" :disabled="!validateInput()">複製</button>
      </td>
    </tr>
    <tr style="color:red">
      <td>{{ !validateInput() ? errorText1 : "" }}</td>
      <td>{{ !validateInput() ? errorText2 : "" }}</td>
      <td>{{ !validateInput() ? errorText3 : "" }}</td>
      <td>{{ !validateInput() ? errorText4 : "" }}</td>
      <td></td>
    </tr>
  </tbody>
</table>
<!-- <div class="row" v-if="editItems.length > 0">
  <button type="button" class="col" @click="cancelItem()" >取消</button>
  <button type="button" class="col" @click="confirmItem()" v-if="editorTitle === 'Adding Area'">確認新增</button>
  <button type="button" class="col" @click="updateItem()" v-if="editorTitle === 'Editing Area'">更新</button>
  <button type="button" class="col" @click="cloneItem()" v-if="editorTitle === 'Editing Area'">複製</button>
</div> -->

</template>

<style scoped>

</style>
