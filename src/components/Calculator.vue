<template>
  <div class="hello">
    <h1 class="title is-1">Beer calculator</h1>
    <div class="columns">
      <div class="column is-half">
        <form>
          <div class="field">
            <label class="label">Number of beers</label>
            <div class="select">
              <select v-model="quantity">
                <option value="24">24</option>
                <option value="16">16</option>
                <option value="6">6</option>
              </select>
            </div>
          </div>
          <div class="field">
            <label class="label">Size of can/bottle (ml)</label>
            <div class="select">
              <select v-model="size">
                <option value="330">330</option>
                <option value="355">355</option>
                <option value="375">375</option>
                <option value="500">500</option>
              </select>
            </div>
          </div>
          <div class="field">
            <label class="label">ABV (%)</label>
            <div class="control">
              <input class="input" type="number" v-model="abv" />
            </div>
          </div>
          <div class="field">
            <label class="label">Price ($)</label>
            <div class="control">
              <input class="input" type="number" v-model="price" />
            </div>
          </div>
          <h2 class="title is-2">Total:</h2>
          <table class="table">
            <tbody>
              <tr>
                <th>Excise</th>
                <td>${{ totalExcise.toFixed(2) }}</td>
              </tr>
              <tr>
                <th>Cost after Exise</th>
                <td>${{ (price - totalExcise).toFixed(2) }}</td>
              </tr>
              <tr>
                <th>Cost per ltr of beer</th>
                <td>${{ costPerLtr.toFixed(2) }}</td>
              </tr>
              <tr>
                <th>Cost per ltr of beer after Excise</th>
                <td>${{ costPerLtrPostExcise.toFixed(2) }}</td>
              </tr>
            </tbody>
          </table>
        </form>
      </div>
    </div>
  </div>
</template>

<script>

// from here: https://www.ato.gov.au/Business/Excise-on-alcohol/Lodging,-paying-and-rates---excisable-alcohol/Excise-duty-rates-for-alcohol/
const EXCISE = 51.31;

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      quantity: 0,
      size: 0,
      abv: 0,
      price: 0,
    };
  },
  computed: {
    totalExcise() {
      return this.totalVolume * (this.abv / 100) * EXCISE;
    },
    vessleInLtrs() {
      return this.size / 1000;
    },
    totalVolume() {
      return this.vessleInLtrs * this.quantity;
    },
    costPerLtr(){
      return this.price * this.vessleInLtrs;
    },
    costPerLtrPostExcise(){
      return (this.price - this.totalExcise) * this.vessleInLtrs;
    },
  },
};
</script>
