<template>
  <v-dialog v-model="dialog" max-width="800px">
    <v-form>
      <v-card max-width="800px">
        <v-card-title>Project</v-card-title> <v-divider></v-divider>
        <v-card-text>
          <v-row>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field outlined label="name" :value="this.dataTest.name">
              </v-text-field>
            </v-col>
            <v-col cols="6" class="ma-0 pa-0 pr-3 pl-3">
              <v-menu
                ref="menu"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    outlined
                    v-model="formatDateTo"
                    label="dateTo"
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="dateTo"
                  type="month"
                  no-title
                  scrollable
                >
                </v-date-picker>
              </v-menu>
            </v-col>
            <v-col cols="6" class="ma-0 pa-0 pr-3 pl-3">
              <v-menu
                ref="menu"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    outlined
                    v-model="formatDateFrom"
                    label="dateFrom"
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="dateFrom"
                  type="month"
                  no-title
                  scrollable
                >
                </v-date-picker>
              </v-menu>
            </v-col>

            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-autocomplete
                outlined
                label="position"
                :items="this.dataTest.items_position"
              >
              </v-autocomplete>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                outlined
                label="description"
                :value="this.dataTest.description"
              ></v-text-field>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                outlined
                label="responsibilities"
                :value="this.dataTest.responsibilities"
              ></v-text-field>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-combobox
                outlined
                type="number"
                label="teamSize"
                :items="this.dataTest.teamSize"
              ></v-combobox>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-combobox
                outlined
                multiple
                persistent-hint
                label="technologies"
                :items="this.dataTest.technologies"
              >
              </v-combobox>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions class="pt-0 pb-6 px-4">
          <v-spacer></v-spacer>
          <v-btn
            depressed
            outlined
            width="120"
            class="font-weight-regular"
            :style="{ 'border-color': '#e5e5e5' }"
            @click="dialog = false"
          >
            cancel
          </v-btn>
          <v-btn depressed width="120" color="red" class="white--text">
            delete
          </v-btn>
          <v-btn depressed width="120" color="primary" @click="dialog = false">
            save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-form>
  </v-dialog>
</template>
<script>
import moment from "moment";
import * as configBase from "./../config/config";
import axios from "axios";

export default {
  mounted() {
    //console.log("date", this.dateTo);
    //console.log(this.dataTest);
  },
  name: "DialogProject",
  data: () => ({
    dataTest: {},
    dialog: true,
    dateTo: new Date().toISOString().substr(0, 7),
    dateFrom: new Date().toISOString().substr(0, 7),
    editedIndex: -1,
  }),

  computed: {
    formatDateFrom() {
      return this.dataTest.dateFrom
        ? this.dataTest.dateFrom
        : this.dateFrom
        ? moment(this.dateFrom).format("MMM YYYY")
        : "";
    },
    formatDateTo() {
      return this.dataTest.dateTo
        ? this.dataTest.dateTo
        : this.dateTo
        ? moment(this.dateTo).format("MMM YYYY")
        : "";
    },
  },
  created: async function () {
    await axios
      .get(`${configBase.VUE_APP_URL}/dataProject/1`)
      .then((res) => {
        //console.log(JSON.stringify(res.data));
        this.dataTest = JSON.parse(JSON.stringify(res.data));
      })
      .catch((e) => {
        console.log(e);
      });
    // console.log(this.dataTest.name);
    //configBase.VUE_APP_URL -> process.env.VUE_APP_URL
  },
  methods: {},
};
</script>