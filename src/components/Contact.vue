<template>
  <form class="contact-form" @submit.prevent="sendEmail">
    <div class="form-group">
      <label for="msg-name">Name</label>
      <input type="text" id="msg-name" class="form-control" v-model="contactFormData.name" />
    </div>
    <div class="form-group">
      <label for="msg-email">Email</label>
      <input type="text" id="msg-email" class="form-control" v-model="contactFormData.email" />
    </div>
    <div class="form-group">
      <label for="msg-message">Message</label>
      <textarea
        rows="10"
        id="msg-message"
        class="form-control"
        v-model="contactFormData.message"
      ></textarea>
    </div>
    <div class="form-result">
      <p class="alert alert-success" v-if="success && !error">Message sent successfully.</p>
      <p class="alert alert-error" v-if="!success && error">Message failed.</p>
    </div>
    <div class="form-group">
      <button class="btn" type="submit">Send</button>
    </div>
  </form>
</template>

<script>
import emailjs from "emailjs-com";
import { serviceID, userID } from "../../Scripts/config";
export default {
  name: "ContactForm",
  data: function() {
    return {
      contactFormData: {
        name: "",
        email: "",
        message: ""
      },
      success: false,
      error: false
    };
  },
  methods: {
    sendEmail() {
      emailjs
        .send(serviceID, "template_opza1mc", this.contactFormData, userID)
        .then(
          result => {
            this.success = true;
            this.resetForm();
            console.log("SUCCESS!", result.text);
          },
          error => {
            this.error = true;
            this.error = console.log("FAILED...", error.text);
          }
        )
        .then(
          setTimeout(() => {
            this.success = false;
            this.error = false;
          }, 5000)
        );
    },
    resetForm() {
      this.contactFormData = {
        name: "",
        email: "",
        message: ""
      };
    }
  }
};
</script>

<style scoped>
.contact-form {
  padding: 20px;
  border-radius: 4px;
  width: 75%;
  margin-left: 12.5%;
  color: rgb(240, 240, 240);
}
.form-group {
  padding: 10px;
}
.form-control {
  width: 100%;
  padding: 12px 15px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
input[type="text"].form-control {
  margin: 8px 0;
  display: inline-block;
}
textarea.form-control {
  resize: none;
}
.btn {
  cursor: pointer;
  padding: 8px 10px;
  outline: none;
  border: none;
  background: #232741;
  font-size: 16px;
  width: 100%;
  border-radius: 4px;
  text-align: center;
  color: rgb(240, 240, 240);
}
.alert {
  padding: 10px;
  margin-right: 10px;
  margin-left: 10px;
}
.alert-success {
  color: #3aa843;
}
.alert-error {
  color: #ff2121;
}
</style>
