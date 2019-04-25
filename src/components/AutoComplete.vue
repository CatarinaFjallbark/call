<template>
  <div class="component-wrapper-drop">
    <div class="drop-down-relative">
      <div class="drop-down-style">
        <div class="head-drop-down">
          <div class="component-wrapper">
            <div class="input-wrapper">
              <input class="input-style" @input="e => input(e)">
            </div>
          </div>
        </div>
      </div>
      <div
        :style="{
          height: filteredList.length > 10 ? '400px' : filteredList.length > 0 ? 'auto' : '0', overflowY: filteredList.length > 10 ? 'scroll' : 'hidden'
        }"
        class="drop-down-list-style"
      >
        <ul>
          <li
            v-for="(value, index) in filteredList"
            :key="`${index}${value}`"
            :id="`item${index}`"
            :class="{selected: index === selectedIndex}"
            class="drop-down-item"
            @mouseover="onHover(index)"
            @click="select(value)"
          >
            <a>{{ value }}</a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AutoComplete",
  props: {
    list: {
      type: Array,
      required: true
    },
    select: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      selectedIndex: 0,
      searchTerm: null
    };
  },
  watch: {
    list(newVal, oldVal) {
      console.log('newVal', newVal)
      console.log('oldVal', oldVal)
    },
  },
  methods: {
    onHover(index) {
      this.selectedIndex = index || 0;
    },
    input(e) {
      this.searchTerm = e.target.value.toUpperCase();
    }
  },
  computed: {
    filteredList() {
      return this.searchTerm === null
        ? []
        : this.list.filter(val =>
            val.toUpperCase().includes(this.searchTerm.toUpperCase())
          );
    }
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.drop-down-relative {
  position: relative;
  width: 200px;
}

.drop-down-item {
  color: var(--grey4);
}

.component-wrapper-drop {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 0 0;
}

.head-drop-down {
  width: 100%;
}

.drop-down-style {
  height: 30px;
  width: 100%;
  cursor: pointer;
}

.drop-down-list-style {
  box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.16);
  background-color: var(--white);
  width: inherit;
  z-index: 300;
  right: 0;
  min-width: 300px;
  background-color: white;
}

.input-style {
  width: 300px;
}

a {
  text-decoration: none;
  user-select: none;
  color: inherit;
  line-height: 15px;
}

ul {
  list-style-type: none;
  padding: 8px 0;
}

li {
  height: 34px;
  display: flex;
  align-items: center;
  padding-left: 10px;
  cursor: pointer;
  user-select: none;
}

li.selected {
  background-color: gainsboro;
  color: grey;
}

</style>
