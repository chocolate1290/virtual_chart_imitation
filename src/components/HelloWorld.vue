<template>
  <div>
    <!-- {{ momomo }} -->
    <!-- <button v-on:click="ring">button</button> -->
    <v-sparkline
      :value="value"
      :smooth="radius || false"
      :padding="padding"
      :line-width="width"
      :stroke-linecap="lineCap"
      color="purple"
      :fill="fill"
      :type="type"
      :auto-line-width="autoLineWidth"
      
    ></v-sparkline>
    <v-text-field
      label="volatility Max"
      hide-details="auto"
      v-model="max"
    ></v-text-field>
    <v-text-field
      label="volatility min"
      hide-details="auto"
      v-model="min"
    ></v-text-field>
    <!-- <v-input v-model="max" height="50px" background-color="#BBDEFB"/> -->
      <!-- <v-input /> -->
    <v-data-table :headers="headers" :items="rates"> </v-data-table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
      max:0.5,
      min:-0.5,
    width: 0.5,
    radius: 0,
    padding: 8,
    lineCap: 'round',
    value: [100],
    fill: false,
    type: 'trend',
    autoLineWidth: false,
      momomo: "",
      headers: [
        { text: "ID", value: "id" },
        { text: "通貨", value: "currency" },
        { text: "レート", value: "rate" },
      ],
      rates2: [
        {
          id: 1,
          currency: "JPY",
          rate: "104",
        },
      ],
      response: "",
    };
  },
  methods: {
    ring() {
      this.axios
        .get("https://api.exchangeratesapi.io/latest")
        .then((Response) => {
          console.log(Response.data.rates);
          this.response = Response.data.rates;
        });
    },
    tick(){
      const max = Number(this.max);
      const min = Number(this.min);
      if(isNaN(max) || isNaN(min)) return;
      this.value.push((this.value.slice(-1)[0])+Math.random() * (max-min) + min)
    }
  },
  computed: {
    rates() {
      return Object.keys(this.response).map((currency, index) => {
        return {
          id: index+1,
          currency: currency,
          rate: this.response[currency],
        };
      });
    },
  },
  created() {
    this.ring();
        setInterval(this.tick, 10);
  },
};
</script>