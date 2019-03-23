<template>
  <div>
    <v-toolbar color="blue-grey" dark fixed app clipped-right>
      <v-toolbar-title>后台管理</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <div>
            <v-layout>
              <v-flex xs8>
                <v-dialog
                  ref="dialog"
                  v-model="modal"
                  :return-value.sync="date"
                  persistent
                  lazy
                  full-width
                  width="290px"
                >
                  <v-text-field
                    slot="activator"
                    v-model="date"
                    label="Picker in dialog"
                    prepend-icon="event"
                    readonly
                  ></v-text-field>
                  <v-date-picker v-model="date" scrollable locale="zh">
                    <v-spacer></v-spacer>
                    <v-btn flat color="primary" @click="modal = false">Cancel</v-btn>
                    <v-btn flat color="primary" @click="$refs.dialog.save(date)">OK</v-btn>
                  </v-date-picker>
                </v-dialog>
              </v-flex>
              <v-flex xs2 offset-xs2>
                <v-btn block @click="onQuery">查询</v-btn>
              </v-flex>
            </v-layout>
            <v-data-table
              :headers="headers"
              :items="order_data"
              class="elevation-1"
              rows-per-page-text="每页显示："
              :rows-per-page-items="[10,20,50,100]"
            >
              <template slot="items" slot-scope="props">
                <td>{{ props.item.order_id }}</td>
                <td class="text-xs-center">{{ props.item.customer_building_name }}</td>
                <td class="text-xs-center">{{ props.item.customer_building_number }}</td>
                <td class="text-xs-center">{{ props.item.customer_room_number }}</td>
                <td class="text-xs-center">{{ props.item.total_copy_count }}</td>
                <td class="text-xs-center">{{ props.item.customer_name }}</td>
                <td class="text-xs-center">{{ props.item.customer_phone }}</td>
                <td class="text-xs-center"></td>
                <td class="text-xs-center">
                  <span v-if="props.item.order_deliver" class="green--text">已送达</span>
                  <span v-else class="red--text">未送达</span>
                </td>
                <td class="text-xs-center px-0">
                  <v-btn flat icon color="success" @click="check(props.item.order_id)">
                    <v-icon>check</v-icon>
                  </v-btn>
                </td>
              </template>
              <template slot="no-data">
                <v-subheader class="text-xs-center">空</v-subheader>
              </template>
            </v-data-table>
          </div>
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
var fly = require("flyio");
export default {
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    modal: false,
    dialog: false,
    headers: [
      {
        text: "订单ID",
        align: "left",
        value: "name",
        sortable: false
      },
      { text: "楼群", value: "building_name", sortable: false },
      { text: "楼号", value: "building_number", sortable: false },
      { text: "房间号", value: "room_number", sortable: false },
      { text: "份数", value: "copy", sortable: false },
      { text: "顾客姓名", value: "name", sortable: false },
      { text: "顾客电话", value: "phone", sortable: false },
      { text: "配送状态", value: "stat_deliver_confirm", sortable: false },
      { text: "操作", value: "actions", sortable: false }
    ],
    order_data: []
  }),

  created() {},

  methods: {
    onQuery() {
      let self = this;
      fly
        .post("https://rucprint.cn/api/querydeliver", {
          order_date:
            this.date.substr(0, 4) +
            this.date.substr(5, 2) +
            this.date.substr(8, 2)
        })
        .then(res => {
          self.order_data = res.data.data;
        });
    },
    check(id) {
      let self = this;
      fly
        .post("https://rucprint.cn/api/checkdeliver", {
          order_id: id
        })
        .then(() => {
          self.onQuery();
        });
    }
  }
};
</script>