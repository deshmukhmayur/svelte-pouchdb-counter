<script lang="ts">
  import { onMount } from 'svelte'
  import PouchDB from 'pouchdb-browser'
  import { COUCHDB_DB_PATH, COUCHDB_AUTH_HEADER } from 'src/env';

  /* Setup local pouchdb */
  const pouch = new PouchDB('testdb')
  /* Setup remote couchdb connection with custom authentication */
  const remoteDb = new PouchDB(COUCHDB_DB_PATH, {
    fetch: async (uri, opts) => {
      const headers = new Headers(opts.headers)
      headers.append('Authorization', COUCHDB_AUTH_HEADER)
      return fetch(uri, {
        ...opts,
        headers
      })
    }
  })
  /* Default / initial count */
  let countDoc: any = { _id: 'counter', count: 0 }

  onMount(async () => {
    /* Fetch the latest update from the db and update the local state */
    countDoc = await pouch.get('counter').catch(err => countDoc)
    /* Listen for any changes in the database and update the local state */
    pouch.changes({
      since: 'now',
      live: true
    }).on('change', async val => {
      countDoc = await pouch.get('counter')
    })

    /* Replicate the data to and from the remote couchdb server */
    pouch.replicate.to(remoteDb, { live: true, retry: true }, console.error)
    pouch.replicate.from(remoteDb, { live: true, retry: true }, console.error)
  })

  /* Increment the count and update the db */
  const increment = async () => {
    countDoc.count += 1
    await pouch.put(countDoc)
  }
</script>

<button on:click={increment}>
  Clicks: {countDoc.count}
</button>

<style>
  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: rgba(255, 62, 0, 0.2);
  }
</style>
