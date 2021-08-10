<template>

<div id="Holder"><h3>Add New User</h3>
<label id="Username">Username
<input id="user" type="text" name="username">
</label><br>
<label id="Num">Tel Number
<input id="num" type="text" name="number">   
</label><br>
<button id="fix" @click="AddUser">Add User</button>
</div>

<table>
  <tr>
    <td>id</td>
    <td>UserName</td>
    <td>Phone Num</td>
    <td>Remove User</td>
  </tr>
<tr v-for="(item,key) in AllUsers" :key="key" :class="`row${key}`">
    <td>{{key}}</td>
    <td class="name1">{{item.name}}</td>
    <td class="num1">{{item.num}}</td>
    <td><button  @click="Remove(key,item.name,item.num)">Remove</button></td>
</tr>

<tr>
  <th colspan="4" v-if="Timer"><h3>Removed Users</h3></th>
</tr>
<tr v-for="(item,key) in AllDel" :key="key" :class="`rows row1${key}`">
    <td>{{key}}</td>
    <td class="name3">{{item.name}}</td>
    <td class="num3">{{item.num}}</td>

</tr>
</table>


</template>

<script>
import $ from 'jquery';


export default{

created() {
    window.addEventListener('load', this.load);   
  },

  

data(){

  return{

    Timer:false,
    AllUsers:[],
    AllDel:[],
  }

},

methods:{

  AddUser(){
    let user=$('#user');
    let num=$('#num');

    if(user.val()==""){
      user.val('None');
    }if(num.val()==""){
      num.val("None");
    }


    var obj ={
      name:user.val(),
      num:num.val(),
    }

    
    this.AllUsers.push(obj);
    for(let i=0;i<129;i++){
      let testing=`close-${i}`;
      let testing1=localStorage.getItem(testing);

      if(testing1==null){
        localStorage.setItem(`close-${i}`,user.val());
        localStorage.setItem(`closenum-${i}`,num.val());

        break;
      }


    }
    

    setTimeout( ()=>{
      user.val('');
      num.val('');
    },500 );


  },

  Remove(key,name,num){

    for(let i=0;i<120;i++){
      let remove0=`remove-${i}`;
      let remove1=localStorage.getItem(remove0);
      if(remove1){

        if(remove1==name){
         localStorage.setItem(`remove-${i}`,name);
         localStorage.setItem(`removenum-${i}`,num);
         break;
                      }
        }else{
         localStorage.setItem(`remove-${i}`,name);
         localStorage.setItem(`removenum-${i}`,num);
         break;
        }


    }


   for(let i=0;i<120;i++){
      let localname=`close-${i}`;
      let localnum=`closenum-${i}`;
      let testname=localStorage.getItem(localname);
      let testnum=localStorage.getItem(localnum);
      if(testname == name && testnum == num){
        localStorage.removeItem(localname);
        localStorage.removeItem(localnum);
        break;
      }

   } 
    
    var row=$(`.row${key}`);
    row.remove();

  },

    load(){
      // localStorage.clear();
      for(let i=0;i<120;i++){
      let removename=`remove-${i}`;
      let removenum=`removenum-${i}`;
      let removename1=localStorage.getItem(removename);
      let removenum1=localStorage.getItem(removenum);
      
      if(removename1){

        this.Timer=true;

        let jokername={
          name:removename1,
          num:removenum1
        }
    
      this.AllDel.push(jokername);
          
      }

      let closename=`close-${i}`;
      let closenum=`closenum-${i}`;
      let closename1=localStorage.getItem(closename);
      let closenum1=localStorage.getItem(closenum);

      if(closename1){

        let jokername1={
          name:closename1,
          num:closenum1
        }

        this.AllUsers.push(jokername1);

      }

    }


    setTimeout( ()=>{

      let rowss=$('.rows');
      for(let i=0;i<rowss.length;i++){
        $(rowss[i]).remove();
      }
      
      this.Timer=false;
    },15000 );
    }

}


}


</script>

<style>

  body{
    background: lightblue;
  }

  #Holder{
    border: 3px solid green;
    width: 450px;
    height: 180px;
    text-align: center;
    margin-left: auto;
    margin-right: auto;
  }


  input{
    margin-top: 10px;
  }

  #fix{
    margin-top: 20px;
  }


  table{
    margin-top: 30px;
    width: 99%;
    border: 3px solid black;
    text-align: center;
  }

  tr ,td{
    border: 1px solid black;
  }
</style>
