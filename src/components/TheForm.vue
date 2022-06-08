<!-- GETTING DATA FROM FORM INPUTS -->

<!-- TEXT INPUTS / NUMBER INPUTS
=================================================================

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
==================================================================

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
================================================================

      -v-model can also be used the same way to get input from a dropdown just like text-inputs

  e.g.  <select id="referrer" name="referrer" v-model="referrer">
              <option value="google">Google</option>
              <option value="wom">Word of mouth</option>
              <option value="newspaper">Newspaper</option>

      -Values from the select options will be then stored in the referrer data property whenever we change a value

      -We can initialize our referrer data property with one value from the select options values and select our favorite starting value

-->

<!-- CHECKBOXES / RADIOBUTTONS 
=================================================================

      -v-model can also be used the same way to get selected values from a checkbox/radio buttons input 

      -Names for checkboxes / radio buttons needs to match but with different values
      -id of the inputs needs to match with the value provided in the for="" label attribute

      -GOTCHAS WITH CHECKBOXES 
      -------------------------------

      -If you have multiple checkboxes with the same name like we do here , this automatically creates a group of thos checkboxes

      -That means we need to handle it differently by creating an empty [] as a starting value and Vue will then add all the checked items into this []

      -However , it still behaves strangely because we need to make one adjustment here;
      
      - And that is by adding different values to these checkboxes ; Otherwise Vue does not know which values to add into the empty []

  
-->

<!-- SINGLE CHECK - PROBABLY FOR ACCEPTING TERMS AND CONDITIONS
=================================================================

      -So how can we get the value of a single checkbox since its not a group but just a single value 

      -We can add confirm as a data-property and set it to false initially ; Then bind it with v-model

      -In summary , if you have a group of checkboxes which share the same name to yield value , then you get an array of the selected checkboxes - values IS A MUST attribute

      In case of a single checkbox you get true / false values
  
-->

<!-- ADDING A BASIC FORM VALIDATION
==========================================================================================================================================================================

-We can validate input values on every keystrokes or whenever the input blurs i.e When the input loses focus

-We can do this by adding @blur event and then check whether the input entered is valid and if not shiw the user an error msg;

  e.g. <input id="user-name" name="user-name" type="text" v-model="username" @blur=""/>

  -We can  add a span that will be rendered conditionally if usernameValidity is invalid
    
    e.g. <span v-if="usernameValidity === 'invalid'">Please enter a valid name!</span>

  -We can also add a class dynamically for  styling the div for the same condition
  
  .e.g.   <div class="form-control" :class="{ invalid: usernameValidity === 'invalid' }">
  
-->

<!--BUILDING A CUSTOM CONTOL COMPONENT
=========================================================================================

- Added RatingControl component with three buttons to allow a user to select a rating

-The idea is allow a user to select a rating which should be highlighted and then saved as part of the overall form

-N/B : Added behaviour of clicking and selecting active button in the parent component (Rating Control)

-BUt how do select the value of the button that was Clicked ????????????

-Well it turns out that we can use v-model on a component and then simply bind the value that this control has internally to some data property that we store here;

-Add rating data property and set it to null ; Then bind it with v-model on the rating control component

-Leak up => v-model is a shortcut  that combines @input="" and :value="" and that is something we can leverage

-Vue will set a very specific prop if you use v-model on a component and it will listen to a very specific event which you can emit in that component.

-Now which prop/event is that; 'Let's go to rating control and receive that prop;

-THEREFORE  USING V-MODEL ON A CUSTOM COMPONENT IS LIKE MANUALLY BINDING THE :model-value="" prop like this.
    .eg.
    <rating-control v-model="rating" :model-value="" ></rating-control>

  -THAT IS ONE THING V-MODEL DOES UNDER THE HOOD ON CUSTOM COMPONENTS 


-THEREFORE  USING V-MODEL ON A CUSTOM COMPONENT IS LIKE MANUALLY BINDING THE :model-value="" and listening to @update:modelValue="" event

.e.g.   <rating-control v-model="rating" :model-value="" @update:modelValue="" ></rating-control>

-If use v-model="rating" is the same as like doing the 2 (  :model-value="" @update:modelValue="" )

-BUT THERE IS A PROBLEM WHEN WE IT COMES TO RESETTING

-The reset is not in in sync with the value stored in rating here

Therefore go to RatingControl and we can can work around that problem - continue.. in RatingControl



-->

<template>
  <form @submit.prevent="submitForm">
    <div class="form-control" :class="{ invalid: usernameValidity === 'invalid' }">
      <label for="user-name">Your Name</label>
      <input
        id="user-name"
        name="user-name"
        type="text"
        v-model.trim="username"
        @blur="validateName"
      />
      <p v-if="usernameValidity === 'invalid'">Please enter a valid name!</p>
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
        <input
          id="interest-news"
          name="interest"
          type="checkbox"
          v-model="interest"
          value="news"
        />
        <label for="interest-news">News</label>
      </div>

      <div>
        <input
          id="interest-tutorials"
          name="interest"
          type="checkbox"
          v-model="interest"
          value="tutorials"
        />
        <label for="interest-tutorials">Tutorials</label>
      </div>

      <div>
        <input
          id="interest-nothing"
          name="interest"
          type="checkbox"
          v-model="interest"
          value="nothing"
        />
        <label for="interest-nothing">Nothing</label>
      </div>
    </div>

    <div class="form-control">
      <h2>How do you learn?</h2>
      <div>
        <input id="how-video" name="how" type="radio" v-model="how" value="video" />
        <label for="how-video">Video Courses</label>
      </div>
      <div>
        <input id="how-blogs" name="how" type="radio" v-model="how" value="blogs" />
        <label for="how-blogs">Blogs</label>
      </div>
      <div>
        <input id="how-other" name="how" type="radio" v-model="how" value="other" />
        <label for="how-other">Other</label>
      </div>
    </div>

    <div class="form-control">
      <input type="checkbox" name="terms" id="terms" v-model="confirm" />
      <label for="terms"> Agree to our Terms of use</label>
    </div>

    <div class="form-control">
      <rating-control v-model="rating"></rating-control>
    </div>

    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
import RatingControl from "./RatingControl.vue";

export default {
  components: {
    RatingControl,
  },
  data() {
    return {
      username: "",
      userage: null,
      referrer: "google",
      interest: [],
      how: null,
      confirm: false,
      usernameValidity: "pending",
      rating: null,
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

      console.log("CheckBoxes");
      console.log(this.interest);

      console.log("Radio Buttons");
      console.log(this.how);

      // Reset to null
      this.interest = [];
      this.how = null;

      console.log("Confirm ..?");
      console.log(this.confirm);

      this.confirm = false;

      console.log("Rating : ");
      console.log(this.rating);

      this.rating = null;
    },
    validateName() {
      if (this.username === "") {
        this.usernameValidity = "invalid";
      } else {
        this.usernameValidity = "valid";
      }
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
