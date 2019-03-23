<template>
  <div>
    <v-toolbar color="blue-grey" dark fixed app clipped-right>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>后台管理</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-navigation-drawer v-model="drawer" fixed app>
      <v-list dense>
        <v-list-tile @click.stop="left = !left">
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Open Temporary Drawer</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-content>
      <v-layout justify-center align-center>
        <v-btn @click="nav('paycheck')">付款确认</v-btn>
        <v-btn @click="nav('printman')">打印管理</v-btn>
        <v-btn @click="nav('deliverman')">配送确认</v-btn>
      </v-layout>

      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <router-view></router-view>
        </v-layout>
      </v-container>
    </v-content>
    <v-footer color="blue-grey" class="white--text" app>
      <v-spacer></v-spacer>
      <span>RUC Print Online 后台管理&copy; 2019</span>
      <v-spacer></v-spacer>
    </v-footer>
  </div>
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
    this.$router.push("paycheck");
  }
};
</script>
