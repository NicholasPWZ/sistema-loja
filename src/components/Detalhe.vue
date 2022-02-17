<template>
  <section class="detalhe">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <h2>Detalhe do Produto</h2>
          <img :src="produtos.img" :alt="produtos.name" />
          
        </div>
        <div class="col-md-9">
          <h2>{{produtos.title}}</h2>
          <p>R$:{{produtos.price}}</p>
        </div>
        <div class="col-md-3">
          <div class="detalhe__box-price">
            <p>Quantidade </p>
            <input id="qtd input" @input="toCalculate"  type="number" v-model="quantity"/>
          </div>
        </div>
        <div class="col-md-12">
          <hr/>
          <h3>Descrição: </h3>
          <p>
            Lorem Ipsum is simply dummy text of the printing and typesetting industry. 
            Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, 
            when an unknown printer took a galley of type and scrambled it to make a type 
            specimen book. It has survived not only five centuries, but also the leap into 
            electronic typesetting, remaining essentially unchanged.
            Lorem Ipsum is simply dummy text of the printing and typesetting industry. 
            Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, 
            when an unknown printer took a galley of type and scrambled it to make a type 
            specimen book. It has survived not only five centuries, but also the leap into 
            electronic typesetting, remaining essentially unchanged
          </p>
          <h4>Total : R$: {{total.toString().replace('.', ',')}}</h4>
        </div>
        <div class="col-md-12">
          <button class="pedidobutton" @click="open()"> Fazer Pedido</button>
          <p v-if="flag2">Pedido cadastrado com sucesso</p>
          <div v-if="flag">
         <section class="NovoPedido">
      <div>
        <hr>
        <h3>Novo pedido:</h3>
          
        <b-form-group >
          <b-button-toolbar>
            <b-input-group class="mt-3" >
              <label for="input1">Cpf cliente:  </label>
              <b-form-input class="ml-4" id="input1" size="sm" v-model="cpfCliente"></b-form-input>
             <b-input-group-append>
              <b-button @click="getDataCostumer" id="clientbutton">Buscar</b-button>
              </b-input-group-append>
            </b-input-group>
          </b-button-toolbar>
        </b-form-group>
        
          <ul>
            <li>Nome: {{clientes.nome}} {{clientes.sobrenome}}</li>
            <li>Cpf: {{clientes.CPF}}</li>
            <li>Data de nascimento: {{clientes.dataNascimento}}</li>
          </ul>
        <hr>
        <button class="pedidobutton" @click="createPedido()">Salvar Pedido</button>
      </div>
  </section>
  
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>

export default {
  name : "Detalhe",
  

  
  data: function(){
    return{
      clientes:[],
      produtos: [],
      quantity : 1,
      finalQuantity : 1,
      preco : 0,
      total : 0,
      cpfCliente:" ",
      flag: false,
      flag2: false,
    }
  },
  methods: {
    getDataProduct : async function(){
      const URL = "http://localhost:3000/produtos/" + this.$route.params.id
      const result = await fetch(URL)
      .then((res)=> res.json())
      .then((res) => res)
      .catch((error)=>{
        return{
          error: true,
          message : error,
        };
      });
      console.log(result)
      if(!result.error){
        this.produtos = result;
      }
    },
    getDataCostumer: async function(){
      const URL = "http://localhost:3000/clientes/busca/" + this.cpfCliente
      const result = await fetch(URL)
      .then((res)=>res.json())
      .then((res)=> res)
      .catch((error)=>{
        return{
          error: true,
          message: error
        }
      })
      if(!result.error){
        this.clientes = result;
      }
    },
    createPedido: async function () {
      const novoPedido = {
        produtoId: this.produtos._id,
        clienteCpf: this.clientes.CPF,
        valorTotal: this.total,
        valorUnitario: this.produtos.price,
        quantidade: this.finalQuantity,
      };
      
    
      const result = await fetch("http://localhost:3000/pedidos", {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(novoPedido),
      })
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.flag = !this.flag
        this.flag2 = !this.flag2;
      }
    },
    
    
    search : function(){
      this.getDataCostumer();
    },
    open : function(){
      this.flag = !this.flag
    },
  
    toCalculate : function(){
      this.finalQuantity = this.quantity;

      if(this.quantity <= 1){
        this.finalQuantity = 1
      }
      this.preco = parseInt(this.produtos.price);
      const total = this.preco * this.finalQuantity;
      this.total = total.toFixed(2)
    },
      },
    created: function(){
      this.getDataProduct();
    },
    

    
 }
 

</script>
<style>
  .detalhe {
    padding: 50px 0px;
  }

  .detalhe img{
    margin: 20px auto;
    display: block;
  }

  .detalhe__box-price{
    text-align: right;
    font-weight: bold;
  }

  #qtdinput {
    width: 50px;
    height: 30px;
    border-radius: 4px;
    padding: 0px 8px;
  }

  .pedidobutton {
    width: 170px;
    height: 40px;
    border-radius: 6px;
    background-color: #ae382b;
    color: #f5a022;
    border: none;
    font-weight: bold;
    display: block;
    margin: 30px auto;
  }
  #clientbutton{
    width: 80px;
    height: 35px;
    border-radius: 6px;
    background-color: #ae382b;
    color: #f5a022;
    border: none;
    font-weight: bold;
    
  }
  #input1{
    width: 400px;
  }

</style>