<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Edit {{ lead.company }}</h1>
            </div>
            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label >Company</label>
                        <div class="control">
                            <input type="text" name="company" class="input" v-model="lead.company">
                        </div>
                    </div>

                    <div class="field">
                        <label >Contact Person</label>
                        <div class="control">
                            <input type="text" name="contact_person" class="input" v-model="lead.contact_person">
                        </div>
                    </div>

                    <div class="field">
                        <label >Email</label>
                        <div class="control">
                            <input type="text" name="email" class="input" v-model="lead.email">
                        </div>
                    </div>

                    <div class="field">
                        <label >Phone</label>
                        <div class="control">
                            <input type="text" name="phone" class="input" v-model="lead.phone">
                        </div>
                    </div>

                    <div class="field">
                        <label >Website</label>
                        <div class="control">
                            <input type="text" name="website" class="input" v-model="lead.website">
                        </div>
                    </div>

                    <div class="field">
                        <label >Confidience</label>
                        <div class="control">
                            <input type="number" name="confidence" class="input" v-model="lead.confidence">
                        </div>
                    </div>

                    <div class="field">
                        <label >Estimated value</label>
                        <div class="control">
                            <input type="number" name="estimation_value" class="input" v-model="lead.estimation_value">
                        </div>
                    </div>

                    <div class="field">
                        <label >Status</label>
                        <div class="control">
                            <select class="select is-fullwidth" v-model="lead.status">
                                <option value="New">New</option>
                                <option value="Contacted">Contacted</option>
                                <option value="Lost">Lost</option>
                                <option value="InProgress">InProgress</option>
                                <option value="Won">Won</option>
                            </select>
                        </div>
                    </div>

                    <div class="field">
                        <label >Priority</label>
                        <div class="control">
                            <select class="select is-fullwidth" v-model="lead.priority">
                                <option value="Low">Low</option>
                                <option value="Medium">Medium</option>
                                <option value="High">High</option>
                            </select>
                        </div>
                    </div>

                    <div class="field">
                        <label >Assigned to</label>
                        <div class="control">
                            <select class="select is-fullwidth" v-model="lead.assigned_to">
                                <option value="" selected>Select member</option>
                                <option v-for="member in team.members"
                                :key="member.id"
                                v-bind:value="member.id"
                                >{{ member.username }}</option>
                            </select>
                        </div>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Update</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
export default {
    name:'EditLead',
    data(){
        return {
            lead:{},
            team:{
                members:[]
            }
        }
    },
    mounted(){
        this.getLead()
        this.getTeam()
    },
    methods:{
        async getLead(){
            this.$store.commit('setIsLoading',true)

            const leadId=this.$route.params.id
            axios
            .get(`api/v1/leads/${leadId}`)
            .then(response =>{
                this.lead=response.data
            })
            .catch(error =>{
                console.log(error);
            })
            
            this.$store.commit('setIsLoading',false)
        },
        async submitForm(){
            this.$store.commit('setIsLoading',true)
            const leadId=this.$route.params.id

            axios
                .patch(`api/v1/leads/${leadId}/`,this.lead)
                .then(response =>{
                    console.log(response);
                    toast({
                            message:'The lead was updated',
                            type:'is-success',
                            dismissible:true,
                            pauseOnHover:true,
                            duration:2000,
                            position:'bottom-right'
                        })

                    this.$router.push(`/dashboard/leads/${leadId}`)
                })
                .catch(error =>{
                    console.log(error);
                })

            this.$store.commit('setIsLoading',false)
        },
        async getTeam(){
            this.$store.commit('setIsLoading',true)
            await axios
                .get('api/v1/teams/get_my_team')
                .then(response =>{
                    this.team=response.data
                    console.log(response.data);
                })
                .catch(error =>{
                        console.log(error);
                })

            this.$store.commit('setIsLoading',false)
        }
    }
}

</script>