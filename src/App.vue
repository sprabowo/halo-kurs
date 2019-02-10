<template>
  <div id="app" class="container-500">
    <section>
      <h1>HaloKurs</h1>
      <p>realtime exchange currency</p>
    </section>
    <section class="header">
        <div class="row">USD - United State Dollars</div>
        <div class="row">
            <div style="width:15%;font-weight:bold;">USD</div>
            <div style="width:85%;"><input v-on:keypress="__handleInput(event)" v-model="amount" class="input_amount" type="text"/></div>
        </div>
    </section>
    <my-list-currency @delete-list="__deleteList" @add-list="__addList" v-if="show" :data-list="rateShow" :data-list-all="rateShowAdd" :data-input="amount" :data-amount="results"/>
  </div>
</template>

<script>
import MyListCurrency from './components/ListCurrency.vue'

export default {
  name: 'app',
  components: {
    MyListCurrency
  },
  computed: {
    rateShow () {
      return Object.keys(this.rates).filter(o => this.rates[o])
    },
    rateShowAdd () {
      return Object.keys(this.rates).filter(o => !this.rates[o])
    }
  },
  data () {
    return {
      show: false,
      amount: 10,
      results: {},
      rates: {
        'IDR': true,
        'EUR': false,
        'CAD': false,
        'GBP': false,
        'CHF': false,
        'SGD': false,
        'INR': false,
        'MYR': false,
        'JPY': false,
        'KRW': false
      }
    }
  },
  methods: {
    __handleInput (event) {
      event = event || window.event
      let charCode = (event.which) ? event.which : event.keyCode
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        event.preventDefault()
      } else {
        return true
      }
    },
    __deleteList (val) {
      if (this.rates && this.rates.hasOwnProperty(val)) {
        this.rates[val] = false
      }
    },
    __addList (val) {
      if (this.rates && this.rates.hasOwnProperty(val)) {
        this.rates[val] = true
      }
    }
  },
  mounted () {
    try {
      this.show = false
      fetch('https://api.exchangeratesapi.io/latest?base=USD')
        .then(res => res.json())
        .then(response => {
          this.results = response.rates
          this.show = true
        })
        .catch(() => { this.show = false })
    } catch (error) {
    }
    document.title = `HaloKurs`
  }
}
</script>

<style lang="scss">
:root {
  font-size: 1rem;
}
.row {
    display: flex;
    width: 100%;
}
.container-500 {
  @media screen and (max-width:767px) {
    width: 100%;
  }
  width: 484px;
  margin: 0 auto;
}
section {
  @media screen and (max-width: 767px) {
    margin-bottom: 16px;
  }
  margin-bottom: 32px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  @media screen and (max-width: 767px) {
    margin-top: 32px
  }
  margin-top: 48px;
}
.header {
    @media screen and (max-width: 767px) {
        padding: 16px
    }
    border: 1px solid #c0c0c0;
    border-radius: 7px;
    padding: 32px;
    text-align: left;
}
.input_amount {
    border-radius: 7px;
    text-indent: 8px;
    width: 100%;
    border: 1px solid #f0f0f0;
}
.row {
    display: flex;
    width: 100%;
}
</style>
