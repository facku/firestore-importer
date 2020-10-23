<template>
  <q-page padding>
    <h1 class="text-h4">MySQL module Test</h1>

    <div class="row q-mb-md">
      <div class="col">
        <q-input outlined v-model="sqlCredentials.host" label="MySQL Host" class="q-pb-sm"/>
      </div>
    </div>
    <div class="row">
      <div class="col-6">
        <q-input outlined v-model="sqlCredentials.user" label="MySQL User" />
      </div>
      <div class="col-6">
        <q-input type="password" outlined v-model="sqlCredentials.password" label="Password" class="q-pl-md"/>
      </div>
    </div>

    <div class="row">
      <q-btn outline icon="search" class="q-mt-lg" color="primary" @click="getDatabases">Buscar BB.DD</q-btn>
    </div>


    <q-separator class="q-my-lg" v-show="databases.length"/>

    <div class="row" v-show="databases.length">
      <div class="col-3">
        <p class="text-h5">

          Databases
        </p>
        <q-list bordered separator>
          <q-item clickable v-ripple v-for="(db,index) in databases" :key="index" @click="onItemClick(index, db)">
            <q-item-section> {{db}} </q-item-section>
          </q-item>
          </q-item>
        </q-list>
      </div>
      <div class="col-3 q-mx-sm">
        <p class="text-h5">Tables</p>
        <q-list bordered separator>
          <q-item clickable v-ripple v-for="(db,index) in databases" :key="index" @click="onItemClick(index, db)">
            <q-item-section> {{db}} </q-item-section>
          </q-item>
          </q-item>
        </q-list>
      </div>
      <div class="col">
        <p class="text-h5">Exports</p>
        <q-list bordered separator>
          <q-item clickable v-ripple v-for="(db,index) in databases" :key="index" @click="onItemClick(index, db)">
            <q-item-section> {{db}} </q-item-section>
          </q-item>
          </q-item>
        </q-list>
      </div>
    </div>

  </q-page>
</template>

<script>
import * as MySQL from "mysql2";

export default {
  name: "PageBridge",

  data: () => {
    return {
      sqlCredentials:{
        host: "localhost",
        user: "root",
        password: ""
      },
      databases: ['asas','sasas','0asasas','asas','sasas','0asasas','asas','sasas','0asasas']
    };
  },

  methods: {
    getDatabases() {
      console.log('getDatabases start')

      this.$q.loading.show();

      this.databases = [];
      try {
        const connection = MySQL.createConnection(this.sqlCredentials);

        connection.query("show databases", async (err, results, fields) => {
          if(!results){
            alert("NO SE PUDO LEER DESDE MYSQL")
            console.log("NO SE PUDO LEER DESDE MYSQL")
            console.log(results)
            connection.destroy()
            this.$q.loading.hide()
          }

          const databases = await results.map(db => {
            console.log('getDatabases Map ->', db.Database)
            return db.Database
          });
          connection.destroy();
          this.databases = databases;
          this.$q.loading.hide();
          console.log('getDatabases end')
        });
      } catch (error) {
        connection.destroy()
        this.$q.loading.hide();
        console.log("EROR ERROR ERRROR ROEOERORO")
        console.log(error)
      }
    },

    onItemClick(db,index){
      // alert(`${db} ${index}`)
    }
  }

};
</script>
