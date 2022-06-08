<!-- Allows a user to pick a rating

-Added activeOptions and set it to null initially

-Add a @click event and update the activeOptions selected  
    i.e if the user clicks Poor button the activeOptions is set to poor and some dynamic styles applied

 --   BUILDING A CUSTOM CONTOL COMPONENT --

 As said , v-model on this component  <rating-control v-model="rating" ></rating-control> will set a very specific prop which we can receive here by adding props : []

 -And that prop will be modelValue and thus we can receive it as follows : props : ['modelValue']


  -THE OTHER THING IS THE SPECIAL EVENT THAT IS EMITTED WHICH IS update:modelValue

  -THEREFORE WE NEED TO EMIT IT IN THE activate() and forward the option and then  initialize our activeOption with the modelValue we get on the prop

  -So that the form component is able to set our initial value directly
    
    e.g. 
   
    activeOptions: this.modelValue,

    -Seems the reseting of rating is not in sync and instead of using data property to initialize

    -We can comment out on data(){} - which i have commented out      

  -And then simply use a computed property as follows : 

   computed: {
    activeOptions() {
      return this.modelValue;
    },
  },

  OR JUST COMMENT OUT 

//    computed: {
//     activeOptions() {
//       return this.modelValue;
//     },
//   },

-AND THEN REPLACE WITH THE PROP VALUE WE ARE GETTING FROM TheForm component

 e.g.  - <li :class="{ active: modelValue === 'poor' }">
         - <li :class="{ active: modelValue === 'average' }">
         - <li :class="{ active: modelValue === 'great' }">

-AND THEREFORE REMOVE  this.activeOptions = option; FROM THE acivate() method

-AN NOW THIS WORKS I.E IS IN SYNC WITH THE RESET


-->

<template>
  <ul>
    <li :class="{ active: modelValue === 'poor' }">
      <button type="button" @click="activate('poor')">Poor</button>
    </li>
    <li :class="{ active: modelValue === 'average' }">
      <button type="button" @click="activate('average')">Average</button>
    </li>
    <li :class="{ active: modelValue === 'great' }">
      <button type="button" @click="activate('great')">Great</button>
    </li>
  </ul>
</template>

<script>
export default {
  props: ["modelValue"],

  emits: ["update:modelValue"],

  //   data() {
  //     return {
  //       activeOptions: this.modelValue,
  //     };
  //   },
  //   computed: {
  //     activeOptions() {
  //       return this.modelValue;
  //     },
  //   },

  methods: {
    activate(option) {
      this.$emit("update:modelValue", option);
    },
  },
};
</script>

<style scoped>
.active {
  border-color: purple;
}
.active button {
  border-color: purple;
}

ul {
  list-style: none;
  margin: 0.5rem 0;
  padding: 0;
  display: flex;
}

li {
  margin: 0 1rem;
  border: 1px solid #ccc;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-content: center;
}

button {
  font: inherit;
  border: none;
  background-color: transparent;
  cursor: pointer;
}
</style>
