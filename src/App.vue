<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      class="flex justify-center mx-12 text-2xl"
    >
      TOTAL AMOUNT: {{ totalAmount }}
    </v-app-bar>

    <v-main>
      <div v-for="(position, i) in positions" :key="i">
        <div class="flex flex-row justify-center gap-x-3 pt-8 border-b-2">
          <div>
            <combo-box
              :data="currency"
              label="Currency"
              :width="135"
              v-model="position.currency"
            ></combo-box>
          </div>
          <div class="w-[175px]">
            <v-text-field
              outlined
              label="Amount"
              v-model="position.amount"
              type="number"
            >
            </v-text-field>
          </div>
          <div class="w-[350px]">
            <combo-box
              outlined
              label="Account Number"
              v-model="position.accountNumber"
              :data="accountNumberDataForm"
              :width="350"
            >
            </combo-box>
          </div>
          <div>
            <combo-box
              label="Payment Method"
              :width="195"
              v-model="position.paymentMethod"
              :data="paymentMethods"
            ></combo-box>
          </div>
          <div class="w-[730px]">
            <v-text-field outlined label="Description" v-model="position.desc">
            </v-text-field>
          </div>
          <div class="border-l-2 pl-3 h-14">
            <v-icon
              class="icon cursor-pointer"
              color="#4A90E2"
              @click="deleteHandle(i)"
              >mdi-delete</v-icon
            >
            <v-icon
              @click="saveHandle(i)"
              class="icon cursor-pointer"
              color="#4A90E2"
              >mdi-content-save</v-icon
            >
          </div>
        </div>
      </div>
      {{ currency[1].floorNum }}
    </v-main>
  </v-app>
</template>

<script>
import ComboBox from "./components/ComboBox.vue";

export default {
  name: "App",

  components: {
    ComboBox,
  },

  data() {
    return {
      currency: [
        { text: "TRY", floorNum: 29 },
        { text: "USD", floorNum: 4 },
        { text: "EUR", floorNum: 1 },
        { text: "GBP", floorNum: 10 },
        { text: "JPY", floorNum: 11 },
        { text: "CAD", floorNum: 8 },
        { text: "AUD", floorNum: 9 },
        { text: "CHF", floorNum: 20 },
        { text: "CNY", floorNum: 13 },
        { text: "RUB", floorNum: 17 },
      ],
      accountNumberData: [
        {
          _id: "1",
          caption: "test",
          account_number: "0",
        },
        {
          _id: "2",
          caption: "test1",
          account_number: "5244",
        },
        {
          _id: "3",
          caption: "Digitastic",
          account_number: "00452",
        },
        {
          _id: "4",
          caption: "Company Name 1",
          account_number: "09567905",
        },
        {
          _id: "5",
          caption: "Company Name 2",
          account_number: "125145",
        },
      ],
      paymentMethods: ["Credit Card", "Cash", "Bank Transfer"],
      positions: [
        {
          currency: null,
          amount: 0,
          accountNumber: null,
          paymentMethod: null,
          desc: null,
        },
      ],
      totalAmount: 0,
    };
  },
  computed: {
    accountNumberDataForm() {
      return this.accountNumberData.map(
        (item) => `${item.account_number} - ${item.caption}`
      );
    },
  },
  methods: {
    saveHandle(i) {
      const payload = {
        currency: this.positions.currency,
        amount: this.positions.amount,
        accountNumber: this.positions.accountNumber,
        paymentMethods: this.positions.paymentMethod,
        desc: this.positions.desc,
      };
      this.positions = [...this.positions, payload];
      const selectedCurrencyText = this.positions[i].currency.text;
      this.currency.forEach((x) => {
        if (x.text == selectedCurrencyText) {
          this.totalAmount += this.positions[i].amount * x.floorNum;
          console.log(this.totalAmount);
        }
      });
      // this.totalAmount = Number(this.positions[i].amount);
    },
    deleteHandle(index) {
      const selectedCurrencyText = this.positions[index].currency.text;
      this.currency.forEach((x) => {
        if (x.text == selectedCurrencyText) {
          this.totalAmount -= this.positions[index].amount * x.floorNum;
          console.log(this.totalAmount);
        }
      });
      this.positions.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.icon {
  width: 56px;
  height: 56px;
  border: 1px solid #cccccc;
  border-radius: 4px 0px 0px 4px;
  opacity: 1;
}
</style>
