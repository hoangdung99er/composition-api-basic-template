<template>
  <section class="container">
    <user-data :last-name="lastName" :first-name="firstName"></user-data>
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>
    <button @click="setAge">Change Age</button>
    <div>
      <!-- <input type="text" placeholder="First Name" @input="setFirstName" />
      <input type="text" placeholder="Last Name" @input="setLastName($event)" /> -->

      <!-- <input type="text" placeholder="First Name" v-model="firstName" />
      <input type="text" placeholder="Last Name" v-model="lastName" /> -->

      <input type="text" placeholder="First Name" ref="firstNameInput" />
      <input type="text" placeholder="Last Name" ref="lastNameInput" />
      <button @click="setFullName">Set Full Name</button>
    </div>
  </section>
</template>

<script>
import { reactive, ref, computed, watch, toRefs, provide } from 'vue';
import UserData from './components/UserData.vue';
export default {
  components: {
    UserData,
  },
  setup() {
    // "ref" worked with any kind of value (string , numb, objects)
    // const uName = ref('Hoang Dung');

    // "reactive" made for Object assign, only worked with Object
    const user = reactive({
      name: 'Hoang Dung',
      age: 23,
    });

    const firstName = ref('');
    const lastName = ref('');

    const uAge = ref(33);

    provide('userAgeProvider', uAge);

    const firstNameInput = ref(null);
    const lastNameInput = ref(null);

    const usersRef = toRefs(user);

    const uName = computed(function () {
      return firstName.value + ' ' + lastName.value;
    });

    // whenever user.age changes, this execute the function
    // Watch can be only be a getter function, a ref, a reactive object or an array
    watch([usersRef.age, uName], function (newValues, oldValues) {
      console.log('Old age :', oldValues[0]);
      console.log('New age : ', newValues[0]);
    });

    // setTimeout(function () {
    //   uName.value = 'Ngoc Trinh';
    // }, 2000);

    /*
     * user.value.name (ref)
     * user.value.age (ref)
     **/

    // console.log(isRef(uName.value));
    // console.log(isReactive(user.name), user.age);

    // setTimeout(function () {
    //   user.name = 'Ngoc Trinh';
    //   user.age = 33;
    // }, 2000);

    function setNewAge() {
      user.age = 33;
    }

    function setFullName() {
      firstName.value = firstNameInput.value.value;
      lastName.value = lastNameInput.value.value;
    }

    function setFirstName(event) {
      firstName.value = event.target.value;
    }

    function setLastName(event) {
      lastName.value = event.target.value;
    }

    // return { userName: uName, user };
    // return { userName: usersRef.name, age: usersRef.age };
    return {
      userName: uName,
      user,
      setAge: setNewAge,
      setFirstName,
      setLastName,
      firstName,
      lastName,
      firstNameInput,
      lastNameInput,
      setFullName,
    };
  },
  // data() {
  //   return {
  //     userName: 'Maximilian',
  //   };
  // },
  // provide() {
  //   return {
  //     age : age
  //   }
  // }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

.container {
  margin: 3rem auto;
  max-width: 30rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  text-align: center;
}
</style>
