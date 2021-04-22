<template>
  <div class="container text-center">
    <div>
      <Logo />
      <h1 class="title">desarrollo-polkadot-kusama-nuxtjs</h1>
      <table class="table my-4">
        <thead>
          <tr>
            <th>chain</th>
            <th>Node name</th>
            <th>Node version</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>
              {{ chain }}
            </td>
            <td>
              {{ nodeName }}
            </td>
            <td>
              {{ nodeVersion }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { ApiPromise, WsProvider } from '@polkadot/api'
export default {
  data() {
    return {
      chain: null,
      nodeName: null,
      nodeVersion: null,
    }
  },
  async created() {
    // Initialise the provider to connect to the local node
    const provider = new WsProvider('wss://rpc.polkadot.io')

    // Create the API and wait until ready
    const api = await ApiPromise.create({ provider })

    // Retrieve the chain & node information information via rpc calls
    const [chain, nodeName, nodeVersion] = await Promise.all([
      api.rpc.system.chain(),
      api.rpc.system.name(),
      api.rpc.system.version(),
    ])
    this.chain = chain
    this.nodeName = nodeName
    this.nodeVersion = nodeVersion
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
