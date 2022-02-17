<template>
  <div class="Pedidos">
      <main>
       <Header/>

         <h4>Todos os pedidos:</h4>
        <div v-for="pedi in pedidos" :key="pedi._id">
           <CardPedidos
        :produtoId="pedi.produtoId"
        :clienteCpf="pedi.clienteCpf"
        :valorUnitario="pedi.valorUnitario"
        :valorTotal="pedi.valorTotal"
        :quantidade="pedi.quantidade"
        />
         </div>
      
        <Footer/>
        </main>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Footer from "../components/Footer.vue";
import CardPedidos from "../components/CardPedidos.vue"
export default {
      Name:"Pedidos",
      components:{
        Header,
        Footer,
        CardPedidos
      },
      data: function(){
        return{
          pedidos : [

          ],
        }
      },
      methods:{
        getPedidos: async function(){
          const URL= "http://localhost:3000/pedidos"
          const result = await fetch(URL)
          .then((res)=> res.json())
          .then((res) => res)
          .catch((error)=>{
          return{
          error: true,
          message : error,
        };
      });
      
      if(!result.error){
        this.pedidos = result;
      }
        }
        },
      
      created : function(){
        this.getPedidos();
      }
      
 

}
</script>

<style>

</style>