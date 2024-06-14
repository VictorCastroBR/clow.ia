<script setup>
  import { ref } from 'vue'
  import axios from 'axios'

  const image = ref(new FormData)
  const mode = ref(1)
  const testeList = ref([])

  const sendClown = async () => {
    const url = ''
    axios.post(url, {})
    .then((e) => {

    })
    .catch((err) => {
      console.error(err)
    })
    .finally((e) => {

    })
  }

  const openModal = async () => {
    $('#exampleModal').modal('show');
    
    const url = 'http://127.0.0.1:5000/tests'; // Corrigindo o URL
    
    try {
        const response = await axios.get(url);
        testeList.value = response.data.tests; // Atribuindo o valor corretamente
        console.log('Response:', testeList.value); // Logando a resposta para verificação
    } catch (err) {
        console.error('Error:', err); // Logando erros
    }
  };

  const exportJson = async () => {
    const blob = new Blob([JSON.stringify(testeList.value)], { type: "application/json" });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = 'data.json';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
  }

  const onUploadImage = (e) => {
    let files = e.target.files || e.dataTransfer.files
    image.value = new FormData
    image.value.append('files[]', files[0])
    

    sendClown()
    saveResult()

    mode.value = 1
  }

  const saveResult = async (result) => {
    const url = 'http://127.0.0.1:5000/create-test'; // Corrigindo o URL
    
    try {
        const response = await axios.post(url, {result: result});
        testeList.value = response.data; // Atribuindo o valor corretamente
        console.log('Response:', response); // Logando a resposta para verificação
    } catch (err) {
        console.error('Error:', err); // Logando erros
    }
  }

</script>

<template>
  <main>
    <div class="text-end">
      <button @click="openModal" class="btn btn-secondary m-2">
        testes
      </button>
    </div>
    <div class="container text-center">
      <div v-if="mode === 0" class="center-div">
        <h1 class="text-center">CLOWN.IA 🤡</h1>
        <p class="text-center">
          Realize uma análise de imagem utilizando inteligência artificial para identificar se a imagem contém um palhaço.
        </p>
        <div class="file-upload" onclick="document.getElementById('fileInput').click();">
          <input @change="onUploadImage" id="fileInput" type="file" />
          <p>Arraste aqui para anexar</p>
        </div>
      </div>
      <div v-else class="center-div">
        <h1 class="text-center">100% 🤡</h1>
        <p class="text-center">
          Isto é um palhaço
        </p>
        <button @click="exportJson" class="custom-btn">Exportar Json</button>
        <button @click="mode = 0" class="custom-btn">Enviar novamente</button>
      </div>
    </div>

    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">testes</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <p v-for="test in testeList">
              id: {{ test.id }} - {{ test.result }}
            </p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>

  </main>
</template>

<style scoped>
main {
  background: linear-gradient(to right, #ff7e5f, #feb47b);
}
.container {
  /* Fundo gradiente */
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
}
.center-div {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  text-align: center;
}
.file-upload {
  border: 2px dashed #ddd;
  border-radius: 10px;
  padding: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
.file-upload:hover {
  background-color: #f1f1f1;
}
.file-upload input[type="file"] {
  display: none;
}
.custom-btn {
  background-color: #ff7e5f;
  border: none;
  border-radius: 50px;
  color: white;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  margin-top: 20px;
}
.custom-btn:hover {
  background-color: #feb47b;
  transform: scale(1.05);
}
</style>
