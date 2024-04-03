<template>
    <div class="container">
      <div class="centered">
        
        <v-btn @click="join">Войти</v-btn>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { ref } from 'vue';
  export default{
    data(){
        return{
            login:"",
            password:"",
            isPwd : ref(false),
            rules: [
              value => !!value || 'Обязательно',
              value => (value && value.length >= 3) || 'Min 3 characters',
          ],
        }
    },
    methods:{
        join(){
            let request = {
                email:"test",
                password:"test1",
  
            }
            if(this.ValidMail(this.$data.login)){
                request.email = this.$data.login
            }else{
                if(this.ValidPhone(this.$data.login)){
                    request.phone = this.$data.login
                }else{
                    alert("Наверно указан телефон или электронная почта")
                    return
                }
            }
            console.log(request)
            let response = axios.post("http://192.168.1.104:3000/users/auth/login",request,{
                headers:{
                  "Content-type":"application/json"
                }
            })
  
            axios.delete("http://192.168.1.104:3000/users/admin/deleteUser",
            {
                data:{
                    email:"test"
                },
                headers:{
                    "Content-type":"application/json",
                    "Authorization":"Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjIiLCJpYXQiOjE3MTIxNDQ0NDcsImV4cCI6MTcxMjE0NjI0N30.nIia1vP4mQ33PS5CE6nbHAJTRbam1e0VerEX32u_5jY"
                }
               
              
              
            },
            
            )
            console.log(response.data)
        },
        ValidMail(myMail) {
            const re = /^[\w-\.]+@[\w-]+\.[a-z]{2,4}$/i;
            return  re.test(myMail);
        },
        ValidPhone(myPhone) {
            const re = /^[\d\+][\d\(\)\ -]{4,14}\d$/;
            return re.test(myPhone);;
        }  
  
    }
  }
  
  </script>
  
  <style>
  .container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Устанавливаем высоту контейнера равную высоте видимой области страницы */
  }
  
  .centered {
    width: 30vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 4px 4px 4px 6px rgb(130, 120, 120);
  }
  .centered *{
  
    margin: 5px;
  }
  input, .v-field__field{
    width: 30vw;
    height: 10vh;
    transition-delay: 15ms;
  }
  button{
    text-align: center;
  }
  .q-btn {
  z-index: 1; /* Установите значение z-index для кнопки */
  }
  
  </style>