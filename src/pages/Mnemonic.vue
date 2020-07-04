<template>
  <q-page class="q-pa-md">
    <h5>Mnémoniques</h5>
    <p>Récupération de la clé privée à partir des 24 mots.</p>
    <q-banner inline-actions rounded class="bg-yellow">
      <q-icon name="warning" size="md" />
      Pour plus de sécurité, veuillez désactiver votre connection Internet. Cette page fonctionne sans connection, aucune information n'est envoyée ni collectée.
    </q-banner>
    <br>
    <q-select outlined v-model="blockchain" :options="options" label="Blockchain"  />
    <br>
    <q-input outlined v-model="text" label="24 mots" />
    <br>
    <q-btn
      color="primary"
      @click="wallet = getWallet(text, blockchain)"
    >Calculer</q-btn>
    <br><br>
    <p>Clé publique (Adresse): {{ wallet && '0x' + wallet.getAddress().toString('hex') }}</p>
    <p>Clé privée: {{ wallet && wallet.getPrivateKey().toString('hex') }}</p>
  </q-page>
</template>

<script>
import { hdkey } from "ethereumjs-wallet";
import * as bip39 from "bip39";

export default {
  name: 'Mnemonic',
  data: () => {
    const options = [
      {
        label: "Ethereum",
        value: "m/44'/60'/0'/0/0"
      },{
        label: "Wethio",
        value: "m/44'/77777'/0'/0/0"
      }];
    return {
      text: "",
      options,
      blockchain: options[0],
      wallet: null,
      getWallet: (text, blockchain) => {
         return hdkey.fromMasterSeed(bip39.mnemonicToSeedSync(text)).derivePath(blockchain.value).getWallet();
      }
    }
  }
}
</script>
