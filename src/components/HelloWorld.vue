<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <div>
          <h1 class="display-2 font-weight-bold mb-3" id="title">Reports</h1>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-card>
      <yearchart
        :chartdata="chartData"
        :options="{ responsive: true, maintainAspectRatio: false, title: { display: true, text: 'Yearly DSH Amount'} }"
      ></yearchart>
      </v-card>
    </v-row>
    <v-row>
      <v-col>
        <totalpie
        :chartdata="piechart1"
        :options="{ responsive: true,  title: { display: true, text: 'Dallas Children\s'} }"></totalpie>
      </v-col>
      <v-col>
        <totalpie
        :chartdata="piechart2"
        :options="{ responsive: true,  title: { display: true, text: 'Southwest General Hospital'} }"></totalpie>
      </v-col>
    </v-row>
    <v-row>
      <v-card style="width: 100%">
        <v-card-title>
          <v-row>System Costs</v-row>
          <v-row>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
          </v-row>
        </v-card-title>
      <v-data-table
        :headers="headers"
        :items="records"
        :items-per-page="10"
        :search="search"
        class="elevation-1"
        
      >
        <template v-slot:item.DSHAmt="{ item }">
          {{ item.DSHAmt | currency }}
        </template>
        <template v-slot:item.DSRIPAmt="{ item }">
          {{ item.DSRIPAmt | currency }}
        </template>
        <template v-slot:item.UCAmt="{ item }">
          {{ item.UCAmt | currency }}
        </template>
      </v-data-table>
      </v-card>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import currencyFormatter from "currency-formatter";
import yearchart from "./YearlyChart.vue";
import totalpie from "./TotalPie.vue";

export default Vue.extend({
  name: "HelloWorld",
  components: {
    yearchart,
    totalpie
  },
  filters: {
    currency: function (item: string) {
      return currencyFormatter.format(parseInt(item), { code: "USD" });
    },
  },
  computed: {
    chartData: function () {
      let labels = Array.from(new Set(this.records.map((item) => parseInt(item.Year))));
      let datasets: any[] = [];
      let colors = ["rgb(138 191 234)", "#396D9A", "#f3f3f3"];
      let counter = 0;
      for (let hospital of Array.from(
        new Set(this.records.map((item) => item.Hosp_Name))
      )) {
        datasets.push({
          label: hospital,
          backgroundColor: colors[counter],
          data: this.records
            .filter((x) => x.Hosp_Name == hospital)
            .map((x) => parseInt(x.DSHAmt)),
        });
        counter++;
      }

      return { labels, datasets };
    },
    piechart1: function(){
      let labels = ["DSH", "DSRIP", "UC"];
      let datasets: any[] = [];
      let colors = ["rgb(138 191 234)", "#396D9A", "#f3f3f3"];
      let data = this as {records: {Year: string, Hosp_Name: string, DSHAmt: string, DSRIPAmt: string, UCAmt: string}[]};
      for (let year of Array.from(
        new Set(this.records.map((item) => item.Year))
      ).splice(0,3)) {
        datasets.push({
          label: year,
          backgroundColor: colors,
          data: [
            data.records.find((x) => x.Year == year && x.Hosp_Name === "Childrens Medical Center Of Dallas")!.DSHAmt,
            data.records.find((x) => x.Year == year && x.Hosp_Name === "Childrens Medical Center Of Dallas")!.DSRIPAmt,
            data.records.find((x) => x.Year == year && x.Hosp_Name === "Childrens Medical Center Of Dallas")!.UCAmt,
          ],
        });
      }

      return {labels, datasets};
    },
    piechart2: function(){
      let labels = ["DSH", "DSRIP", "UC"];
      let datasets: any[] = [];
      let colors = ["rgb(138 191 234)", "#396D9A", "#f3f3f3"];
      for (let year of Array.from(
        new Set(this.records.map((item) => item.Year))
      ).splice(0,3)) {
        datasets.push({
          label: year,
          backgroundColor: colors,
          data: [
            this!.records.find((x) => x.Year == year && x.Hosp_Name === "Southwest General Hospital ")!.DSHAmt,
            this!.records.find((x) => x.Year == year && x.Hosp_Name === "Southwest General Hospital ")!.DSRIPAmt,
            this!.records.find((x) => x.Year == year && x.Hosp_Name === "Southwest General Hospital ")!.UCAmt,
          ],
        });
      }

      return {labels, datasets};
    }
  },
  data: () => ({
    search: '',
    headers: [
      {
        text: "System",
        value: "System",
        filterable: true
      },
      {
        text: "Hospital Name",
        value: "Hosp_Name",
      },
      {
        text: "County",
        value: "County",
      },
      {
        text: "SDA",
        value: "SDA",
      },
      {
        text: "Year",
        value: "Year",
      },
      {
        text: "DSH Amount",
        value: "DSHAmt",
      },
      {
        text: "DSRIP Amount",
        value: "DSRIPAmt",
      },
      {
        text: "UC Amount",
        value: "UCAmt",
      },
    ],
    records: [
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2012",
        DSHAmt: "38088115",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2013",
        DSHAmt: "18075267",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2014",
        DSHAmt: "19881840",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2015",
        DSHAmt: "22569280",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2016",
        DSHAmt: "20296256",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2017",
        DSHAmt: "21660699",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2018",
        DSHAmt: "18354333",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2019",
        DSHAmt: "15885327",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Children's Dallas",
        Hosp_Name:
          "Childrens Medical Center Of Dallas",
        County: "Dallas",
        SDA: "Dallas",
        Program: "Private",
        State_ID: "138910807",
        Year: "2020",
        DSHAmt: "4053416",
        DSRIPAmt: "291620505",
        UCAmt: "289725378",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2012",
        DSHAmt: "7132526",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2013",
        DSHAmt: "5998857",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2014",
        DSHAmt: "7085545",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2015",
        DSHAmt: "6687191",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2016",
        DSHAmt: "5305579",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2017",
        DSHAmt: "5544389",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2018",
        DSHAmt: "5152540",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2019",
        DSHAmt: "4657241",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Steward Health Care",
        Hosp_Name: "Southwest General Hospital ",
        County: "Bexar",
        SDA: "Bexar",
        Program: "Private",
        State_ID: "136491104",
        Year: "2020",
        DSHAmt: "5972840",
        DSRIPAmt: "8113977",
        UCAmt: "48950053",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2012",
        DSHAmt: "0",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2013",
        DSHAmt: "1706078",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2014",
        DSHAmt: "1270849",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2015",
        DSHAmt: "1358446",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2016",
        DSHAmt: "1210621",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2017",
        DSHAmt: "1028078",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2018",
        DSHAmt: "1011531",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2019",
        DSHAmt: "1149381",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
      {
        System: "Free-Standing",
        Hosp_Name:
          "Medina County Hospital ",
        County: "Medina",
        SDA: "Bexar",
        Program: "Public Non-State",
        State_ID: "212140201",
        Year: "2020",
        DSHAmt: "751790",
        DSRIPAmt: "8629275",
        UCAmt: "16425873",
      },
    ],
  }),
});
</script>
