<script setup>

</script>
<template>
    <div class="input-group">
      <label v-if="label" :class="label.class">{{
        typeof label == "string" ? label : label.text
      }}</label>
      <span v-if="icon" class="input-group-text"
        ><i class="fas" :class="`fa-${icon}`" aria-hidden="true"></i
      ></span>
      <select class="form-select" aria-label="Default select example">
        <option selected>Open this select menu</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
    </div>
  </template>