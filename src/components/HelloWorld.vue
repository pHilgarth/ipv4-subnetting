<div class="form-group">
        <label for="addressClass" class="form-label">Class:</label>
        <input
          type="text"
          class="form-control"
          id="addressClass"
          v-model="userInput.addressClass"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="addressType" class="form-label"
          >Type (Public or Private):</label
        >
        <input
          type="text"
          class="form-control"
          id="addressType"
          v-model="userInput.addressType"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="dsnm" class="form-label">Default Subnet Mask:</label>
        <input
          type="text"
          class="form-control"
          id="dsnm"
          v-model="userInput.dsnm"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="nsnm" class="form-label">New Subnet Mask:</label>
        <input
          type="text"
          class="form-control"
          id="nsnm"
          v-model="userInput.nsnm"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="borrowed-bits" class="form-label">Borrowed Bits:</label>
        <input
          type="text"
          class="form-control"
          id="borrowedBits"
          v-model="userInput.borrowedBits"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="host-bits" class="form-label">Remaining Host Bits:</label>
        <input
          type="text"
          class="form-control"
          id="hostBits"
          v-model="userInput.hostBits"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="network" class="form-label">Network-Address:</label>
        <input
          type="text"
          class="form-control"
          id="network"
          v-model="userInput.network"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="first-host" class="form-label">First Host:</label>
        <input
          type="text"
          class="form-control"
          id="firstHost"
          v-model="userInput.firstHost"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="last-host" class="form-label">Last Host:</label>
        <input
          type="text"
          class="form-control"
          id="lastHost"
          v-model="userInput.lastHost"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group">
        <label for="broadcast" class="form-label">Broadcast-Address:</label>
        <input
          type="text"
          class="form-control"
          id="broadcast"
          v-model="userInput.broadcast"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group" v-if="subnetted">
        <label for="first-subnet" class="form-label">First Subnet:</label>
        <input
          type="text"
          class="form-control"
          id="firstSubnet"
          v-model="userInput.firstSubnet"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group" v-if="subnetted">
        <label for="next-subnet" class="form-label">Next (Sub)Net:</label>
        <input
          type="text"
          class="form-control"
          id="nextSubnet"
          v-model="userInput.nextSubnet"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group" v-if="subnetted">
        <label for="last-subnet" class="form-label">Last Subnet:</label>
        <input
          type="text"
          class="form-control"
          id="lastSubnet"
          v-model="userInput.lastSubnet"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group" v-if="subnetted">
        <label for="amount-subnets" class="form-label"
          >Amount of Subnets:</label
        >
        <input
          type="text"
          class="form-control"
          id="amountSubnets"
          v-model="userInput.amountSubnets"
        />
        <p class="feedback hidden"></p>
      </div>
      <div class="form-group" v-if="subnetted">
        <label for="amount-hosts" class="form-label"
          >Amount of Hosts per Subnet:</label
        >
        <input
          type="text"
          class="form-control"
          id="amountHosts"
          v-model="userInput.amountHosts"
        />
        <p class="feedback hidden"></p>
      </div>