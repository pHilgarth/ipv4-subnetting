<template>
  <section class="questions">
    <h2 :class="{ testing: !subnetted }">
      subnetted-{{ subnetted }}---answers-{{ answers }}
    </h2>
    <form>
      <form-group
        v-for="field in requiredFormFields"
        :key="field.name"
        :name="field.name"
        :text="field.text"
        @emit-value="updateValue"
      ></form-group>
      <button class="check-form button" @click="checkAnswers($event)">
        Check
      </button>
    </form>
  </section>
</template>

<script>
import FormGroup from "./FormGroup";

export default {
  components: {
    FormGroup,
  },

  beforeCreate() {
    console.log("beforeCreated");
  },

  created() {
    console.log("created");
  },
  beforeMount() {
    console.log("beforeMount");
  },
  mounted() {
    console.log("mounted");
    if (this.subnetted) {
      this.requiredFormFields = this.allFormFields;
    } else {
      this.requiredFormFields = this.allFormFields.filter(
        (field) => field.showIfNotSubnetted
      );
    }
  },
  beforeUpdate() {
    console.log("beforeUpdate");
  },
  updated() {
    console.log("updated");

    if (this.subnetted) {
      this.requiredFormFields = this.allFormFields;
    } else {
      this.requiredFormFields = this.allFormFields.filter(
        (field) => field.showIfNotSubnetted
      );
    }
    
    
  },
  beforeUnmount() {
    console.log("beforeUnmount");
  },
  unmounted() {
    console.log("unmounted");
  },

  data() {
    return {
      allFormFields: [
        {
          name: "addressClass",
          text: "Class:",
          showIfNotSubnetted: true,
        },
        {
          name: "addressType",
          text: "Type (Public or Private):",
          showIfNotSubnetted: true,
        },
        {
          name: "dsnm",
          text: "Default Subnet Mask:",
          showIfNotSubnetted: true,
        },
        {
          name: "nsnm",
          text: "New Subnet Mask:",
          showIfNotSubnetted: true,
        },
        {
          name: "borrowedBits",
          text: "Borrowd Bits:",
          showIfNotSubnetted: true,
        },
        {
          name: "hostBits",
          text: "Host Bits:",
          showIfNotSubnetted: true,
        },
        {
          name: "network",
          text: "Network Address:",
          showIfNotSubnetted: true,
        },
        {
          name: "firstHost",
          text: "First Host:",
          showIfNotSubnetted: true,
        },
        {
          name: "lastHost",
          text: "Last Host:",
          showIfNotSubnetted: true,
        },
        {
          name: "broadcast",
          text: "Broadcast Address",
          showIfNotSubnetted: true,
        },
        {
          name: "firstSubnet",
          text: "First Subnet:",
          showIfNotSubnetted: false,
        },
        {
          name: "nextSubnet",
          text: "Next Subnet:",
          showIfNotSubnetted: false,
        },
        {
          name: "lastSubnet",
          text: "Last Subnet:",
          showIfNotSubnetted: false,
        },
        {
          name: "amountSubnets",
          text: "Amount of Subnets",
          showIfNotSubnetted: false,
        },
        {
          name: "amountHosts",
          text: "Amount of Hosts:",
          showIfNotSubnetted: false,
        },
      ],

      requiredFormFields: [],

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
    updateValue(name, value) {
      this.userInput[name] = value;
    },
  },

  //   updated() {
  //       if (this.subnetted) {
  //           this.requiredFormFields = this.formFields;
  //       }
  //       else {
  //           this.requiredFormFields = this.allFormFields.filter(field => field.showIfNotSubnetted);
  //       }
  //   },

  props: {
    answers: Object,
    subnetted: {
        type: Boolean,
        required: true
    }
  },
};
</script>