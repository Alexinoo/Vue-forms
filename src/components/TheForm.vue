<!-- GETTING DATA FROM FORM INPUTS -->

<!-- TEXT INPUTS / NUMBER INPUTS
- There 2 main ways we can use to get data from the inputs

-OPTION 1

     1. )  v-model
            - uses two way binding - You cannot just listen to what the user entered but you can also overwrite what is in the form and you can change the value stored in the input 


     2.) @input 
            - use custom event listener that listens to each keystroke  and store what the user entered in some data property
            -Useful if you want to validate input on every keystrokes before the user event tries to submit anything

      3.) ref=""

            -ref is an inbuilt Vue property that we can add to text inputs as an attribute
              e.g  <input id="age" name="age" type="number" ref="ageInput" />

            -We can then use $refs to access the native DOM element and we can retrieve the value using the following syntax: this.$refs.ageInput.value
         
-->

<!-- NUMBER INPUTS 

-We can use the v-model and ref to get inputs from number inputs from the Form but they have different behaviors;

        1.) v-model ="userage"

              -When used on a number input; v-model automatically converts it from a STRING to a NUMBER datatype

              -We can also force it to do that by adding number modifier to the v-model directive e.g    v-model.number="userage"

              THERE ARE A QUITE A NUMBER OF V-MODEL MODIFIERS AVAILBALE TO US FROM VUE

                i) v-model.lazy =>Allows you to change how Vue updates the bound property - at a lower frequency

                 ii) v-model.trim =>  Tells Vue to remove the trailing /leading white spaces and at the end if any


        2.) ref="ageInput"

          -usage :  <input id="age" name="age" type="number" ref="ageInput" />

          -we get the value by using : this.$refs.ageInput.value

           -In most cases this returns a STRING and therefore we need to be very careful if what we are expecting is of type NUMBER ( Need to do some conversions to ) using Number()
-->

<!-- DROPDOWNS  

      -v-model can also be used the same way to get input from a dropdown just like text-inputs

  e.g.  <select id="referrer" name="referrer" v-model="referrer">
              <option value="google">Google</option>
              <option value="wom">Word of mouth</option>
              <option value="newspaper">Newspaper</option>

      -Values from the select options will be then stored in the referrer data property whenever we change a value

      -We can initialize our referrer data property with one value from the select options values and select our favorite starting value

-->

<template>
  <form @submit.prevent="submitForm">
    <div class="form-control">
      <label for="user-name">Your Name</label>
      <input id="user-name" name="user-name" type="text" v-model="username" />
    </div>

    <div class="form-control">
      <label for="age">Your Age (Years)</label>
      <input id="age" name="age" type="number" ref="userage" />
    </div>

    <div class="form-control">
      <label for="referrer">How did you hear about us?</label>
      <select id="referrer" name="referrer" v-model="referrer">
        <option value="google">Google</option>
        <option value="wom">Word of mouth</option>
        <option value="newspaper">Newspaper</option>
      </select>
    </div>

    <div class="form-control">
      <h2>What are you interested in?</h2>

      <div>
        <input id="interest-news" name="interest" type="checkbox" />
        <label for="interest-news">News</label>
      </div>

      <div>
        <input id="interest-tutorials" name="interest" type="checkbox" />
        <label for="interest-tutorials">Tutorials</label>
      </div>

      <div>
        <input id="interest-nothing" name="interest" type="checkbox" />
        <label for="interest-nothing">Nothing</label>
      </div>
    </div>

    <div class="form-control">
      <h2>How do you learn?</h2>
      <div>
        <input id="how-video" name="how" type="radio" />
        <label for="how-video">Video Courses</label>
      </div>
      <div>
        <input id="how-blogs" name="how" type="radio" />
        <label for="how-blogs">Blogs</label>
      </div>
      <div>
        <input id="how-other" name="how" type="radio" />
        <label for="how-other">Other</label>
      </div>
    </div>
    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      userage: null,
      referrer: "google",
    };
  },
  methods: {
    submitForm() {
      console.log("Username : " + this.username);
      this.username = "";

      console.log(30);

      console.log("User age : ");
      console.log(this.$refs.userage.value); //Returns a STRING
      console.log(Number(this.$refs.userage.value)); //Returns a NUMBER
      this.userage = null;

      console.log("Referrer : " + this.referrer);
      this.referrer = "google"; //Then reset it back to google
    },
  },
};
</script>

<style scoped>
form {
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}

.form-control {
  margin: 0.5rem 0;
}

.form-control.invalid input {
  border-color: red;
}
.form-control.invalid label {
  color: red;
}

label {
  font-weight: bold;
}

h2 {
  font-size: 1rem;
  margin: 0.5rem 0;
}

input,
select {
  display: block;
  width: 100%;
  font: inherit;
  margin-top: 0.5rem;
}

select {
  width: auto;
}

input[type="checkbox"],
input[type="radio"] {
  display: inline-block;
  width: auto;
  margin-right: 1rem;
}

input[type="checkbox"] + label,
input[type="radio"] + label {
  font-weight: normal;
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}
</style>
