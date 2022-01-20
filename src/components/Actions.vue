<template>
  <div class="actions">
    <button 
      class="create shadow"
      :class="{ 'disabled': disabledCreate}"
      :disabled="disabledCreate"
      @click="$emit('create')"
    >
      Create
    </button>

    <button 
      class="update shadow"
      :class="{ 'disabled': noneIsSelected }"
      :disabled="disabledUpdate"
      @click="$emit('update')"
    >
      Update
    </button>

    <button 
      class="delete shadow"
      :class="{ 'disabled': noneIsSelected }"
      :disabled="noneIsSelected"
      @click="$emit('remove')"
    >
      Delete
    </button>
  </div>
</template>

<script>
  export default {
    name: 'Actions',

    props: {
      noneIsSelected: Boolean,
      disabledCreate: Boolean,
      disabledUpdate: Boolean
    },
  }
</script>

<style>
 
  .actions button {
    cursor: pointer;
    font-size: 16px;
    font-weight: 500;
    margin-right: 10px;
    border-radius: 7px;
    border: none;
    padding: 10px 20px;
  }

  .disabled {
    opacity: .6;
    cursor: auto;
  }

  .create {
    background: #6FE7B7;
  }

  .update {
    background: #FCE789;
  }

  .delete {
    background: #FBA5A6;
  }
  
</style>
