# Svelte + PouchDB + TS + Vite

This was a very small test project created for experimenting with PouchDB in svelte.

The purpose behind it was to understand svelte as well as PouchDB and it's usage.

You can view a demo app (without the remote couchdb connection) here: https://deshmukhmayur.github.io/svelte-pouchdb-counter

### Technologies Used

- [Svelte](https://svelte.dev/) - Component framework for building simple web apps
- [PouchDB](https://pouchdb.com/) - Local database (in-browser)
- [CouchDB](https://couchdb.apache.org/) - Remote database
- [Vite](https://vitejs.dev/) - Build Tool

## Setup

### Prerequisites

- NodeJS - https://nodejs.org/en/download/
- Docker - https://www.docker.com/products/docker-desktop

### 1. Start the couchdb container using docker-compose 
You can skip this step if you already have a different couchdb server.

```bash
# Start the docker-compose service
docker compose up -d couchdb
```

### 2. Start the Svelte App

```bash
# Install dependencies
npm install

# Start dev server
npm run dev
```

If you are using your own couchdb server, make sure to change the credentials in [`env.ts`](src/env.ts).

## Contributing

I'm not accepting any active contributions to this project since this is just a project created while following a tutorial.

I will open it for contributions once I've made some additional modifications.

## License
This project is licensed under [MIT License](./LICENSE).