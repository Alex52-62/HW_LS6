<template>
  <div id="Dashboard" :class="[$style.wrapper]">
    <header>
      <h1>My personal cost</h1>
    </header>

    <main>
      <div class="content-page">
        <router-view />
      </div>
      <PaymentsDisplay
        :list="pageList"
        :sl="catSl"
        @sendSelected="checkIn($event)"
        @addNewCategory="addingCat($event)"
      />
      <br />
      <AddPayment :cat-sl="catSl" @addNewPayment="addData" />
      <br />
      <CategorySelect />
      <h4>Total: {{ getFPV }} units</h4>
      <Pagination :pd="paymentsList" @sendPage="getPage($event)" />
      <!--     <button @click="showPaymentsForm">Show Payments Form</button>
          <button @click="closePaymentsForm">Close</button>
              <button @click="popUp">Show popUp</button>-->
    </main>
  </div>
</template>

<script>
import PaymentsDisplay from "../components/PaymentsDisplay.vue";
import AddPayment from "../components/AddPayment.vue";
import Pagination from "../components/pagination.vue";
import CategorySelect from "../components/CategorySelect.vue";
import { mapMutations, mapGetters, mapActions } from "vuex";
export default {
  name: "Dashboard",
  components: {
    PaymentsDisplay,
    CategorySelect,
    AddPayment,
    Pagination,
  },
  data: () => ({
    catSl: "",
    pageList: [],
    page: "",
    curPage: 1,
    data: {
      form: "qw",
    },
  }),

  methods: {
    checkIn(category) {
      this.catSl = category;
    },
    addingCat(category) {
      this.addCategory(category);
      console.log("push to state", category);
    },
    getPage(page) {
      this.pageList = page;
    },
    /*addData(data) {
      this.paymentsList = [...this.paymentsList, data];
    },*/
    ...mapMutations([
      "setPaymentListData",
      "addDataToPaymentsList",
      "addCategory",
    ]),
    ...mapActions(["fetchData", "fetchCategory"]),
    addData(data) {
      console.log("push to state", data);
      // this.paymentsList.push(data)
      // this.paymentsList = [...this.paymentsList, data]
      this.addDataToPaymentsList(data);
    },
  },
  computed: {
    ...mapGetters({
      paymentsList: "getPaymentList",
      categories: "getCategoryList",
    }),
    // currentElements() {
    //   const { n, curPage } = this;
    //   return this.paymentsList.slice(n * (curPage - 1), n * (curPage - 1) + n);
    // },
    getFPV() {
      return this.$store.getters.getFullPyamentValue;
    },
    // paymentsList(){
    //   return this.$store.getters.getPaymentList
    // }
  },
  created() {
    //this.paymentsList = list();

    if (this.paymentsList.length == 1) {
      this.fetchData();
    } else {
      //break;
      console.log(this.paymentsList.length);
    }
    //console.log(this.paymentsList.length);
    if (!this.categories.length) {
      this.fetchCategory();
    }
  },
  mounted() {
    const page = this.$route.params.page || 1;
    this.curPage = Number(page);
  },
};
</script>

<style lang="scss" module>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  display: flex;
  align-items: center;
  flex-direction: column;
}

h4 {
  color: red;
  display: flex;
  justify-content: center;
  font-weight: 500;
  background: gainsboro;
}
</style>