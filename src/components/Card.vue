<template>
  <div class="mt-10">
    <v-container>
      <v-row>
        <v-col cols="12" md="6 offset-md-3">
          <v-text-field
            v-model="editedItem.name"
            @click:append="save"
            @keyup.enter="save"
            class="pt-6 pb-4 px-4 mb-4"
            label="Add your shopping item here"
            append-icon="mdi-plus"
            hide-details
            clearable
          ></v-text-field>
          <v-data-table
            :headers="headers"
            :items="shoppingItems"
            hide-default-header
            hide-default-footer
            :single-select="singleSelect"
            show-select
            item-key="name"
            class="elevation-2"
          >
            <template>
              <v-switch
                v-model="singleSelect"
                label="Single select"
                class="pa-3"
              ></v-switch>
            </template>
            <template v-slot:top>
              <v-dialog v-model="dialog" persistent max-width="500px">
                <v-card>
                  <v-card-title>
                    <span class="text-h5">Edit item</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-text-field
                            v-model="editedItem.name"
                            label="Edit item"
                            hide-details
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" @click="close"> Cancel </v-btn>
                    <v-btn color="primary" @click="save"> Save </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-dialog v-model="dialogDelete" persistent max-width="400px">
                <v-card class="pa-6">
                  <v-card-title class="text-center text-h6 mb-6"
                    >Are you sure you want to delete this item?</v-card-title
                  >
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" @click="closeDelete">Cancel</v-btn>
                    <v-btn color="primary" @click="deleteItemConfirm">OK</v-btn>
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </template>
            <template v-slot:item.actions="{ item }">
              <v-icon small class="mr-2" @click="editItem(item)">
                mdi-pencil
              </v-icon>
              <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
            </template>
            <template v-slot:no-data>
              <div class="d-flex align-center justify-center pa-12">
                <v-icon x-large color="green lighten-2">mdi-check </v-icon>
                <span class="text-h6">No List</span>
              </div>
            </template>
          </v-data-table>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    singleSelect: false,
    selected: [],
    headers: [
      {
        text: "Product Name",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Actions", value: "actions", sortable: false },
    ],
    shoppingItems: [],
    editedIndex: -1,
    editedItem: {
      name: "",
    },
    defaultItem: {
      name: "",
    },
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.shoppingItems = [];
    },

    editItem(item) {
      this.editedIndex = this.shoppingItems.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.shoppingItems.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.shoppingItems.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.shoppingItems[this.editedIndex], this.editedItem);
      } else {
        this.shoppingItems.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>

<style lang="scss">
.v-application .text-start {
  font-size: 20px !important;

  .v-icon.v-icon {
    font-size: 20px !important;
  }
}

.v-data-table__wrapper {
  & tr td{
    &:first-child {
      width: 40px;
    }
    &:last-child {
      text-align: end !important;
    }
  }
}
</style>
