<template>
  <h1>请指定要访问的界面！</h1>
</template>

<script>
// import PaymentCheck from "../components/PaymentCheck";

export default {
  // components: {
  //   PaymentCheck
  // },
  data: () => ({
    drawer: false,
    right: false,
    left: false
  }),
  props: {
    source: String
  },
  methods: {
    nav(addr) {
      this.$router.push(addr);
    }
  },
  created() {
    // this.$router.push("paycheck");
  }
};
</script>
