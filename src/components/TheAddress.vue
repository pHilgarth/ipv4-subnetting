
<template>
  <section class="ip-address">
    <h2 class="ip-address-value">{{ randomAddress }}</h2>
    <button class="ip-address-button button" @click="getRandomAddress">
      {{ labelIpAddressButton }}
    </button>
  </section>
</template>

<script>
export default {
  data() {
    return {
      labelIpAddressButton: "Get random IPv4-Address",
      randomAddress: "",
      subnetted: false,
    };
  },

  methods: {
    getRandomAddress() {
      this.subnetted = false;

      let randomAddress = "";
      let randomOctet;
      let defaultPrefixLength;
      let randomPrefixLength;

      //get 4 octets for random ipv4-address
      for (let i = 0; i <= 3; i++) {
        if (i == 0) {
          // when fetching the first octet we restrict the possible addresses to class a, b or c
          randomOctet = this.getRandomNumber(0, 224);

          // after fetching the first octet we generate a valid and random subnetmask
          if (randomOctet < 128) {
            //class a address - prefixLength minimum is 8
            randomPrefixLength = this.getRandomNumber(8, 31);
            defaultPrefixLength = 8;
          } else if (randomOctet < 192) {
            //class b address - prefixLength minimum is 16
            randomPrefixLength = this.getRandomNumber(16, 31);
            defaultPrefixLength = 16;
          } else {
            //class c address - prefixLength minimum is 24
            randomPrefixLength = this.getRandomNumber(24, 31);
            defaultPrefixLength = 24;
          }
        } else {
          //fetching the last 3 octets
          randomOctet = this.getRandomNumber(0, 256);
        }

        //constructing the final address
        randomAddress += randomOctet;

        if (i < 3) {
          randomAddress += ".";
        } else {
          randomAddress += "/" + randomPrefixLength;
        }

        if (defaultPrefixLength !== randomPrefixLength) {
          this.subnetted = true;
        }
      }

      this.randomAddress = randomAddress;
      this.labelIpAddressButton = "Get new IPv4-Address";

      let answers = this.getAnswers(randomAddress, defaultPrefixLength);

      this.$emit('address-generated', answers, this.subnetted);
    },

    getRandomNumber(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },

    getAnswers(randomAddress, defaultPrefixLength) {
        let answers = {
            addressClass: '',
        addressType: '',
        dsnm: '',
        nsnm: '',
        borrowedBits: '',
        hostBits: '',
        network: '',
        firstHost: '',
        lastHost: '',
        broadcast: '',
        firstSubnet: '',
        nextSubnet: '',
        lastSubnet: '',
        amountSubnets: '',
        amountHosts: ''
        };

      randomAddress = "201.234.5.1/24";
      defaultPrefixLength = 24;

      let splittedAddress = randomAddress.split("/");
      let octets = splittedAddress[0].split(".");
      let newPrefixLength = Number(splittedAddress[1]);

      answers.addressClass = this.getClass(octets[0]);
      answers.addressType = this.getType(octets);
      answers.dsnm = this.getSubnetMask(defaultPrefixLength);
      answers.nsnm = this.getSubnetMask(newPrefixLength);
      answers.borrowedBits = (newPrefixLength - defaultPrefixLength).toString();
      answers.amountSubnets = Math.pow(2, answers.borrowedBits).toString();
      answers.hostBits = (32 - newPrefixLength).toString();
      answers.amountHosts = (Math.pow(2, answers.hostBits) - 2).toString();
      answers.network = this.getNetworkAddress(octets, newPrefixLength);
      answers.firstHost = this.getFirstHost(answers.network);
      answers.nextSubnet = this.getNextSubnet(answers.network, newPrefixLength);
      answers.broadcast = this.getBroadcast(answers.nextSubnet, newPrefixLength);
      answers.lastHost = this.getLastHost(answers.broadcast);
      answers.firstSubnet = this.getFirstSubnet(answers.network, defaultPrefixLength);
      answers.lastSubnet = this.getLastSubnet(
        answers.network,
        defaultPrefixLength,
        newPrefixLength
      );

      return answers;
    },

    getClass(firstOctet) {
      let addressClass = "";

      if (firstOctet < 128) {
        addressClass = "a";
      } else if (firstOctet < 192) {
        addressClass = "b";
      } else {
        addressClass = "c";
      }

      return addressClass;
    },

    getType(octets) {
      if (
        octets[0] === "10" || //class A privates
        (octets[0] === "172" && octets[1] >= "16" && octets[1] <= "31") || //class B privates
        (octets[0] === "192" && octets[1] === "168")
      ) {
        //class C privates
        return "private";
      } else {
        return "public";
      }
    },

    getSubnetMask(prefixLength) {
      let subnetMask = "";

      for (let i = 0; i < 4; i++) {
        if (prefixLength >= 8) {
          subnetMask += "255.";

          //remove processed bit
          prefixLength -= 8;
        } else {
          subnetMask += (256 - Math.pow(2, 8 - prefixLength)).toString() + ".";

          //remove processed bit
          prefixLength -= prefixLength;
        }
      }

      //remove the last '.' from the result and return it
      return subnetMask.slice(0, -1);
    },

    getNetworkAddress(octets, prefixLength) {
      //calculate each octet
      for (let i = 0; i < 4; i++) {
        if (prefixLength >= 8) {
          //networkAddress += octets[i] + '.';

          //remove processed bit
          prefixLength -= 8;
        } else {
          let octet = 0;
          let bitCount = 0;

          for (let _i = 7; _i >= 0; _i--) {
            if (bitCount < prefixLength) {
              let bitValue = Math.pow(2, _i);

              octet += bitValue;

              if (octet > octets[i]) {
                octet -= bitValue;
              }

              bitCount++;
            } else {
              break;
            }
          }

          octets[i] = octet.toString();

          //networkAddress += octet + '.';

          //remove processed bit
          prefixLength -= prefixLength;
        }
      }

      return octets.join(".");
    },

    getFirstHost(network) {
      //let firstHost = '';
      let octets = network.split(".");
      let lastIndex = octets.length - 1;

      octets[lastIndex] = (Number(octets[lastIndex]) + 1).toString();

      // octets.forEach(octet => {
      //     firstHost += octet + '.';
      // });

      return octets.join("."); //firstHost.slice(0, -1);
    },

    getLastHost(broadcast) {
      let octets = broadcast.split(".");
      let lastIndex = octets.length - 1;

      octets[lastIndex] = (Number(octets[lastIndex]) - 1).toString();

      return octets.join(".");
    },

    getBroadcast(nextSubnet) {
      let decrementValue = true;
      let octets = nextSubnet.split(".");

      //starting from last octet
      for (let i = 3; i >= 0; i--) {
        if (Number(octets[i]) === 0) {
          octets[i] = "255";

          //broadcast = '255.' + broadcast;
        } else {
          if (decrementValue) {
            octets[i] = (Number(octets[i]) - 1).toString();
            //broadcast = (Number(octets[i]) - 1).toString() + '.' + broadcast;

            decrementValue = false;
          } else {
            //broadcast = octets[i] + '.' + broadcast;
          }
        }
      }

      return octets.join(".");
      //return broadcast.slice(0, -1);
    },

    getFirstSubnet(network, prefixLength) {
      let octets = network.split(".");

      for (let i = 0; i < 4; i++) {
        prefixLength -= 8;

        if (prefixLength < 0) {
          octets[i] = "0";
        }
      }

      return octets.join(".");
    },

    getNextSubnet(network, prefixLength) {
      let octets = network.split(".");
      let incrementPreviousOctet = false;

      //get each octet
      for (let i = 0; i < 4; i++) {
        if (prefixLength >= 8) {
          //nextSubnet += octets[i] + '.';

          prefixLength -= 8;

          if (prefixLength == 0) {
            incrementPreviousOctet = true;
          }
        } else if (prefixLength > 0) {
          let bitValue = Math.pow(2, 8 - prefixLength);

          octets[i] = (Number(octets[i]) + bitValue).toString();

          let octetCounter = i;

          while (octets[octetCounter] > 255) {
            octets[octetCounter] = "0";
            octetCounter--;

            octets[octetCounter] = (
              Number(octets[octetCounter]) + 1
            ).toString();
          }

          prefixLength -= prefixLength;

          incrementPreviousOctet = false;
        } else {
          if (incrementPreviousOctet) {
            octets[i - 1] = (Number(octets[i - 1]) + 1).toString();

            let octetCounter = i - 1;

            while (octets[octetCounter] > 255) {
              octets[octetCounter] = "0";
              octetCounter--;

              octets[octetCounter] = (
                Number(octets[octetCounter]) + 1
              ).toString();
            }

            incrementPreviousOctet = false;
          }
        }
      }

      return octets.join(".");
    },

    getLastSubnet(network, defaultPrefixLength, newPrefixLength) {
      let prefixLengthHelper = defaultPrefixLength;
      let octets = network.split(".");

      for (let i = 0; i < 4; i++) {
        prefixLengthHelper -= 8;

        if (prefixLengthHelper < 0) {
          newPrefixLength -= defaultPrefixLength;

          for (let _i = i; _i < 4; _i++) {
            if (newPrefixLength >= 8) {
              octets[_i] = "255";

              newPrefixLength -= 8;
            } else if (newPrefixLength > 0) {
              octets[_i] = (256 - Math.pow(2, 8 - newPrefixLength)).toString();

              newPrefixLength -= newPrefixLength;
            } else {
              octets[_i] = "0";
            }
          }

          break;
        }
      }

      return octets.join(".");
    },
  },
};
</script>
