<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      class="flex justify-center mx-12 text-2xl"
    >
      TOTAL AMOUNT: {{ test }}
    </v-app-bar>

    <v-main>
      <div v-for="(positins, i) in allPositions" :key="i">
        <div class="flex flex-row justify-center gap-x-3 pt-8 border-b-2">
          <div>
            <combo-box
              :data="currency"
              label="Currency"
              :width="135"
              v-model="selectedCurrency"
            ></combo-box>
          </div>
          <div class="w-[175px]">
            <v-text-field
              outlined
              label="Amount"
              v-model="currentAmount"
              type="number"
            >
            </v-text-field>
          </div>
          <div class="w-[350px]">
            <combo-box
              outlined
              label="Account Number"
              v-model="accountNumber"
              :data="accountNumberDataForm"
              :width="350"
            >
            </combo-box>
          </div>
          <div>
            <combo-box
              label="Payment Method"
              :width="195"
              v-model="selectedPaymentMethod"
              :data="paymentMethods"
            ></combo-box>
          </div>
          <div class="w-[730px]">
            <v-text-field outlined label="Description" v-model="description">
            </v-text-field>
          </div>
          <div class="border-l-2 pl-3 h-14">
            <v-icon class="icon cursor-pointer" color="#4A90E2"
              >mdi-delete</v-icon
            >
            <v-icon
              @click="saveHandle"
              class="icon cursor-pointer"
              color="#4A90E2"
              >mdi-content-save</v-icon
            >
          </div>
        </div>
      </div>
      {{ allPositions }}
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
      selectedCurrency: "",
      currentAmount: 0,
      accountNumber: "",
      description: "",
      selectedPaymentMethod: "",
      currency: [
        "TRY",
        "USD",
        "EUR",
        "GBP",
        "JPY",
        "CAD",
        "AUD",
        "CHF",
        "CNY",
        "RUB",
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
      allPositions: [
        {
          currency: "",
          amount: 0,
          accountNumber: "",
          paymentMethod: "",
          desc: "",
        },
      ],
      test: 0
    };
  },
  computed: {
    accountNumberDataForm() {
      return this.accountNumberData.map(
        (item) => `${item.account_number} - ${item.caption}`
      );
    },
    totalAmount() {
      return this.allPositions.map(
        (item) => this.test += item.amount
      )
    }
  },
  methods: {
    saveHandle() {
      const payload = {
        currency: this.selectedCurrency,
        amount: this.currentAmount,
        accountNumber: this.accountNumber,
        paymentMethods: this.paymentMethods,
        desc: this.description,
      };

      this.allPositions = [...this.allPositions, payload];
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
