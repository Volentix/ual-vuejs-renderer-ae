<template>
  <q-page class="flex flex-center">
    <img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
    <ual-trigger :options="opts" @login="userCallback" />
  </q-page>
</template>

<script>
import { MockAuthenticator } from '../authMock'
import { Scatter } from 'ual-scatter'
export default {
  name: 'PageIndex',
  data () {
    return {
      user: {
        name: '',
        chainId: ''
      },
      opts: {
        name: 'VUE UAL test',
        nets: [{
          chainId: 12345,
          rpcEndpoints: [{
            protocol: 'https',
            host: 'example.net',
            port: Number(443)
          }]
        }],
        authenticators: [
          { authenticator: Scatter, netChainIds: [12345], options: { appName: 'UAL Example' } },
          MockAuthenticator
        ]
      }
    }
  },
  methods: {
    async userCallback (users) {
      const loggedInUser = users[0]
      this.user.name = await loggedInUser.getAccountName()
      this.user.chainId = await loggedInUser.getChainId()
      console.info('User Information:')
      console.info('Account Name:', this.user.name)
      console.info('Chain Id:', this.user.chainId)

      // balanceUpdateInterval = setInterval(updateBalance, 1000)
    }
  }
}
</script>
