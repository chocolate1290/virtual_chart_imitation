<template>
  <div>
    {{ momomo }}
    <button v-on:click="ring">button</button>
    <v-sparkline
      :value="value"
      :smooth="radius || false"
      :padding="padding"
      :line-width="width"
      :stroke-linecap="lineCap"
      :fill="fill"
      :type="type"
      :auto-line-width="autoLineWidth"
      
    ></v-sparkline>
    <v-data-table :headers="headers" :items="rates"> </v-data-table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
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
      this.value.push((this.value.slice(-1)[0])+Math.random() * 0.2 - 0.1)
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
        setInterval(this.tick, 100);
  },
};
</script>