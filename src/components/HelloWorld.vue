<template>
  <div class="container">
    <input v-on:keyup.enter="send" v-model="prompt" class="form-control form-control-lg" type="text" placeholder="Write a prompt and hit enter" aria-label=".form-control-lg example">
  </div>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div class="response">
          {{ response }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    data() {
      return {
        prompt: "",
        response: ""
      }
    },
    methods: {
      send: async function () {
        // `this` inside methods point to the Vue instance
        const _prompt = this.prompt
        this.prompt = ""

        try {
          const dataToSend = { "prompt": _prompt };
          console.log("Sending JSON...");
          console.log(dataToSend);

          const response = await fetch(process.env.BACKEND_DNS, {
            method: 'POST',
            mode: 'no-cors',
            headers: {
              'Content-Type': 'application/json',
              'accept': 'application/json'
            },
            body: JSON.stringify(dataToSend)
          });
          console.log("I'm back!");
          console.log(response);
          /*if (!response.ok) {
            console.log(response);
            throw new Error('Network response was not ok');
          }*/
          
          // const responseData = await response.json();
          // console.log(responseData.text);
        } catch (error) {
          console.error('Error sending POST request:', error);
        }

      }
    }
  }
</script>
<style>
.response{
  border: 1px solid rgb(201, 201, 201);
  padding: 10px;
  border-radius: 10px;
  margin-top: 30px;
  height: calc(100vh - 180px);
  overflow-y: scroll;
  overflow-x: hidden;
}
</style>
