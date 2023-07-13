<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">leads</h1>
                <router-link to="/dashboard/leads/add"
                    v-if="$store.state.team.max_leads>num_leads"
                >Add leads</router-link>
                <div class="notification is-danger" v-else>
                    you have reached your limitions ! please upgrade
                </div>
                <hr>

                <form @submit.prevent="getLeads">
                    <div class="field has-addons">
                        <div class="control">
                            <input type="text" class="input" v-model="query">
                        </div>
                        <div class="control">
                            <button class="button is-success">Search</button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="column is-12">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Company</th>
                            <th>Conact Person</th>
                            <th>Assigned To</th>
                            <th>Status</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="lead in leads" :key="lead.id">
                            <td>{{ lead.company }}</td>
                            <td>{{ lead.contact_person }}</td>
                            <td>
                                <template v-if="lead.assigned_to">{{ lead.assigned_to.first_name }} {{ lead.assigned_to.last_name }}</template>
                            </td>
                            <td>{{ lead.status  }}</td>
                            <td>
                                <router-link :to="{name:'Lead',params:{id:lead.id}}">Details</router-link>
                            </td>
                        </tr>

                    </tbody>
                </table>
                <div class="buttons">
                    <button class="button is-light" @click="goPrevPage()" v-if="showPrevButton">Previous</button>
                    <button class="button is-light" @click="goNextPage()" v-if="showNextButton">Next</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'Leads',
    data(){
        return {
            leads:[],
            showNextButton:false,
            showPrevButton:false,
            currentPage:1,
            query:'',
            num_leads:0
        }
    },
    mounted(){
        this.getLeads()
    },
    methods:{
        goNextPage(){
            this.currentPage +=1
            this.getLeads()
        },
        goPrevPage(){
            this.currentPage -=1
            this.getLeads()
        },
        async getLeads(){
            this.$store.commit('setIsLoading',true)
            this.showNextButton=false
            this.showPrevButton=false

            await axios
                .get(`api/v1/leads/`)
                .then(response =>{
                    this.num_leads=response.data.count
                    console.log('data',response.data);
                })

            await axios
                .get(`api/v1/leads/?page=${this.currentPage}&search=${this.query}`)
                .then(response =>{
                    this.leads=response.data.results
                    console.log('next',response.data.next);
                    
                    if(response.data.next){
                        this.showNextButton=true
                    }
                    if(response.data.previous){
                        this.showPrevButton=true
                    }
                })
                .catch(error =>{
                    console.log(error);
                })


            this.$store.commit('setIsLoading',false)
        },
    }
}
</script>