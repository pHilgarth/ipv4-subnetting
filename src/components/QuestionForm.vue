<template>
  <section class="questions">
    <h2>{{ userInput.dsnm }}</h2>
    <form>
      <form-group
        v-for="field in requiredFormFields"
        :key="field.name"
        :name="field.name"
        :text="field.text"
        @emit-value="updateValue"
      ></form-group>
      <button class="check-form button" @click.prevent="checkAnswers($event)">
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

  mounted() {
    if (this.subnetted) {
      this.requiredFormFields = this.allFormFields;
    } else {
      this.requiredFormFields = this.allFormFields.filter(
        (field) => field.showIfNotSubnetted
      );
    }
  },

  updated() {
    if (this.subnetted) {
      this.requiredFormFields = this.allFormFields;
    } else {
      this.requiredFormFields = this.allFormFields.filter(
        (field) => field.showIfNotSubnetted
      );
    }
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
        dsnm: "initial",
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

  props: {
    answers: Object,
    subnetted: Boolean
  },
};
</script>