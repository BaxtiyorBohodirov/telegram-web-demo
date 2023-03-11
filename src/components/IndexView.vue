<template>
  <div class="container-fluid h-100">
    <div class="row justify-content-center h-100">
      <div class="col-md-4 col-xl-3 chat" style="height: 90%">
        <div class="card mb-sm-3 mb-md-0 contacts_card" style="height: 100%">
          <div class="card-header">
            <div class="input-group">
              <input
                type="text"
                placeholder="Search..."
                name=""
                v-model="contact_search"
                class="form-control search"
              />
              <div class="input-group-prepend">
                <span class="input-group-text search_btn"
                  ><i class="fas fa-search"></i
                ></span>
              </div>
            </div>
          </div>
          <div class="card-body contacts_body">
            <ui class="contacts" v-if="contacts.length>0">
              <li  v-for="(contact,index) in contacts" :key="contact.id" :class="index==active_contact?'active':''" @click="changeContact(contact.id)" style="cursor: pointer;">
                <div class="d-flex bd-highlight">
                  <div class="img_cont">
                    <img
                      :src="contact.image"
                      class="rounded-circle user_img"
                    />
                    <span class="online_icon" :class="contact.is_online?'':'offline'"></span>
                  </div>
                  <div class="user_info">
                    <span>{{contact.name}}</span>
                    
                    <p v-if="contact.is_online">{{contact.name}} is online</p>
                    <p v-else>{{contact.last_active}}</p>
                  </div>
                </div>
              </li>
            </ui>
            <div v-else class="row justify-content-center" style="height:90%; width: 100%; align-items: center; ">
                <h3 style="text-align: center; color: white;">
                  No Contacts...
                </h3>
            </div>
          </div>
          <div class="card-footer"></div>
        </div>
      </div>
      <div class="col-md-8 col-xl-6 chat" style="height: 90%">
        <div class="card" style="height: 100%" v-if="contacts.length>0">
          <div class="card-header msg_head">
            <div class="d-flex bd-highlight">
              <div class="img_cont">
                <img
                  :src="contacts[active_contact].image"
                  class="rounded-circle user_img"
                />
                <span class="online_icon" :class="!contacts[active_contact].is_online?'offline':''"></span>
              </div>
              <div class="user_info">
                <span>Chat with {{contacts[active_contact].name}}</span>
                <p>1767 Messages</p>
              </div>
              <div class="video_cam">
                <span><i class="fas fa-video"></i></span>
                <span><i class="fas fa-phone"></i></span>
              </div>
            </div>
            <span id="action_menu_btn"><i class="fas fa-ellipsis-v"></i></span>
            <div class="action_menu">
              <ul>
                <li><i class="fas fa-user-circle"></i> View profile</li>
                <li><i class="fas fa-users"></i> Add to close friends</li>
                <li><i class="fas fa-plus"></i> Add to group</li>
                <li><i class="fas fa-ban"></i> Block</li>
              </ul>
            </div>
          </div>
          <div class="card-body msg_card_body">
            <div v-for="message in contacts[active_contact].messages" :key="message.id">
              <div v-if="message.user_id!=user.id" class="d-flex justify-content-start mb-4">
                <div class="img_cont_msg">
                  <img
                    :src="contacts[active_contact].image"
                    class="rounded-circle user_img_msg"
                  />
                </div>
                <div class="msg_cotainer">
                  {{message.text}}
                  <span class="msg_time">{{message.date}}</span>
                </div>
              </div>
              <div v-else class="d-flex justify-content-end mb-4">
                <div class="msg_cotainer_send">
                 {{message.text}}
                  <span class="msg_time_send">{{message.date}}</span>
                </div>
                <div class="img_cont_msg">
                  <img
                    :src="user.image"
                    class="rounded-circle user_img_msg"
                  />
                </div>
              </div>
            </div>
            
          </div>
          <div class="card-footer">
            <div class="input-group">
              <div class="input-group-append">
                <span class="input-group-text attach_btn"
                  ><i class="fas fa-paperclip"></i
                ></span>
              </div>
              <textarea
                name=""
                
                v-model="new_message"
                class="form-control type_msg"
                placeholder="Type your message..."
              ></textarea>
              <div class="input-group-append" @click="sendMessage">
                <span class="input-group-text send_btn"
                  ><i class="fas fa-location-arrow"></i
                ></span>
              </div>
            </div>
          </div>
        </div>
        <div v-else class="card row justify-content-center" style="height:100%; width: 100%; align-items: center; ">
                <h3 style="text-align: center; color: white;">
                  No Contacts...
                </h3>
            </div>
      </div>
    </div>
  </div>
