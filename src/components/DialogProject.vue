<template>
  <v-dialog v-model="dialog" max-width="800px">
    <template v-slot:activator="{ on, attrs }">
      <v-btn color="red lighten-2" dark v-bind="attrs" v-on="on">
        Click Me
      </v-btn>
    </template>
    <v-form>
      <v-card max-width="800px">
        <v-card-title>Project</v-card-title> <v-divider></v-divider>
        <v-card-text>
          <v-row>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                outlined
                class="text-capitalize"
                label="name"
                v-model="dataPJ.name"
              >
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
                    class="text-capitalize"
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
                  :max="new Date().toISOString()"
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
                    class="text-capitalize"
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
                  :max="dateTo"
                >
                </v-date-picker>
              </v-menu>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                class="text-capitalize"
                outlined
                label="position"
                v-model="dataPJ.itemsPosition"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                class="text-capitalize"
                outlined
                label="description"
                v-model="dataPJ.description"
              ></v-text-field>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-text-field
                class="text-capitalize"
                outlined
                label="responsibilities"
                v-model="dataPJ.responsibilities"
              ></v-text-field>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-combobox
                class="text-capitalize"
                outlined
                type="number"
                label="teamSize"
                :items="this.itemTeam"
                v-model="dataPJ.teamSize"
              ></v-combobox>
            </v-col>
            <v-col cols="12" class="ma-0 pa-0 pr-3 pl-3">
              <v-combobox
                class="text-capitalize"
                outlined
                multiple
                persistent-hint
                label="technologies"
                v-model="dataPJ.technologies"
                :items="this.itemTech"
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
          <v-btn depressed width="120" color="primary" @click="update">
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
  name: "DialogProject",
  updated() {
    console.log(this.dateTo);
  },
  data: () => ({
    dialog: false,
    itemTeam: [1, 2, 3, 4, 5, 6],
    itemTech: ["Java", "Vue", "React", "HTML", "CSS"],
    dataTest: {},
    dataPJ: {
      id: 0,
      name: "",
      dateTo: "",
      dateFrom: "",
      teamSize: 1,
      itemsPosition: "",
      description: "",
      responsibilities: "",
      technologies: [],
    },
    dateTo: new Date().toISOString().substr(0, 7),
    dateFrom: new Date().toISOString().substr(0, 7),
  }),
  computed: {
    formatDateFrom() {
      return this.dataPJ ? moment(this.dateFrom).format("MMM YYYY") : "";
    },
    formatDateTo() {
      return this.dataPJ.dateTo
        ? this.dataPJ.dateTo
        : this.dateTo
        ? moment(this.dateTo).format("MMM YYYY")
        : "";
    },
  },
  async mounted() {
    await this.get();
    this.dataPJ = this.dataTest;
  },
  methods: {
    async get() {
      await axios
        .get(`${configBase.VUE_APP_URL}/dataProject/1`)
        .then((res) => {
          this.dataTest = res.data;
        })
        .catch((e) => {
          console.log(e);
        }); // console.log(this.dataTest.name); //configBase.VUE_APP_URL -> process.env.VUE_APP_URL },
    },
    async update() {
      this.dialog = false;
      console.log("hihi: " + JSON.stringify(this.dataPJ));
      await axios
        .put(`${configBase.VUE_APP_URL}/dataProject/1`, this.dataPJ)
        .then((res) => {
          // console.log(JSON.stringify(res.data));
          this.dataPJ = res.data;
        })
        .catch((e) => {
          throw new Error(e);
        });
    },
  },
};
</script>