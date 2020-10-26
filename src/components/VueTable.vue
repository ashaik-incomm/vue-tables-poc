<template>
  <v-data-table
    :headers="headers"
    :items="users"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Manage Users</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
          <v-spacer></v-spacer>
            <v-spacer></v-spacer>
              <v-spacer></v-spacer>
                <v-spacer></v-spacer>
         <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
        <v-spacer></v-spacer>
           
        
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.fname"
                      label="First name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.lname"
                      label="Last Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.role"
                      label="Role (Permissions)"
                    ></v-text-field>
                    
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.reset"
                      label="Reset"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'First Name',
          align: 'start',
          sortable: false,
          value: 'fname',
        },
        { text: 'Last Name', value: 'lname' },
        { text: 'Role (Permissions)', value: 'role' },
        { text: 'Reset', value: 'reset', visibility: 'hidden' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      users: [],
      editedIndex: -1,
      editedItem: {
        fname: '',
        lname: 0,
        fat: 0,
        role: 0,
        reset: 0,
      },
      defaultItem: {
        name: '',
        lname: 0,
        fat: 0,
        role: 0,
        reset: 0,
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.users = [
          {
            fname: 'Frozen',
            lname:'Yogurt',
            calories: 159,
            fat: 6.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Ice cream',
            lname:'sandwich',
            calories: 237,
            fat: 9.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Ech',
            lname: 'clair',
            calories: 262,
            fat: 16.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Cup',
            lname: 'cake',
            calories: 305,
            fat: 3.7,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Ginger',
            lname: 'bread',
            calories: 356,
            fat: 16.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Jelly',
            lname: 'bean',
            calories: 375,
            fat: 0.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Lolli',
            lname: 'pop',
            calories: 392,
            fat: 0.2,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Honey',
            lname: 'comb',
            calories: 408,
            fat: 3.2,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Doh',
            lname: 'nut',
            calories: 452,
            fat: 25.0,
            role: ['A','B'],
            reset: '',
          },
          {
            fname: 'Kit',
            lname: 'kat',
            calories: 518,
            fat: 26.0,
            role: ['A','B'],
            reset: '',
          },
        ]
      },

      editItem (item) {
        this.editedIndex = this.users.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.users.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        this.users.splice(this.editedIndex, 1)
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.users[this.editedIndex], this.editedItem)
        } else {
          this.users.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>
