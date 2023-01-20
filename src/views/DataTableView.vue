<script lang="ts">
export type Customer = {
  id: number;
  name: string;
  country: {
    name: string;
    code: string;
  };
  company: string;
  date: string;
  status: string;
  verified: boolean;
  activity: number;
  representative: {
    name: string;
    image: string;
  };
  balance: number;
};

export default {
  data() {
    return {
      customers: null as Customer[] | null,
      totalCustomers: null as number | null,
      supportedSizes: [3, 10, 50],
      requestParam: {
        page: 1,
        size: 3,
      },
    };
  },
  computed: {
    nbCustomers() {
      return this.customers?.length ?? 0;
    },
  },
  watch: {
    requestParam: {
      async handler() {
        await this.getCustomers();
      },
      deep: true,
      immediate: true,
    },
  },
  methods: {
    async getCustomers() {
      // simule call back avec pagination
      console.log("REQUEST: ", this.requestParam);
      const customers = [
        {
          id: 1000,
          name: "James Butt",
          country: {
            name: "Algeria",
            code: "dz",
          },
          company: "Benton, John B Jr",
          date: "2015-09-13",
          status: "unqualified",
          verified: true,
          activity: 17,
          representative: {
            name: "Ioni Bowcher",
            image: "ionibowcher.png",
          },
          balance: 70663,
        },
        {
          id: 1001,
          name: "Josephine Darakjy",
          country: {
            name: "Egypt",
            code: "eg",
          },
          company: "Chanay, Jeffrey A Esq",
          date: "2019-02-09",
          status: "proposal",
          verified: true,
          activity: 0,
          representative: {
            name: "Amy Elsner",
            image: "amyelsner.png",
          },
          balance: 82429,
        },
        {
          id: 1002,
          name: "Art Venere",
          country: {
            name: "Panama",
            code: "pa",
          },
          company: "Chemel, James L Cpa",
          date: "2017-05-13",
          status: "qualified",
          verified: false,
          activity: 63,
          representative: {
            name: "Asiya Javayant",
            image: "asiyajavayant.png",
          },
          balance: 28334,
        },
        {
          id: 1003,
          name: "Lenna Paprocki",
          country: {
            name: "Slovenia",
            code: "si",
          },
          company: "Feltz Printing Service",
          date: "2020-09-15",
          status: "new",
          verified: false,
          activity: 37,
          representative: {
            name: "Xuxue Feng",
            image: "xuxuefeng.png",
          },
          balance: 88521,
        },
        {
          id: 1004,
          name: "Donette Foller",
          country: {
            name: "South Africa",
            code: "za",
          },
          company: "Printing Dimensions",
          date: "2016-05-20",
          status: "proposal",
          verified: true,
          activity: 33,
          representative: {
            name: "Asiya Javayant",
            image: "asiyajavayant.png",
          },
          balance: 93905,
        },
        {
          id: 1005,
          name: "Simona Morasca",
          country: {
            name: "Egypt",
            code: "eg",
          },
          company: "Chapman, Ross E Esq",
          date: "2018-02-16",
          status: "qualified",
          verified: false,
          activity: 68,
          representative: {
            name: "Ivan Magalhaes",
            image: "ivanmagalhaes.png",
          },
          balance: 50041,
        },
        {
          id: 1006,
          name: "Mitsue Tollner",
          country: {
            name: "Paraguay",
            code: "py",
          },
          company: "Morlong Associates",
          date: "2018-02-19",
          status: "renewal",
          verified: true,
          activity: 54,
          representative: {
            name: "Ivan Magalhaes",
            image: "ivanmagalhaes.png",
          },
          balance: 58706,
        },
        {
          id: 1007,
          name: "Leota Dilliard",
          country: {
            name: "Serbia",
            code: "rs",
          },
          company: "Commercial Press",
          date: "2019-08-13",
          status: "renewal",
          verified: true,
          activity: 69,
          representative: {
            name: "Onyama Limba",
            image: "onyamalimba.png",
          },
          balance: 26640,
        },
        {
          id: 1008,
          name: "Sage Wieser",
          country: {
            name: "Egypt",
            code: "eg",
          },
          company: "Truhlar And Truhlar Attys",
          date: "2018-11-21",
          status: "unqualified",
          verified: true,
          activity: 76,
          representative: {
            name: "Ivan Magalhaes",
            image: "ivanmagalhaes.png",
          },
          balance: 65369,
        },
        {
          id: 1009,
          name: "Kris Marrier",
          country: {
            name: "Mexico",
            code: "mx",
          },
          company: "King, Christopher A Esq",
          date: "2015-07-07",
          status: "proposal",
          verified: false,
          activity: 3,
          representative: {
            name: "Onyama Limba",
            image: "onyamalimba.png",
          },
          balance: 63451,
        },
      ];
      this.totalCustomers = customers.length;
      this.customers = customers.filter(
        (_c, index) =>
          index >= this.requestParam.size * (this.requestParam.page - 1) &&
          index < this.requestParam.size * this.requestParam.page
      );
    },
  },
};
</script>

<template>
  <div>
    <h1>DataTable</h1>
    <DataTable
      dataKey="id"
      :value="customers"
      :lazy="true"
      :paginator="true"
      v-model:rows="requestParam.size"
      :totalRecords="totalCustomers"
      :rowsPerPageOptions="supportedSizes"
      @page="requestParam.page = $event.page + 1"
      :scrollable="true"
      scrollDirection="horizontal"
      style="width: 600px"
    >
      <template #header>
        <div class="datatable-header-title">Customers</div>
      </template>
      <template #paginatorend>
        <span>
          Showing ... to ... of {{ nbCustomers }}
          {{ nbCustomers > 1 ? "customers" : "customer" }}
        </span>
      </template>

      <Column field="id" header="ID" :sortable="true"></Column>
      <Column
        field="name"
        header="Name"
        :sortable="true"
        style="width: 200px"
      ></Column>
      <Column
        field="country.name"
        header="Country"
        :sortable="true"
        style="width: 200px"
      ></Column>
      <Column
        field="representative.name"
        header="Agent"
        :sortable="true"
        style="width: 200px"
      ></Column>
      <Column field="balance" header="Balance" :sortable="true"></Column>
      <Column field="status" header="Status" :sortable="true"></Column>
      <Column field="activity" header="Activity" :sortable="true"></Column>
    </DataTable>
  </div>
</template>

<style>
.datatable-header-title {
  font-weight: bold;
}
</style>
