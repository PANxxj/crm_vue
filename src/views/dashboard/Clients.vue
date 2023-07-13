<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Clients</h1>
                <router-link to="/dashboard/clients/add"
                v-if="$store.state.team.max_leads>num_clients"
                >Add Clients</router-link>

                <div class="notification is-danger" v-else>
                    you have reached your limitions ! please upgrade
                </div>
                
                <hr>
                <form @submit.prevent="getClients">
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
                <template v-if="clients.length">
                    <table class="table is-fullwidth">
                        <thead>
                            <tr>
                                <th>Name</th>
                                <th>Conact Person</th>
                                <th></th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="client in clients" :key="client.id">
                                <td>{{ client.name }}</td>
                                <td>{{ client.contact_person }}</td>
                                <td>
                                    <router-link :to="{name:'Client',params:{id:client.id}}">Details</router-link>
                                </td>
                            </tr>

                        </tbody>
                    </table>
                    <div class="buttons">
                    <button class="button is-light" @click="goPrevPage()" v-if="showPrevButton">Previous</button>
                    <button class="button is-light" @click="goNextPage()" v-if="showNextButton">Next</button>
                </div>
                </template>
                <template v-else>
                    <p>You don't have any clients yet !     </p>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'Clients',
    data(){
        return {
            clients:[],
            showNextButton:false,
            showPrevButton:false,
            currentPage:1,
            query:'',
            num_clients:0
        }
    },
    mounted(){
        this.getClients()
    },
    methods:{
        goNextPage(){
            this.currentPage +=1
            this.getClients()
        },
        goPrevPage(){
            this.currentPage -=1
            this.getClients()
        },
        async getClients(){
            this.$store.commit('setIsLoading',true)
            this.showNextButton=false
            this.showPrevButton=false
            await axios
                .get(`api/v1/clients/`)
                .then(response =>{
                    this.num_clients=response.data.count
                })
            await axios
                .get(`api/v1/clients/?page=${this.currentPage}&search=${this.query}`)
                .then(response =>{
                    this.clients=response.data.results

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
        }
    }
}
</script>