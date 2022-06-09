<template>
  <v-container>
    <v-row class="text-center">

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Devoted test task
        </h1>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
          <v-btn
            elevation="5"
            color="primary"
            :loading="isLoading"
            v-if="!data"
            @click="getData"
            >Get data</v-btn>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >

        <h2 class="headline font-weight-bold mb-3">
          Table with data:  {{ data !== null ? data.first : 'empty' }}
        </h2>

      </v-col>

    </v-row>

    <div class="table__wrapper">
      <v-simple-table v-if="data">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">
                Title
              </th>
              <th class="text-left">
                Authors
              </th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="item in data"
              :key="item.id"
            >
              <td>{{ item.title }}</td>
              <td v-if="!Array.isArray(item.authors)">{{ item.authors }}</td>
              <td v-else>
                <span v-for="(author, i) of item.authors" 
                  :key="i">{{ author }} 
                  <span v-if="i !== item.authors.length - 1">, </span>
                </span>
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </div>

    <div>
      <v-row justify="space-around">
        <v-col cols="auto">
          <v-dialog
            transition="dialog-bottom-transition"
            max-width="600"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                v-show="data"
                v-bind="attrs"
                v-on="on"
              >Add new item</v-btn>
            </template>
            <template v-slot:default="dialog">
              <v-card>
                <v-toolbar
                  color="primary"
                  dark
                >Adding new item</v-toolbar>
                <v-card-text>
                  <v-form v-model="valid">
                    <v-container>
                      <v-col>
                        <v-col
                          cols="12"
                        >
                          <v-text-field
                            v-model="title"
                            :rules="titleRules"
                            label="Enter title"
                            required
                          ></v-text-field>
                        </v-col>

                        <v-col
                          cols="12"
                        >
                          <v-text-field
                            v-model="authors"
                            :rules="authorsRules"
                            label="Enter authors"
                            required
                          ></v-text-field>
                        </v-col>

                        <v-btn
                          :disabled="!valid"
                          color="success"
                          class="mr-4"
                          @click="addNewItem(); dialog.value = false"
                          justify="center"
                        >
                          Submit
                        </v-btn>

                      </v-col>
                    </v-container>
                  </v-form>
                </v-card-text>
                <v-card-actions class="justify-end">
                  <v-btn
                    text
                    @click="dialog.value = false"
                  >Close</v-btn>
                </v-card-actions>
              </v-card>
            </template>
          </v-dialog>
        </v-col>
      </v-row>
    </div>

  </v-container>
</template>

<script>
  export default {
    name: 'MyTable',

    data: () => ({
      data: null,
      isLoading: false,
      valid: false,
      title: '',
      authors: '',
      titleRules: [
        v => !!v || 'title is required',
      ],
      authorsRules: [
        v => !!v || 'author is required',
      ],
    }),

    methods: {
      getData() {
        if( this.data != null ) return false;

        this.isLoading = true;
        fetch('http://localhost:3000/posts')
          .then((response) => response.json())
          .then((json) => {
              this.isLoading = false;
              this.data = json;
          });
      },

      async addNewItem() {
        let authorsData = this.authors.indexOf(',') != -1 ? this.authors.split(',') : this.authors;

        let data = {
          title: this.title,
          authors: authorsData
        };

        let response = await fetch('http://localhost:3000/posts', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        });

        let result = await response.json();
        this.data.push(result);
      }
    },
  }
</script>

<style>
  .table__wrapper {
    width: 100%;
  }
</style>