<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">{{ client.name }}</h1>
                <router-link :to="{name:'EditClient',params:{id:client.id}}" class="button is-light">Edit</router-link>
            </div>

            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">Details</h2>
                    <p><strong>Created at : </strong>{{ client.created_at }}</p>
                    <p><strong>Modified at : </strong>{{ client.updated_at }}</p>
                </div>
            </div>
            
            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">contact information</h2>
                    <p><strong>Contact Person : </strong>{{ client.contact_person }}</p>
                    <p><strong>Email : </strong>{{ client.email }}</p>
                    <p><strong>Phone : </strong>{{ client.phone }}</p>
                    <p><strong>Website : </strong>{{ client.website }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'Client',
    data(){
        return {
            client:{}
        }
    },
    mounted(){
        this.getClient()
    },
    methods:{
        async getClient(){
            this.$store.commit('setIsLoading',true)

            const clientId=this.$route.params.id
            await axios
                .get(`api/v1/clients/${clientId}`)
                .then(response =>{
                    this.client=response.data
                })
                .catch(error =>{
                    console.log(error);
                })
            
            this.$store.commit('setIsLoading',false)
        }
    }
}
</script>