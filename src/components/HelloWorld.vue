<template>
  <div class="hello">
    {{msg}}
    <form @submit.prevent="add_contact">
      <input v-model="name" type="text" placeholder="name">
      <phone-mask-input
        ref="phone"
        v-model="phone"
        autoDetectCountry
        showFlag
        @onValidate="onValidate"
        wrapperClass="wrraper-example"
        inputClass="input-phone"
        flagClass="flag-example"
      />

      <button >add</button>
    </form>
    <h2>new contact</h2>
    <div>
      {{name}} : {{phone}} {{phone | phone_format}}
    </div>
    <h2>contacts list</h2>
    <p> Кількість контактів - {{contacts.length}}</p>
    <ul v-if="contacts.length>0">
      <li 
        v-for="item, index in contacts" 
        :class="item['status']"
        >
        {{item.name}}
        <a :href="'tel:'+item.phone"> {{item.phone | phone_format}} </a> 
        <i @click="remove_item(index)" class="fas fa-user-times"></i>
        <select @change="set_status(index, $event.target.value)">
          <option v-for="status in statuts" :value="status">
            {{status}}
          </option>
        </select>
      </li>

    </ul>
    <p> Кількість контактів - {{contacts.length}}</p>
  </div>
</template>

<script>
import PhoneMaskInput from  "vue-phone-mask-input";
export default {
  name: 'HelloWorld',
  components:{
    PhoneMaskInput
  },
  props: {
    msg: String
  },
  data: function(){
    return {
      name: '',
      phone: '',
      contacts: [],
      statuts: [null,'friend', 'work']
    }
  },
  filters: {
    phone_format: function(value){
      var re = /\(?(\d{3})\)?[- ]?(\d{2})[- ]?(\d{3})[- ]?(\d{2})[- ]?(\d{2})/g; 
      var subst = '$1 $2-$3-$4-$5'; 

      return value.replace(re, subst); 
    }
  },
  methods:{
    onValidate: function(data){
      console.log(data)
    },
    add_contact: function(){
      if(this.name.length < 2){
        alert('name is required')
        return false;
      }
      let contact = {
        name: this.name,
        phone: this.phone
      }
      this.contacts.push(contact);
      // this.contacts = this.contacts.concat(contact)
      this.name = "";
      this.phone = "";
      document.querySelector('.input-phone').value = '';
    },
    remove_item: function(index){
      this.contacts.splice(index, 1);
    },
    set_status: function(index, status){
      this.contacts[index]['status']= status
      this.$set(this.contacts, index, this.contacts[index]);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.friend{
  color: green
}
.work{
  color: orange
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
