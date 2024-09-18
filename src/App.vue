<script setup>

import {ref} from 'vue'
import {firebaseApp} from "@/firebase";

import {
  getAuth,
  signInWithEmailAndPassword,
} from "firebase/auth";


// ✅ 로그인
const auth = getAuth(firebaseApp);

const loginInfo = ref({
  txtEmail: '',
  txtPassword: '',
});

const loginEmailPassword = async () => {

  const loginEmail = loginInfo.value.txtEmail;
  const loginPassword = loginInfo.value.txtPassword;

  try {
    const userCredential = await signInWithEmailAndPassword(
        auth,
        loginEmail,
        loginPassword
    );

    console.log(userCredential.user);
  } catch (error) {
    if(error.code === "auth/invalid-email")
      console.log("잘못된 이메일 입니다.")
    else if (error.code === "auth/invalid-credential")
      console.log("비밀번호가 틀렸습니다.")
    else if (error.code === "auth/missing-password")
      console.log("비밀번호를 입력해주세요.")

  }
}


// const BASEURL = 'https://port-0-java-springboottest-m0ce3xjwfbac249f.sel4.cloudtype.app'
const BASEURL = 'http://java-springboottest:8080'
// const BASEURL = 'http://localhost:8080'


const count = ref(0);
const rawHtml = "<h1>Message: {{ count }}</h1>"
const h1Id = ref("hi")
const name = ref("")

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
const postUserName = async () => {
  await fetch(`${BASEURL}/user`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({name: name.value})
  });
  testData.value = `${name.value} "입력 성공"`;
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


    <button @click="postUserName">Post User</button>
    <input v-model="name" placeholder="추가할 이름을 입력하세요"/>


    <h1>로그인 테스트</h1>
    <input v-model="loginInfo.txtEmail" placeholder="이메일 입력해주세요"/><br>
    <input v-model="loginInfo.txtPassword" placeholder="패스워드 입력해주세요."/>
    <button @click="loginEmailPassword">로그인</button>

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
