<template>
  <div class="payments-list">
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>date</th>
          <th>
            category
            <select v-model="selected">
              <option
                v-for="(category, sl) in getCategoryList"
                @submit="getSelected()"
                :key="sl"
              >
                {{ category }}
              </option>
            </select>
          </th>
          <th>
            <input
              class="i"
              v-model.trim="category"
              @keyup.enter="addCat(category)"
              placeholder="add category"
            />
          </th>
          <th>value</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(item, idx) in list" :key="idx">
          <td>{{ idx + 1 }}</td>
          <td>{{ item.date }}</td>
          <td align="center">{{ item.category }}</td>
          <td></td>
          <td align="center">{{ item.value }}</td>
          <td>
            <details class="dropdown">
              <summary class="threePoints"></summary>

              <div class="dropdown-menu show">
                <button class="dropdown-item" @click="popUp"><img src="../assets/edit.png">Edit</button>
                <button class="dropdown-item" @click="erase(item)"><img src="../assets/delete.png">Delete</button>
              </div>
            </details>
            <!--<button class="threePoints" @click="popUp"></button>
            <div class="content"></div>-->
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  name: "PaymentsDisplay",
  methods: {
    popUp() {
      this.$modal.show("AddPayment", { name: "AddPayment" });
    },
    erase(item){
      //console.log([this.list[0]]);
      //console.log(idx);
      //this.list.splice(idx, 1)   удаляет только из текущего окна
      console.log(item.id);
      this.delDataFromPaymentsList(item);
    },
            ...mapMutations([
      "delDataFromPaymentsList",
    ]),
    addCat(category) {
      this.$emit("addNewCategory", category);
      console.log("addNewCategory", category);
    },
  },
  watch: {
    selected() {
      const sl = this.selected;
      this.$emit("sendSelected", sl);
    },
  },
  computed: {
    getCategoryList() {
      return this.$store.getters.getCategoryList;
    },
  },
  props: {
    list: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      selected: "",
      category: "",
    };
  },
};
</script>

<style scoped>
.payments-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 2px solid grey;
  width: 60%;
  margin: auto;
  padding-bottom: 30px;
}

td {
  border-bottom: 1px solid grey;
}

th {
  padding: 20px;
}

.i {
  width: 120px;
  background-color: #fff3ce;
}

.threePoints:after {
  content: "\2807";
  font-size: 25px;
}

.dropdown-item {
  border: none;
  background-color: inherit;
  padding: 14px 28px;
  font-size: 16px;
  cursor: pointer;
  display: inline-block;
}

.dropdown-item:hover {
  background: #eee;
}

/* Убираем стандартный маркер Chrome */
details summary::-webkit-details-marker {
  display: none;
}
/* Убираем стандартный маркер Firefox */
details > summary {
  list-style: none;
}
</style>