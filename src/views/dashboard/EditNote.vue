<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Edit Note </h1>
            </div>

            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label >Name</label>
                        <div class="control">
                            <input type="text" name="name" class="input" v-model="note.name">
                        </div>
                    </div>

                    <div class="field">
                        <label >Body</label>
                        <div class="control">
                            <textarea class="textarea" v-model="note.body"></textarea>
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
import axios from 'axios';
import { toast } from 'bulma-toast';
export default {
    name:'EditNote',
    data(){
        return {
            note:{}
        }
    },
    mounted(){
        this.getNote()
    },
    methods:{
        async getNote(){
            this.$store.commit('setIsLoading',true)

            const noteId=this.$route.params.note_id
            await axios
                .get(`api/v1/leads/${noteId}`)
                .then(response =>{
                    this.note=response.data
                })
                .catch(error =>{
                    console.log(error);
                })
            
            this.$store.commit('setIsLoading',false)
        },
        async submitForm(){
            this.$store.commit('setIsLoading',true)
            await axios
                .patch(`api/v1/notes/${this.note.id}`,this.note)
                .then(response =>{
                    console.log(response);
                    toast({
                            message:'The note has been updated',
                            type:'is-success',
                            dismissible:true,
                            pauseOnHover:true,
                            duration:2000,
                            position:'bottom-right'
                        })

                    this.$router.push({name:'Client',params:{'id':this.$route.params.id}})
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