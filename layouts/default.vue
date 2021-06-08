<template>
  <v-app dark>
    <v-main>
      <h1>TOTP Generation</h1>
      <v-container>
        <!-- <nuxt /> -->
        <h3>Private Key Generation</h3>
        <v-btn @click="generate()">Generate</v-btn><br />
        <v-text-field
          v-model="privateKey"
          readonly
          hint="This output is used to generate the 8 digit code on users device"
          label="Private Key"
        ></v-text-field>
        <br />
        <a :href="uriData"
          ><v-img :src="imgData" max-height="150" max-width="150"></v-img></a
      ></v-container>
      <v-container>
        <h3>Verify</h3>
        <v-text-field
          v-model="inputPrivateKey"
          hint="This private key will be stored on the database"
          label="Private Key"
        ></v-text-field>
        <v-text-field
          v-model="inputCode"
          hint="This code is the 6 digit number provided by the users auth app"
          label="2FA Code"
        ></v-text-field>
        <v-btn @click="verify()">Verify</v-btn>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
const twofactor = require('node-2fa')
export default {
  data() {
    return {
      privateKey: '',
      uriData: '#',
      imgData: '',
      inputPrivateKey: '',
      inputCode: '',
    }
  },
  methods: {
    generate() {
      const newSecret = twofactor.generateSecret({
        name: 'CryptoCollab',
        account: 'accountName',
      })
      this.privateKey = newSecret.secret
      this.uriData = newSecret.uri
      this.imgData = newSecret.qr
    },
    verify() {
      const verification = twofactor.verifyToken(
        this.inputPrivateKey,
        this.inputCode
      )
      console.log(verification)
    },
  },
}
</script>
