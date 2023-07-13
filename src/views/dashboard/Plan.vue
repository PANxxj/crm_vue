<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Plans</h1>
            </div>

            <div class="column is-4">
                <div class="box">
                    <h2 class="subtitle">Free</h2>
                    <h4 class="is-size-3">Rs.0</h4>

                    <p>Max 5 clients </p>
                    <p>Max 5 leads </p>

                    <button @click="subscribe('free')" class="button is-primary mt-6">Subscribe</button>
                </div>
            </div>

            <div class="column is-4">
                <div class="box">
                    <h2 class="subtitle">Small Team</h2>
                    <h4 class="is-size-3">Rs.10</h4>

                    <p>Max 15 clients </p>
                    <p>Max 15 leads </p>
                    <button @click="subscribe('smallteam')" class="button is-primary mt-6">Subscribe</button>
                </div>
            </div>

            <div class="column is-4">
                <div class="box">
                    <h2 class="subtitle">Big Team</h2>
                    <h4 class="is-size-3">Rs.50</h4>

                    <p>Max 50 clients </p>
                    <p>Max 50 leads </p>
                    <button @click="subscribe('bigteam')" class="button is-primary mt-6">Subscribe</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name:'Plan',
    data(){
        return {
        }
    },
    methods:{
        async subscribe(plan){
            this.$store.commit('setIsLoading',true)

            const data={
                plan:plan
            }

            await axios
                    .post(`api/v1/teams/upgrade_plan`,data)
                    .then(response =>{
                        console.log(response.data);
                        this.$store.commit('setTeam',{
                            'id':response.data.id,
                            'name':response.data.name,
                            'plan':response.data.plan.name,
                            'max_leads':response.data.plan.max_leads,
                            'max_clients':response.data.plan.max_clients
                    
                    });
                    this.$router.push('/dashboard/team')
                    })
                    .catch(error =>{
                        console.log(error);
                    })

                this.$store.commit('setIsLoading',false)
        }
    }
}
</script>