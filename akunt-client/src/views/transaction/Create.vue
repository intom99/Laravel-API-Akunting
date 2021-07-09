<template>
  <div class="container my-4">
    <div class="row justify-content-center">
      <div class="col-8">
        <div class="card rounded shadow">
          <div class="card-header">
            
        <h3>Create Transaction</h3>
          </div>
          <div class="card-body">
            <form @submit.prevent="store()">
              <div class="mb-3">
                <label class="form-label">Title</label>
                <input type="text" class="form-control" v-model="transaction.title"  placeholder="title"/>
                <div v-if="validation.title" class="text-danger">
                    {{validation.title[0]}}
                </div>
              </div>
              
              <div class="mb-3">
                <label class="form-label">Amount</label>
                <input type="text" class="form-control" v-model="transaction.amount" placeholder="amount" />
                <div v-if="validation.amount" class="text-danger">
                    {{validation.amount[0]}}
                </div>
              </div>
              
              <div class="mb-3">
                <label class="form-label">Time</label>
                <input type="text" class="form-control" v-model="transaction.time" placeholder="yyyy-mm-dd hh:mm:ss" />
                <div v-if="validation.time" class="text-danger">
                    {{validation.time[0]}}
                </div>
              </div>
              
              <div class="mb-3">
                <label class="form-label">Type</label>
                <select class="form-select" v-model="transaction.type" id="">
                    <option value="expense">Expense</option>
                    <option value="revenue">Revenue</option>
                </select>
                <div v-if="validation.type" class="text-danger">
                    {{validation.type[0]}}
                </div>
              </div>
                <hr>
                <button class="btn btn-md btn-primary" type="submit">Save</button>
                <router-link
              :to="{ name: 'transaction.index' }"
              class="btn btn-md btn-secondary rounded mx-2"
              >Back</router-link
            >
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import {reactive, ref} from 'vue'
import {useRouter} from 'vue-router'
import axios from 'axios'

export default {
    setup(){
        //data binding
        const transaction = reactive({
            'title': '',
            'amount': '',
            'time': '',
            'type': '',
            
        });

        const validation = ref([]);

        const router = useRouter();

        function store(){
            axios.post('http://127.0.0.1:8000/api/transaction',
            transaction)
            .then(()=>{
                router.push({
                    name: 'transaction.index'
                })
            }).catch((err)=>{
                validation.value = err.response.data
            });
        }

        return {
            transaction,
            validation,
            router,
            store
        }
    }
};
</script>

<style>
</style>