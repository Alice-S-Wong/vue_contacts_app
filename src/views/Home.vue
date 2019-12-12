<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>All Contacts</h1>
    <p>First Name: <input type="text" v-model="newFirstName"></p>
    <p>Middle Name: <input type="text" v-model="newMiddleName"></p>
    <p>Last Name: <input type="text" v-model="newLastName"></p>
    <p>Email: <input type="text" v-model="newEmail"></p>
    <p>Phone Number: <input type="text" v-model="newPhoneNumber"></p>
    <p>Bio: <input type="text" v-model="newBio"></p>
    <button v-on:click="createContact()">Create Contact</button>
    <div v-for="contact in contacts">
      <p>ID: {{contact.id}}</p>
      <p>First Name: {{contact.first_name}}</p>
      <p>Middle Name: {{contact.middle_name}}</p>
      <p>Last Name: {{contact.last_name}}</p>
      <button v-on:click="toggleInfo(contact)">Show Info</button>
      <div v-if="currentContact === contact">
        <p>Email: {{contact.email}}</p>
        <p>Phone Number: {{contact.phone_number}}</p>
        <p>Bio: {{contact.bio}}</p>
        <p>Update Contact</p>
        <p>First Name: <input type="text" v-model="contact.first_name"></p>
        <p>Middle Name: <input type="text" v-model="contact.middle_name"></p>
        <p>Last Name: <input type="text" v-model="contact.last_name"></p>
        <p>Email: <input type="text" v-model="contact.email"></p>
        <p>Phone Number: <input type="text" v-model="contact.phone_number"></p>
        <p>Bio: <input type="text" v-model="contact.bio"></p>
        <button v-on:click="updateContact(contact)">Update Contact</button>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newFirstName: "",
      newMiddleName: "",
      newLastName: "",
      newEmail: "",
      newPhoneNumber: "",
      newBio: "",
      currentContact: {}
    };
  },
  created: function() {
    axios.get("api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newFirstName,
        middle_name: this.newMiddleName,
        last_name: this.newLastName,
        email: this.newEmail,
        phone_number: this.newPhoneNumber,
        bio: this.newBio
      };
      console.log(params);
      axios.post("api/contacts", params).then(response => {
        this.contacts.push(response.data);
        this.newFirstName = "";
        this.newMiddleName = "";
        this.newLastName = "";
        this.newEmail = "";
        this.newPhoneNumber = "";
        this.newBio = "";
      });
    },
    toggleInfo: function(theContact) {
      if (this.currentContact === theContact) {
        this.currentContact = null;
      } else {
        this.currentContact = theContact;
      }
    },
    updateContact: function(theContact) {
      var params = {
        first_name: theContact.first_name,
        middle_name: theContact.middle_name,
        last_name: theContact.last_name,
        email: theContact.email,
        phone_number: theContact.phone_number,
        bio: theContact.bio
      };
      console.log(params);
      axios.patch(`/api/contacts/${theContact.id}`, params).then(response => {
        console.log(response.data);
      });
    }
  }
};
</script>