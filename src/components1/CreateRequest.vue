
<template>
    <div>
      <base-button class="btn btn-success btn-lg float-right" type="info" @click.native="modals.classic = true">
        Create a Request
      </base-button>
  
    <!-- Here is the model -->
      <modal :show.sync="modals.classic" size="sm"
       body-classes="p-0">
 
      <!-- card -->
        <card type="secondary"
        header-classes="bg-transparent pb-5"
        body-classes="px-lg-5 py-lg-5"
        class="border-0 mb-0">
                <form>
                  <div class="form-row">
                    <h3 class="text-center" id="heading">Create a Request</h3>
                     <base-button type="danger" class="btn btn-dark" id="button" @click.native="modals.classic = false">
                      ×
                    </base-button>
                  
                  </div>
                  <br>

                    <div class="form-row" id="requesttype">
                        
                       <base-input  label="Request Type" slot="title-container" type="secondary">
                            <select class="form-control" v-model="selected">
                            <option disabled value="">Choose a request</option>
                            <option class="greenText">VPN Access</option>
                            <option class="font-weight-bold">Email id Generation</option>
                            <option class="font-weight-bold">New Laptop</option>
                            </select>
                      </base-input>
                        
                    </div>

      
                     <div class="form-group" id="requesttype">
                        <label>Description</label>
                        <textarea class="form-control" id="exampleFormControlTextarea1" rows="3" placeholder="Write a large text here ..." v-model="input.description"></textarea>
                    </div>
                </form>

        <!--footer-->
        <template slot="footer">

           

             <base-button block type="default" class=" mb-3" v-on:click="submitrequest()">
                Save
            </base-button>

             <base-alert type="success" dismissible v-if="successmessage != ''">
                <span><b> {{successmessage}}</b></span>
            </base-alert>

            <base-alert type="danger" dismissible v-if="errormessage != ''">
                <span><b> {{errormessage}}</b></span>
            </base-alert>

          </template>
         </card>
      </modal>
    </div>
</template>

<script>

  import {Modal} from '@/components';
 
  import { BaseAlert } from '@/components';
  import Request from '../screens/Request';
   import { EventBus } from '../main.js';
  
    export default {
      components :{
          Modal,
          // VModal,
          BaseAlert,
          Request
      },
      name: 'CreateRequest',
      

      data(){
        return {
          modals: {
            classic: false,
          },
          successmessage: '',
          selected: '',
          errormessage: '',
          input : {
              request: '',
              option1: '',
              option2: '',
              description: '',
          }
        }
      },
      methods: {
        async submitrequest (){ 
            if(this.input.description != "" && this.selected != "") {
              await this.$axios.post(this.$Request, {
                    Info: this.selected,
                    Description: this.input.description,
                    headers: {
                        'Authorization': `${this.$cookie.get('token')}`
                    }
                  })
                    .then(response => {
                    // JSON responses are automatically parsed.
                      console.log(response)
                      this.successmessage="Your request has been created";

                      EventBus.$emit('RequestGenerated', 'request');


                       setTimeout(function () {
                          this.modals.classic = false;
                        }.bind(this), 500);


                  })
                    .catch(e => {
                      console.log(e)
                  })
            }
            else{
                this.errormessage="Please fill the form"
              }
          },

       }
    }
</script>


<style type="css">

#button {
  position: absolute;
  
    left:440px;
    padding: 15px;
}

#heading {

  position: relative;
  left: 120px;
  padding: 5px;

}

#requesttype{
  position: relative;
  left: 8px;
  padding: 5px;
}

</style>
