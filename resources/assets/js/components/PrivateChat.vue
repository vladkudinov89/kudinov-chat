<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-sm-12">
                <div class="card card-default">
                    <textarea class="form-control" cols="30" rows="10" readonly>
                        {{ messages.join('\n')}}
                    </textarea>
                    <hr>
                    <input type="text" class="form-control" v-model="textMessage" @keyup.enter="sendMessage">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Chat",
        props: ['room'],
        data(){
          return {
              messages : [],
              textMessage : ''
          }
        },
        mounted() {
            console.log('Component mounted.');
            console.log(this.room);
            window.Echo.private('room.' + this.room.id)
                .listen('PrivateChat' , ({data}) => {
                   this.messages.push(data.body)
                });
        },
        methods: {
            sendMessage(){
                axios.post('/messages' , { body: this.textMessage , room_id : this.room.id });
                this.messages.push(this.textMessage);
                this.textMessage = '';
            }
        }
    }
</script>

<style scoped>

</style>