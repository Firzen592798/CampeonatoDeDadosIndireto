<template>
  <h1>Cadastro</h1>
  <label for="id">ID</label>
  <input id="id" type="text" v-model="id"/>
  
  <label for="descricao">Descrição</label>
  <input id="descricao" type="text" v-model="descricao"/>
  <br>
  <button @click="$emit('cadastrar', id, descricao)">Cadastrar</button>
</template>
<script>
export default {
    name: 'Form',
    components: {

    },
    data() {
        return {
            id: "",
            descricao: "",
        
        }
    },
    mounted(){

    },
    methods: {
        showModal(){
            this.$swal('Hello Vue world!!!');
        }
    }
}

</script>