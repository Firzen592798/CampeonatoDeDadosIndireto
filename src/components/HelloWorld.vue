<template>
  <h1>{{ msg }}</h1>
  <button @click="count++">count is: {{ count }}</button>
  <p>Edit <code>components/HelloWorld.vue</code> to test hot module replacement.</p>
  <Form @cadastrar="cadastrar"/>

  <h2>Lista</h2>
  <table style="margin-left: 25%; width:50%">
    <thead>
      <td>ID</td>
      <td>Descrição</td>
    </thead>
    <tbody>
      <tr v-for="(item, index) in lista" :key="item.id">
        <td>
          {{item.id}}
        </td>
        <td>{{item.descricao}}</td>
        <td> <button @click="deletar(index)">Deletar</button> </td>
      </tr>  
    </tbody>
  </table>
</template>
<script>
  import Form from './Form.vue'
  export default {
    name: 'HelloWorld',
    props: {
      msg: String
    },
    components: { Form },
    data() {
      return {
        count: 0,
        lista: []
      }
    },
    methods: {
      cadastrar(id, descricao){
        if(id && descricao)
          this.lista.push({id: id, descricao: descricao});
      },
      deletar(index){
        this.$swal.fire(
          {
            title: 'Deletar?',
            showCancelButton: true,
            confirmButtonText: `Save`,
            denyButtonText: `Don't save`,
          }
        ).then((result) => {
          /* Read more about isConfirmed, isDenied below */
          if (result.isConfirmed) {
            this.lista.splice(index, 1);
          } else  {
            this.$swal.fire('Não deu pra deletar', '', 'info')
          }
        });
      }
    }
  }
</script>
<style scoped>
  table, th, td {
    border: 1px solid black;
  }
</style>