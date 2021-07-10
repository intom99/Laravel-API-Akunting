<template>
  <div class="container my-4">
    <div class="row justify-content-center">
      <div class="col-8">
        <h1>Transaction</h1>
        <div class="card rounded shadow">
          <div class="card-header">
            <router-link
              :to="{ name: 'transaction.create' }"
              class="btn btn-sm btn-primary rounded my-2"
              >Add Transaction</router-link
            >
          </div>
          <div class="card-body">
            <table class="table">
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Amount</th>
                  <th>Type</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(trans, index) in transactions.data" :key="index">
                  <td>{{ trans.title }}</td>
                  <td>{{ trans.amount }}</td>
                  <td>{{ trans.type }}</td>
                  <td>
                    <div class="btn-group">
                      <router-link
                        :to="{
                          name: 'transaction.edit',
                          params: { id: trans.id },
                        }"
                        class="btn btn-sm btn-outline-primary"
                        >Edit</router-link
                      >
                      <button class="btn btn-sm btn-outline-danger" @click.prevent="destroy(trans.id, index)">
                        Delete
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import { onMounted, ref } from "vue";

export default {
  setup() {
    let transactions = ref([]);

    onMounted(() => {
      //
      axios
        .get("http://127.0.0.1:8000/api/transaction")
        .then((result) => {
          transactions.value = result.data;
        })
        .catch((err) => {
          console.log(err.response);
        });
    });

    function destroy(id, index){
        axios.delete(`http://127.0.0.1:8000/api/transaction/${id}`)
        .then(()=>{
            transactions.value.data.splice(index, 1)
        }).catch((err)=>{
            console.log(err.response.data)
        });
    }

    return {
      transactions,
      destroy
    };
  },
};
</script>

<style>
</style>