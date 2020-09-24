<template>
<div>

<nav id="header" class="w-full z-10 pin-t">

	<div id="progress" class="h-1 z-20 pin-t" style="background:linear-gradient(to right, #4dc0b5 var(--scroll), transparent 0);"></div>

		<div class="w-full mx-auto flex flex-wrap items-center justify-between mt-0 py-3 bg-white px-4">

			<div class="pl-4">
				<a class="text-black text-base no-underline hover:no-underline font-extrabold text-xl"  href="#">
					Portals
				</a>
      </div>


			<div class="  flex items-center w-auto mt-2 lg:mt-0 bg-grey-lightest md:bg-transparent z-20" id="nav-content">
				<div class="  lg:flex justify-end flex-1 items-center">
					<MetamaskDropdown
            :acctAddress= "activeAccountAddress"
						:providerNetworkID= "providerNetworkID"
          />
				</div>
			</div>
		</div>
	</nav>



  <div class="lg:flex mb-4">

   	<GameCanvas	ref="gameCanvas"/>

  </div>




</div>
</template>


<script>
import MetamaskDropdown from './MetamaskDropdown.vue'
import GameCanvas from './GameCanvas.vue'
import Web3Helper from '../js/web3-helper.js'
import CryptoAssets from '../js/cryptoassets.js'
import MaticHelper from '../js/matic-helper.js'

const Web3 = require('web3');



export default {
  name: 'Home',
  components: {
     MetamaskDropdown,GameCanvas
  },
  data () {
    return {
      activeAccountAddress: null,
			network: 'ethereum',
			providerNetworkID: null,
			assetName: '0xBTC',
			roomId: null
    }
  },
  created () {


     this.checkSignedIn()



			if ( window.ethereum.selectedAddress) {
				 Web3Helper.init();
	      	this.readWeb3Data();  //opens the window
		 }





  },
	mounted () {
		this.roomId = this.$route.params.id;

 	 console.log(this.$refs.gameCanvas)
 	 this.$refs.gameCanvas.teleportToRoom(this.roomId)


 },
  methods: {
   async checkSignedIn () {



		await window.ethereum

		console.log(window.ethereum)

		 if (!window.ethereum.selectedAddress) {
			 this.$router.replace('/login');
			 return;
		}



    },
   async readWeb3Data () {
     var accounts = await Web3Helper.getConnectedAccounts();

		 this.providerNetworkID = await Web3Helper.getProviderNetworkID();

     this.activeAccountAddress = accounts[0]

		 //this.updateBalances()
   },
	 async setNetwork(networkName)
	 {
		 this.network = networkName;

		 // await this.updateBalances()

		 this.$refs.txform.updateAll()
	 },
	 async selectAsset(assetName)
	 {
		 this.assetName = assetName;


 		this.$refs.txform.updateAll()

		 // await this.updateBalances()
	 },




  }

}
</script>
