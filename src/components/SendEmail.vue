<template>
  <div class="container">
    <div class="d-flex justify-content-between">
      <h3>Send Email</h3>
      <select class="form-control" v-model="url" style="width: 50%; max-width: 150px">
        <option v-for="(server, index) in servers" :key="index" :label="server.name" :value="server.url"></option>
      </select>
    </div>
    <form class="form" v-if="formData">
      <div class="form-group">
        <label>Subject*</label>
        <input class="form-control" v-model="formData.subject" />
      </div>

      <EmailRecipients :recipients="formData.toEmails" label="To*" :min="1" @addRecipient="addEmail('toEmails')" @removeRecipient="removeEmail('toEmails', $event)"/>
      <EmailRecipients :recipients="formData.ccEmails" label="Cc" @addRecipient="addEmail('ccEmails')" @removeRecipient="removeEmail('ccEmails', $event)"/>
      <EmailRecipients :recipients="formData.bccEmails" label="Bcc" @addRecipient="addEmail('bccEmails')" @removeRecipient="removeEmail('bccEmails', $event)"/>

      <div class="form-group">
        <label>Body*</label>
        <textarea class="form-control" v-model="formData.body"></textarea>
      </div>

      <button class="btn btn-primary align-items-center" type=button @click="submit()" :disabled="sending">
        <i class="far fa-save"></i> {{sending ? 'Sending...' : 'Send'}}
      </button>
      <span class="ml-2">{{apiResult}}</span>
    </form>
  </div>
</template>
<script>
    import EmailRecipients from "./EmailRecipients";
    import axios from "axios";

    const Servers = [
        { name: 'Test', url: 'http://34.116.123.6/api/v1' },
        { name: 'Local', url: 'http://localhost:3000/api/v1' }
    ];

    export default {
        name: 'SendEmail',
        components: {
            EmailRecipients
        },
        data: () => {
            return {
                apiResult: '',
                sending: false,
                url: Servers[0].url,
                servers: Servers,
                formData: {
                    subject: '',
                    body: '',
                    toEmails: [{ email: '' }],
                    ccEmails: [],
                    bccEmails: []
                },
            };
        },
        methods: {
            addEmail: function(type) {
                this.formData[type].push({ email: '' });
            },
            removeEmail: function(type, index) {
                this.formData[type].splice(index, 1);
            },
            submit: function() {
                this.sending = true;
                axios
                    .post(`${this.url}/email/send`, this.formData)
                    .then(() => {
                        this.apiResult = 'Success';
                    })
                    .catch(() => {
                        this.apiResult = 'Failure';
                    })
                    .finally(() => {
                        this.sending = false;
                        const timeoutRef = setTimeout(() => {
                            this.apiResult = '';
                            clearTimeout(timeoutRef);
                        }, 1000);
                    })
            },
        }
    }
</script>
<style>
  h3 {
    margin-bottom: 5%;
  }
  .btn:focus {
    outline: none;
    box-shadow: none;
  }
</style>
