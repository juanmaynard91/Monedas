<template>
  <div class="container">
    <h1>Mercado de Monedas</h1>

    <input type="text" class="form-control bg-dark text-light my-4" placeholder="Buscar moneda" @keyup="encontrar()" v-model="buscar" />

    <table class="table table-dark table-hover">
      <thead>
        <tr>
          <th v-for="titulo in titulos" :key="titulo.id">{{ titulo }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in monedasFiltradas" :key="item.id">
          <td class="text-muted">
            {{ index + 1 }} <!-- para empezar del 1 -->
          </td>
          <td>
            <img :src="item.image" style="width: 2rem" class="me-2" />
            <span>{{ item.name }}</span>
            <span class="ms-2 text-uppercase text-muted">{{ item.symbol }}</span>
          </td>
          <td>$ {{ item.current_price }}</td>
          <td :class="[ item.price_change_percentage_24h > 0 ? ' text-success' : 'text-danger' ]">
            {{ item.price_change_percentage_24h }} %
          </td>
          <td>$ {{ item.total_volume.toLocaleString() }}</td> <!--toLocaleString(): para agregarle los puntos donde correspone a los valores -->
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      monedas: [],
      monedasFiltradas: [],
      titulos: ["#", "Moneda", "Precio", "Precio en porcentaje", "24h Volumen"],
      buscar: "",
    };
  },
  methods: {
    encontrar() {
      this.monedasFiltradas =
        this.monedas.filter((item) => item.name.toLowerCase().includes(this.buscar.toLowerCase()) ) ||
        this.monedas.filter((item) => item.symbol.toLowerCase().includes(this.buscar.toLowerCase()) );
    },
  },
  async mounted() {
    const res = await fetch("https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false");
    const data = await res.json();
    //console.log(data);
    this.monedas = data;
    this.monedasFiltradas = data;
  },
};
</script>

<style>
</style>
