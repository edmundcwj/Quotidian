<template>
    <div id="Profile">
     <div class="avatarBackground" style="position:relative">
    <div v-for="data in userAvatar" :key="data.id" >
        <img :src="`https://i.ibb.co/${data.source}`"
        :id="`${data.name}`"  
            :style= 
                "`
                z-index: ${data.layer};
                position:absolute; 
                height: 500px;
                top: 0em;
                left: -5%;
                clip: rect(29px, 400px, 400px, 0);
                `"
        > 
    </div>
    </div>


    <div class="collection">
        <br>
        <div v-for="user in users" v-bind:key="user.id" class="element">
            <p class>Username:</p>
        <div class="text"> {{ user.currentUser }}</div>
        </div>
        <div v-for="user in users" v-bind:key="user.id" class="element">
            <p>Bio:</p>
        <div class="text">{{ user.bio }}</div>
        </div>
        <div class="listfriend">
            <p>Friends</p>
             <div v-for="user in flist" v-bind:key="user.id" class="felement">
                <div class="flist">{{ user }}</div>
            </div>
        </div>

       <!-- <div class="editprof"><button class="btn-small">Edit bios</button></div> -->

    </div>
    </div>
</template>

<script>
//import image from './qPics/Madmaw.png';
import db from './firebaseInit';
import firebase from "firebase";
var auth = firebase.auth();
//console.log(auth.currentUser.uid)
export default{
    name: "users",
    data: function() {
        return{
            users: [],
            userAvatar: [],
            isLoggedin: false,
            flist: [],
            currentUser: "",
            //showbox: true,
            editdesp: "",
            //ProfilePic: image,
            adesp: "",
           // bio: ""
        }
    },
   methods:{
       setUserAvatar() {
        var userID = auth.currentUser.uid;
            db.collection('users').doc(userID).get().then((querySnapshot) =>{
                //allows to redraw at each function call, keeping it updated.
                //creates snapshot at usersID
                var currAvatar= [];
                var ava = [];
                ava = this.Json
                currAvatar = querySnapshot.data().inventory
                for(let i =0; i < currAvatar.length; i++){
                    this.userAvatar[i] = currAvatar[i]
                } 
                                    this.$forceUpdate()
            })
        },
   },
    created() {
      //this.isLoggedin = true;
      var userID = auth.currentUser.uid; 
        this.setUserAvatar();
        let currFriendsList = [];
         db.collection('users').doc(userID).get().then((querySnapshot) =>{
               // this.loading = false;
                this.flist = querySnapshot.data().friendslist
               const data = {
                    'currentUser': querySnapshot.data().username,
                    'bio': querySnapshot.data().bio,
                     
               }
                //this.setUserAvatar()
                this.users.push(data)
        })
       },
    /*friendsList(){
             let currFriendsList = [];
              db.collection('users').doc(userID).get().then((querySnapshot) =>{
               // this.loading = false;
               currFriendsList = querySnapshot.data().friendslist
               //console.log(doc.data().username)
               arrLength = currFriendsList.length;
               
        })
    },*/
      
};
</script>

<style scoped>
.avatarBackground{
    box-shadow: rgba(0, 0, 0, 0.09) 0px 2px 1px, rgba(0, 0, 0, 0.09) 0px 4px 2px, rgba(0, 0, 0, 0.09) 0px 8px 4px, rgba(0, 0, 0, 0.09) 0px 16px 8px, rgba(0, 0, 0, 0.09) 0px 32px 16px;
    border-radius: 9%;
    position: relative;
    display: block;
    margin-left: auto;
    margin-right: auto;
    border: 8px solid #26a69a;
    padding: 0px;
    height: 400px;
    width: 400px;
    background-color: #081F62;
}
.editprof{
    position: relative;
    left: 600px;
}
.collection{
    margin: 1rem 0 1rem 0;
    border: 5px solid #26a69a;
    border-radius: 25px;
    overflow: hidden;
    position: relative;
    background-color:#051747;
    justify-content: center;
    width: 90%;
    margin-left: auto;
    margin-right: auto;
    color: white;
 
}
.listfriend{
    background-color:#535F80;
    justify-content: space-between;
    line-height: 1.5rem;
    padding: 10px 70px 40px;
    margin: 0;
    border-bottom: 1px solid black;
    border-radius: 25px;
    margin-bottom: 1rem;
    color: white;
    width: 50%;
    margin-left: auto;
    margin-right: auto;
}
.felement {
   background-color: #26a69a;
    justify-content: space-between;
    line-height: 1.5rem;
    padding: 10px 70px 40px;
    margin: 0;
    border-bottom: 1px solid black;
    border-radius: 25px;
    margin-bottom: 1rem;
    color: white;
    width: 100%;  
}
.element {
   background-color:#535F80;
    justify-content: space-between;
    line-height: 1.5rem;
    padding: 10px 70px 40px;
    margin: 0;
    border-bottom: 1px solid black;
    border-radius: 25px;
    margin-bottom: 1rem;
    color: white;
    width: 50%;
    margin-left: auto;
    margin-right: auto;   
}
.picture{
    text-align: center;
}
.flist {
    justify-content: space-between;
}
.Fullpage{
    background-color: rgb(0, 0, 196);
    border: black 3px solid;
}
.profilename{
    position: relative;
    left: 300px;
    width: 300px;
    height: 50px;
    font-size: 200%;
    margin: 10px;
    /*border: blue 3px solid;*/
    background-color: white;
}
.pfp{
    position: relative;
    left: 400px;
    width: 300px;
    height: 300px;
    /*border: red 3px solid;*/
    background-color: grey;
}
.desp{
    position: relative;
    left: 300px;
    width: 500px;
    height: 300px;
    margin: 10px;
    /*border: green 3px solid;*/
    background-color: rgb(255, 255, 255);
}
.editbox{
    position: relative;
    left: 300px;
    width: 500px;
    height: 300px;
    margin: 10px;
    /*border: green 3px solid;*/
    background-color: rgb(255, 255, 255);
}
.edit button{
    position: relative;
    left: 300px;
    border: black 3px solid;
}
.txt{
    max-height: 100%;
	max-width: 100%;
}
.myPic{
    max-height: 100%;
	max-width: 100%;
}
</style>
