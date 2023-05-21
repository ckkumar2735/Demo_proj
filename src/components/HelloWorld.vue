<template>
  <div class="container mx-auto p-4">
    <h1 class="text-3xl font-bold mb-6">Expense Data</h1>
    <form @submit.prevent="addItem" class="mb-8">
      <div class="flex items-center">
        <input v-model="newItem.id" type="text" class="border rounded-l py-2 px-4 w-32 focus:outline-none" placeholder="ID" required>
        <input v-model="newItem.name" type="text" class="border py-2 px-4 w-64 focus:outline-none" placeholder="Name" required>
        <input v-model="newItem.amount" type="number" class="border py-2 px-4 w-32 focus:outline-none" placeholder="Amount" required>
        <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-r">Add Item</button>
      </div>
    </form>
    <table class="table-center w-full  ">
      <thead>
        <tr>
          <th class="px-4 py-2">ID</th>
          <th class="px-4 py-2">Name</th>
          <th class="px-4 py-2">Amount</th>
          <th class="px-4 py-2"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="item.id">
          <td class="border px-4 py-2">{{ item.id }}</td>
          <td class="border px-4 py-2">{{ item.name }}</td>
          <td class="border px-4 py-2">{{ item.amount }}</td>
          <td class="border px-4 py-2">
            <button @click="deleteItem(index)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="mt-8">
      <h2 class="text-2xl font-bold">Total Amount: {{ totalAmount }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      newItem: {
        id: '',
        name: '',
        amount: 0
      }
    };
  },
  computed: {
    totalAmount() {
      return this.items.reduce((sum, item) => sum + parseInt(item.amount), 0);
    }
  },
  methods: {
    addItem() {
      this.items.push({ ...this.newItem });
      this.newItem.id = '';
      this.newItem.name = '';
      this.newItem.amount = 0;
      this.saveItemsToLocalStorage();
    },
    deleteItem(index) {
      this.items.splice(index, 1);
      this.saveItemsToLocalStorage();
    },
    saveItemsToLocalStorage() {
      localStorage.setItem('items', JSON.stringify(this.items));
    },
    loadItemsFromLocalStorage() {
      const items = localStorage.getItem('items');
      if (items) {
        this.items = JSON.parse(items);
      }
    }
  },
  mounted() {
    this.loadItemsFromLocalStorage();
  }
};
</script>

<style scoped>
.table-auto {
  width: auto;
  text-align:center;
}


</style>
