<template>
  <div class="hello">
    <p>
      For guides and best practices on using the Tozny Platform for end to end encryption,<br>
      check out the
      <a href="https://developers.tozny.com" target="_blank" rel="noopener">Tozny documentation</a>.
    </p>
    <h3>Encrypt</h3>
    <div>
      <input type="text" placeholder="Record Type" v-model="recordType" />
      <br/>
      <br/>
      <textarea placeholder="Data to encrypt" v-model="dataToEncrypt" />
      <br/>
      <br/>
      <button class="btn button" @click="btnEncrypt">Encrypt Data</button>
      <div v-if="resultMessage">{{resultMessage}}</div>
    </div>
    <h3>Decrypt</h3>
    <div>
      <input type="text" placeholder="Record Id" v-model="recordId" />
      <br/>
      <br/>
      <button class="btn button" @click="btnDecrypt">Decrypt Data</button>
      <div v-if="decryptResult">{{decryptResult}}</div>
    </div>

  </div>
</template>

<script>
import { Config, Client } from 'tozny-browser-sodium-sdk'
import credentials from '../../credentials.json'
export default {
  name: 'TozStore',
  props: {
    msg: String
  },
  data: function () {
    return {
      recordType: "",
      dataToEncrypt: "",
      resultMessage: "",
      decryptResult: "",
      tozny: {
        client: false
      }
    }
  },
  created(){
    const config = Config.fromObject(
      credentials
    )
    this.tozny.client = new Client(config)
    
  },
  methods:{
    async btnEncrypt(){
      this.resultMessage = "";
      if(!this.recordType || !this.dataToEncrypt){
        this.resultMessage = "Both record type and the data field must be populated."
        return;
      }
      try{
        const written = await this.tozny.client.write(this.recordType, {"data": this.dataToEncrypt});
        this.resultMessage = "Successfully wrote record.  Record Id is " + written.meta.recordId;
      }catch(err){
        this.resultMessage = "Unable to write.  Check your configuration.";
        return;
      }
    },
    async btnDecrypt(){
      this.decryptResult = "";
      if(!this.recordId ){
        this.decryptResult = "Record Id must be populated."
        return;
      }
      try{
        const record = await this.tozny.client.read(this.recordId);
        this.decryptResult = JSON.stringify(record)
      }catch(err){
        this.decryptResult = "Unable to read.  Check your configuration or record Id.";
        return;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
