<template>
  <div class="form-group">
    <div class="d-flex justify-content-between align-items-center">
      <label>{{label}}</label>
      <button class="btn align-items-center" type=button @click="addRecipient()">
        <i class="fas fa-plus-circle"></i>
      </button>
    </div>
    <div class="input-group mt-2" v-for="(recipient, index) in recipients" :key="index">
      <input class="form-control input-group-prepend" placeholder="Email*" required v-model="recipient.email" type="email"/>
      <input class="form-control" placeholder="Name" v-model="recipient.name"/>
      <button class="input-group-append btn align-items-center" type=button @click="removeRecipient(index)" :disabled="recipients.length <= min">
        <i class="fas fa-trash"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EmailRecipients',
  props: {
    label: String,
    recipients: Array,
    min: {
        type: Number,
        default: 0
    },
  },
  methods: {
      addRecipient: function() {
          this.$emit('addRecipient');
      },
      removeRecipient: function(index) {
          this.$emit('removeRecipient', index);
      },
  },
}
</script>
