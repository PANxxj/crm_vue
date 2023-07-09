<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Add Lead</h1>
            </div>

            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label >Company</label>
                        <div class="control">
                            <input type="text" name="company" class="input" v-model="company">
                        </div>
                    </div>

                    <div class="field">
                        <label >Contact Person</label>
                        <div class="control">
                            <input type="text" name="contact_person" class="input" v-model="contact_person">
                        </div>
                    </div>

                    <div class="field">
                        <label >Email</label>
                        <div class="control">
                            <input type="text" name="email" class="input" v-model="email">
                        </div>
                    </div>

                    <div class="field">
                        <label >Phone</label>
                        <div class="control">
                            <input type="text" name="phone" class="input" v-model="phone">
                        </div>
                    </div>

                    <div class="field">
                        <label >Website</label>
                        <div class="control">
                            <input type="text" name="website" class="input" v-model="website">
                        </div>
                    </div>

                    <div class="field">
                        <label >Confidience</label>
                        <div class="control">
                            <input type="number" name="confidence" class="input" v-model="confidence">
                        </div>
                    </div>

                    <div class="field">
                        <label >Estimated value</label>
                        <div class="control">
                            <input type="number" name="estimation_value" class="input" v-model="estimation_value">
                        </div>
                    </div>

                    <div class="field">
                        <label >Status</label>
                        <div class="control">
                            <select class="select is-fullwidth" v-model="status">
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
                            <select class="select is-fullwidth" v-model="priority">
                                <option value="Low">Low</option>
                                <option value="Medium">Medium</option>
                                <option value="High">High</option>
                            </select>
                        </div>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Submit</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { toast } from 'bulma-toast';
export default {
    name:'AddLead',
    data(){
        return {
            company:'',
            contact_person:'',
            email:'',
            phone:'',
            website:'',
            confidence:0,
            estimation_value:0,
            status:'New',
            priority:'Medium'
        }
    },
    methods:{
        async submitForm(){
            this.$store.commit('setIsLoading',true)
            console.log('form submitted');
            const lead ={
                company:this.company,
                contact_person:this.contact_person,
                email:this.email,
                phone:this.phone,
                website:this.website,
                confidence:this.confidence,
                estimation_value:this.estimation_value,
                status:this.status,
                priority:this.priority
            }

            await axios
                .post('api/v1/leads/',lead)
                .then(response =>{
                    console.log(response);
                    toast({
                            message:'The lead was added',
                            type:'is-success',
                            dismissible:true,
                            pauseOnHover:true,
                            duration:2000,
                            position:'bottom-right'
                        })

                    this.$router.push('/dashboard/leads')
                })
                .catch(error =>{
                    console.log(error);
                })

            this.$store.commit('setIsLoading',false)
        }
    },
    // mounted:{
    //     // console.log(priority)
    // }
}

</script>