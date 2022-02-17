<template>
  <section>
 
       
        <b-card>
          <b-card-text align="left" >
            <p>Código: {{produtoId}}</p>
            <p>CPF Cliente: {{clienteCpf}}</p>
            <p>Valor Unitário: {{valorUnitario}}</p>
            <p>Valor Total: {{valorTotal}}</p>
            <p>Quantidade: {{quantidade}}</p>
          </b-card-text>
        </b-card>
        
        
          </section>

</template>

<script>
export default {
    Name: "CardPedidos",
    props:{
        produtoId:String,
        clienteCpf:String,
        valorUnitario:String,
        valorTotal:String,
        quantidade:String,
    }
}
</script>

<style>

</style>