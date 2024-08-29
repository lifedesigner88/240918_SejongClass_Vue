<script setup>

import {ref} from 'vue'

const BASEURL = 'https://port-0-java-springboottest-m0ce3xjwfbac249f.sel4.cloudtype.app'
// const BASEURL = 'http://localhost:8080'


const count = ref(0);
const rawHtml = "<h1>Message: {{ count }}</h1>"
const h1Id = ref("hi")

const changeh1Id = () => {
  if (h1Id.value === "hi")
    h1Id.value = "hi22"
  else
    h1Id.value = "hi";
}

const testData = ref("")

const getUsers = async () => {
  const response = await fetch(`${BASEURL}/user`, {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json'
    }
  });
  testData.value = await response.json();
  console.log(testData.value);
}


</script>

<template>

  <div id="app">
    <button @click="count++">
      Count is: {{ count }}
    </button>
    <button @click="changeh1Id"> Change Color</button>

    <br>
    <h1>Message: {{ count }}</h1>
    <span v-html="rawHtml"/>
    <h1 v-bind:id="h1Id">1234</h1>

    <button @click="getUsers">Get User</button>
    <h1>{{ testData }}</h1>
  </div>

</template>

<style scoped>
button {
  font-weight: bold;
}

#hi {
  color: orange;
}

#hi22 {
  color: blue;
}
</style>
