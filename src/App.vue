<template>
  <div id="app" class="wrapper">
    <my-header text="Customers to call"/>
    <div class="filter-wrapper">
    <input class="input-style" v-model="searchTerm">
    <select v-model="sortBy">
      <option v-for="opt in formatList" :key="opt">{{opt}}</option>
    </select>
    </div>
    <ul>
      <li
        v-for="customer in customerList"
        :style="customer.called && {'textDecoration':'line-through'}"
        :key="customer.name"
        v-on:click="setCalled(customer.name)"
      >{{customerToString(customer)}}</li>
    </ul>
      <my-header text="Add more customers to call"/>
    <auto-complete :list="potentialCustomerStrings" :select="addCustomer"/>
  </div>
</template>
<script>
import MyHeader from "./components/MyHeader.vue";
import AutoComplete from "./components/AutoComplete.vue";

export default {
  name: "app",
  components: {
    MyHeader,
    AutoComplete
  },
  data() {
    return {
      sortBy: "age",
      searchTerm: "",
      customers: [
        { name: "Alice", age: 73, called: false },
        { name: "Maja", age: 28, called: false },
        { name: "Lilly", age: 23, called: false },
        { name: "Ella", age: 43, called: false },
        { name: "Catta", age: 28, called: false }
      ],
      potentialCustomers: [
        { name: "Wilma", age: 26, called: false },
        { name: "Ebba", age: 89, called: false },
        { name: "Olivia", age: 19, called: false },
        { name: "Astrid", age: 43, called: true },
        { name: "Alma", age: 34, called: false },
        { name: "Elsa", age: 29, called: false },
        { name: "Alva", age: 36, called: false },
        { name: "Berit", age: 17, called: false },
        { name: "Carolina", age: 53, called: false }
      ]
    };
  },
  computed: {
    formatList() {
      return ["name", "age", "called"];
    },
    customerList() {
      return this.sort(this.customers, this.sortBy).filter(it =>
        this.customerToString(it)
          .toUpperCase()
          .includes(this.searchTerm.toUpperCase())
      );
    },
    customerStrings() {
      return this.customerList.map(this.customerToString);
    },
    potentialCustomerStrings() {
      return this.potentialCustomers.map(this.customerToString);
    }
  },
  methods: {
    customerToString(item) {
      return `${item.name}-${item.age}`;
    },
    sort(list, sortBy) {
      return list.sort((a, b) => (a[sortBy] < b[sortBy] ? -1 : 1));
    },
    setCalled(name, callStatus) {
      this.customers = this.customers.map(c =>
        c.name === name ? { ...c, called: callStatus || !c.called } : c
      );
    },
    addCustomer(custString) {
      const [name, age] = custString.split("-");
      this.customers.push({ name, age: parseInt(age, 10), called: false });
      this.potentialCustomers = this.potentialCustomers.filter(it => it.name !== name);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  text-align: left;
  list-style: none;
  cursor: pointer;
}

.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.filter-wrapper {
  display: flex;
  width: 100%;
  justify-content: space-around;
}

.input-style {
  width: 300px;
}
</style>
