<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">{{ lead.company }}</h1>

                <router-link :to="{name:'EditLead',params:{id:lead.id}}" class="button is-light">Edit</router-link>
            </div>

            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">Details</h2>
                    <template v-if="lead.assigned_to">
                        <p><strong>Assigned To : </strong>{{ lead.assigned_to.username }}</p>
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
        }
    }
}
</script>