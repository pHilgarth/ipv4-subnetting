
<template>
  <section class="app-container">
    <h1>IPv4-Adressierung und -Subnetting</h1>
    <main class="container">
      <p style="color: red; font-size: 1.5rem;">
        Macbook-Test - If an address wasn't subnetted - what's the first and next subnet?
        Here's an error - first subnet is calculated wrong run some tests again
        for special ip addresses (not subnetted, subnetted from 8 to 16 or 24
        ....)
      </p>
      <p style="color: red; font-size:1.5rem">
        replace plain js in checkAnwers()
      </p>
      <p style="color: red; font-size:1.5rem">error when non-subnetted addres is generated (recursive call? )</p>

      <the-address @address-generated="activateForm"></the-address>
      <question-form v-if="formVisible" :answers="answers" :subnetted="subnetted"></question-form>
    </main>
  </section>
</template>

<script>
import QuestionForm from "./components/QuestionForm";
import TheAddress from "./components/TheAddress";

export default {
  name: "App",
  components: {
    QuestionForm,
    TheAddress,
  },

  data: function () {
    return {
      answers: {},
      feedbackText: "",
      formVisible: false,
      subnetted: false,
      userInput: {
        addressClass: "",
        addressType: "",
        dsnm: "",
        nsnm: "",
        borrowedBits: "",
        hostBits: "",
        network: "",
        firstHost: "",
        lastHost: "",
        broadcast: "",
        firstSubnet: "",
        nextSubnet: "",
        lastSubnet: "",
        amountSubnets: "",
        amountHosts: "",
      },
    };
  },
  methods: {
    checkAnswers(event) {
      event.preventDefault();

      for (let prop in this.userInput) {
        let selector = "form .form-group #" + prop + " + .feedback";

        let feedbackElement = document.querySelector(selector);

        feedbackElement.classList.remove("hidden");

        if (this.userInput[prop] === this.answers[prop]) {
          feedbackElement.textContent = "Perfect, correct answer!";
          feedbackElement.classList.add("correct-answer");
          feedbackElement.classList.remove("wrong-answer");
        } else {
          feedbackElement.textContent = "Sorry, wrong answer!";
          feedbackElement.classList.add("wrong-answer");
          feedbackElement.classList.remove("correct-answer");
        }
      }
    },

    activateForm(answers, subnetted) {
      this.formVisible = true;
      this.answers = answers;
      this.subnetted = subnetted;
    }
  }
};
</script>
