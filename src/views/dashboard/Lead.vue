<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">{{ lead.company }}</h1>

                <div class="buttons">
                    <router-link :to="{name:'EditLead',params:{id:lead.id}}" class="button is-light">Edit</router-link>
                    <button @click="convertToClient" class="button is-info">Convert To Client</button>
                    <button class="button is-danger" @click="deleteLead">Delete</button>
                    
                </div>
            </div>

            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">Details</h2>
                    <template v-if="lead.assigned_to">
                        <p><strong>Assigned To : </strong>{{ lead.assigned_to.first_name }} {{ lead.assigned_to.last_name }}</p>
                    </template>
                    <p><strong>Status : </strong>{{ lead.status }}</p>
                    <p><strong>Priority : </strong>{{ lead.priority }}</p>
                    <p><strong>Confidience : </strong>{{ lead.confidence }}</p>
                    <p><strong>Estimated Value : </strong>{{ lead.estimation_value }}</p>
                    <p><strong>Created at : </strong>{{ lead.created_at }}</p>
                    <p><strong>Modified at : </strong>{{ lead.updated_at }}</p>
                </div>
            </div>
            
            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">contact information</h2>
                    <p><strong>Contact Person : </strong>{{ lead.contact_person }}</p>
                    <p><strong>Email : </strong>{{ lead.email }}</p>
                    <p><strong>Phone : </strong>{{ lead.phone }}</p>
                    <p><strong>Website : </strong>{{ lead.website }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'Lead',
    data(){
        return {
            lead:{}
        }
    },
    mounted(){
        this.getLead()
    },
    methods:{
        async deleteLead(){
            this.$store.commit('setIsLoading',true)
            const leadId=this.$route.params.id
            await axios
                .post(`api/v1/leads/delete_lead/${leadId}`)
                .then(response =>{
                    console.log(response.data);
                    this.$router.push('/dashboard/leads')
                })
                .catch(error =>{
                    console.log(error);
                })
        },
        async getLead(){
            this.$store.commit('setIsLoading',true)

            const leadId=this.$route.params.id
            await axios
                .get(`api/v1/leads/${leadId}`)
                .then(response =>{
                    this.lead=response.data
                })
                .catch(error =>{
                    console.log(error);
                })
            
            this.$store.commit('setIsLoading',false)
        },
        async convertToClient(){
            this.$store.commit('setIsLoading',true)

            const leadId=this.$route.params.id
            const data={
                lead_id:leadId
            }
            await axios
                .post(`api/v1/convert_lead_to_client`,data)
                .then(response =>{
                    console.log('converted to client');

                    this.$router.push('/dashboard/clients')
                })
                .catch(error =>{
                    console.log(error);
                })
            
            this.$store.commit('setIsLoading',false)
        }
    }
}
</script>