<template>
    <div class="">
        <label>標題:</label>
        <input type="text" v-model="form.title" @keyup.enter="send()">
        <button type="button" @click="send()">送出</button>
        <button type="button" @click="clear()">清空所有資料</button>
        <div>
            <div v-for="(data, dataKey) in dataList" :key="dataKey">
                <router-link :to="`/detail/${dataKey}`" title="連結內頁">{{data.title}}</router-link>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                form:{},
                title:``,
                dataList:[]
            }
        },
        created(){
            var self = this;
            window.Echo.channel('notification').listen('PushNotification', (e) => {
                if(e.data.log){
                    self.dataList = e.data.log
                }
            });
            self.init();
        },
        methods:{
            init(){
                var self = this;
                axios({
                    method: 'post',
                    url: '/getlist',
                })
                    .then(function(response){
                        self.dataList = response.data;
                    })
            },
            send(){
                var self = this;
                axios({
                    method: 'post',
                    url: '/push',
                    data: self.form
                })
                    .then(function(response){
                        console.log(response);
                    })
            },
            clear(){
                var self = this;
                axios({
                    method: 'post',
                    url: '/clear',
                    data: self.form
                })
                    .then(function(response){
                        console.log(response);
                    })
            }
        }
    }
</script>