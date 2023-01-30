
<template>
 <div class="app">  

  <div class="forDelete">
    <p></p>
  <button type="button" class="removeAll" id="choose" v-show="isChecked" @click="removeChoosePosts">Видалити вибрані пости</button>
    <input type="checkbox" v-model="isChecked">
  
  </div>

        <p class="create">{{ titileOfPage}}</p>
<div class="main-div">
    <form id="task-form" @submit.prevent>
      <input 
     type="text"
      class="add-item" 
      placeholder="Введіть ім'я посту"
      v-model="title"/>
        <button type="submit" class="add-btn"  @click="addPost">Add</button>
     <p class="error" v-show="error">Enter title for the post</p>

    </form>
      </div>
 </div>


 <div class="posts">
          <div class="post" 
                  v-for="post in posts" 
                  v-bind:key="post.id">

                  <div class="content">
                           <input type="text" class="inp" v-model="post.title" :disabled="post.disabled">
                           <input type="text" class="inp" v-model="post.description" :disabled="post.disabled">
                                  
                           <div class="container2" v-if="post.imageUrl!=null" @click="clickContainer">
     
                            <input type="file" class="input_image" accept="image/*" :disabled="post.disabled"  @change="onChange($event, post)" hidden>
                            <img :src="post.imageUrl" id="chosenimage">
                            <button v-if="post.imageUrl!=null" class="cancel-image" @click="delImage(post)">
                               </button>
                              </div>


                              <div class="container" v-else-if="post.imageUrl==null"  @click="clickContainer">
                                 <input type="file" id="input_image" accept="image/*" :disabled="post.disabled"  @change="onChange($event, post)" hidden>
                            <img :src="post.imageUrl" id="chosenimage">
                            <button v-if="post.imageUrl!=null" class="cancel-image" @click="delImage(post)">
                               </button>
                              </div>

                        

                 </div>

               <div class="actions">
                      <button class="status" disabled >{{post.status}}</button> 
                      <button class="edit" @click="edit(post)" >Edit</button>
                      <button class="save"  @click="save(post)">Save</button>
                      <button class="delete" @click="removePost(post.id)">Delete</button>
                      <input type="checkbox" class="checkbox" v-model="post.check">
                 </div>


                </div>

</div>


</template>

<script>

export default{
//components:{postForm, //postList,},
    data()
    {
      return{
        titileOfPage: 'Створити пост)',
        posts:[
  //  {id:1, title:"First",  check: false, image:"0",  disabled:true, status:'Draft', imageUrl:null},
    //{id:2, title:"Second",  check:false, image:"0",  disabled:true, status:'Draft' ,  imageUrl:null},
    //{id:3, title:"Third",  check: false, image:"0",  disabled:true, status:'Draft',  imageUrl:null},
      ],
       check:false,
      title:"",
      description:"",
      error:false,
      isChecked:false,  
      localPosts:[],
      status:"Draft",
      disabled:true,
      imageUrl: null,
       }
    },

methods: {
clickContainer(){
document.getElementById('input_image').click();
},

  onChange($event,post){
                   const image = event.target.files[0];
                const reader = new FileReader();
                reader.readAsDataURL(image);
                reader.onload = event =>{
                    post.imageUrl = event.target.result;
                    //console.log(this.previewImage);
                };},

delImage(post){
         post.imageUrl=null;
                },


      addPost(){ 
         if(this.title=="" || this.title.trim()=="") this.error=true;
else{
  this.error=false;
const newPost={
  id:Date.now(),
  title:this.title,
  description: this.description,
  status:this.status,
  imageUrl: this.imageUrl,
  check:this.check,
  disabled:this.disabled,

}
this.posts.push(newPost);
this.title="";
}
},
edit(post){
  post.disabled=false;
},

save(post){

   post.disabled=true;
   if (post.status=='Draft'){
   this.localPosts.push(post);
    post.status='Published'
    localStorage.setItem("posts", JSON.stringify(this.localPosts));
  }
else {
if(post.status=='Published'){

localStorage.setItem("posts", JSON.stringify(this.localPosts));}
}},

removePost(postId){
  this.posts=this.posts.filter(p=>p.id!=postId);
  this.localPosts=this.localPosts.filter(p=>p.id!=postId);
  localStorage.setItem("posts", JSON.stringify(this.localPosts));
},

removeChoosePosts() {
  let arrayForDelete=[];
            for(let i = 0; i < this.posts.length; i++) {    
              if(this.posts[i].check==true){
               arrayForDelete.push(this.posts[i].id)
               //  console.log(this.posts[i].title);               
                }
              }

              for(let i=0; i<arrayForDelete.length;i++)
              {
                   this.posts = this.posts.filter(p=>p.id!=arrayForDelete[i]);   
                   this.localPosts=this.localPosts.filter(p=>p.id!=arrayForDelete[i]);
                    localStorage.setItem("posts", JSON.stringify(this.localPosts));
              }
         
},

  }}

</script>

<style>
body{
    background: linear-gradient( to right, #8c00ff , #ca18e0,#f711c8);
    margin: 0 auto;
}


.post{
    width:400px;
     background-color: aquamarine;
     color: black;
     padding: 10px;
     margin: 10px;
}

.add-btn{
  color: white;
  margin-left: -80px;
  border: none;
  border-radius: 10px;
  background: linear-gradient( to right, #fc9c02 , #fe3a3a);
  width: 80px;
  height: 40px;
}

  .main-div{
    display: flex;
    justify-content: center;
    margin: 0 auto;
  }

  .add-item{
  padding: 0 5px;
 height: 40px;
  width: 400px;
  border: none;
  border-radius: 10px;
  }

  .post{
  background-color: white;
  width: 400px;
  padding: 10px;
  border-radius: 20px;
  margin: 10px;
}
.posts{
   display: flex;
   justify-content: center; 
   flex-wrap: wrap;
   margin-top: 20px;
    }


    .content{
  display: flex;
  flex-direction: column;
}

.content input{
  cursor: pointer;
  width: 380px;
  background-color: white;
  border-radius: 10px;
  margin: 5px 0;
  height: 30px;
}


.actions{
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.status{
  background-color: white;
  border: none;
}

.save, .delete, .edit
{

  color: white;
  border: none;
  border-radius: 10px;
  background: linear-gradient( to right, #fc9c02 , #fe3a3a);
  width: 100px;
  height: 20px;
}
.forDelete{
  display: flex;
  justify-content: space-between;
}

#choose{
    margin: 10px auto 0 auto;
    color: white;
    border: none;
    border-radius: 10px;
    background: linear-gradient( to right, #fc9c02 , #fe3a3a);
    width: 250px;
    height: 26px;
  
 }

 .container {
  position: relative;
  width: 380px;
  height: 380px;
  margin: 5px;
  background-image: url(gallery.png);
  background-size: cover;
}

.container2 {
  position: relative;
  width: 380px;
  height: 380px;
  margin: 5px;
  background-color: white;
   background-size: cover;
}

#chosenimage{
  border-radius: 10px;
  border: 2px solid black;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 0;
}

.cancel-image{
 background-image: url(cancel.png);
 background-color:white;
 border: none;
 border-radius: 20px;
 background-size: cover;
 position: absolute;
  top: 15px;
  right: 10px;
  width: 40px;
  height: 40px;
  opacity: 0;
}

.cancel-image:hover{
  opacity: 1;
}
.error{
  margin-bottom: 0;
}

.create, .error{
  color: white;
  text-align: center;
  font-size: 18px;
}


</style>