</template>

<script>
  import myimage from "../assets/images/myimage.png";
  export default {
    data(){
      return {
        active_contact:0,
        user:{
          id:1,
          name:"MyName",
          image:myimage
        },
        allcontacts:
        [
          {
            id:1,
            name:"Khalid",
            image:"https://static.turbosquid.com/Preview/001292/481/WV/_D.jpg",
            is_online:true,
            last_active:"",
            messages:[
            {
              id: 1,
              user_id: 2,
              text: "Hi, how are you samim?",
              date: "8:40 AM, Today",
            },
            {
              id: 2,
              user_id: 1,
              text: "Hi Khalid i am good tnx how about you?",
              date: "8:55 AM, Today",
            },
            {
              id: 3,
              user_id: 2,
              text: "I am good too, thank you for your chat template",
              date: "9:00 AM, Today",
            },
            {
              id: 4,
              user_id: 1,
              text: "You are welcome",
              date: "9:05 AM, Today",
            },
            {
              id: 5,
              user_id: 2,
              text: "I am looking for your next templates",
              date: "9:07 AM, Today",
            },
            {
              id: 6,
              user_id: 1,
              text: "Ok, thank you have a good day",
              date: "9:10 AM, Today",
            },
            {
              id: 7,
              user_id: 2,
              text: "Bye, see you",
              date: "9:12 AM, Today",
            },
            ]
          },
          {
            id:2,
            name:"Taherah Big",
            image:"https://2.bp.blogspot.com/-8ytYF7cfPkQ/WkPe1-rtrcI/AAAAAAAAGqU/FGfTDVgkcIwmOTtjLka51vineFBExJuSACLcBGAs/s320/31.jpg",
            is_online:false,
            last_active:"Taherah left 7 mins ago",
            messages:[]
          },
          {
            id:3,
            name:"Sami Rafi",
            image:"https://i.pinimg.com/originals/ac/b9/90/acb990190ca1ddbb9b20db303375bb58.jpg",
            is_online:true,
            last_active:"",
            messages:[]
          },
          {
            id:4,
            name:"Nargis Hawa",
            image:"https://static.turbosquid.com/Preview/001292/481/WV/_D.jpg",
            is_online:false,
            last_active:"Nargis left 30 mins ago",
            messages:[]
          },
          {
            id:5,
            name:"Rashid Samim",
            image:"https://static.turbosquid.com/Preview/001214/650/2V/boy-cartoon-3D-model_D.jpg",
            is_online:false,
            last_active:"Rashid left 50 mins ago",
            messages:[]
          },
        ],
        contacts:[],
        contact_search:"",
        new_message:""
      }
    },
    methods:{
      changeContact(id){
        this.contacts.forEach((x,y) => {
          if(x.id == id)
          {
            this.active_contact = y
          }
        })
      },
      sendMessage(){
        let messages = this.contacts[this.active_contact].messages
        const date = new Date();
        const id =messages.length>0? messages[messages.length-1].id+1:1;
        const message = {
              id: id,
              user_id: 1,
              text: this.new_message,
              date: date.getHours()+":"+date.getMinutes(), 
        }
        
        this.contacts[this.active_contact].messages.push(message)
        this.new_message = ''
      }
    },
    watch:{
      contact_search(new_val){
        this.contacts = this.allcontacts.filter(function(x){
          return x.name.toLowerCase().includes(new_val.toLowerCase())
        })
      }
    },
    mounted(){
      this.contacts = this.allcontacts;
    }
  };
</script>
<style scoped>
    .container-fluid
    {
      height: 100vh !important;
    }
</style>