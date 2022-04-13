<template>
    <div class="min-h-screen bg-gray-100 flex flex-col py-12 px-6 lg:px-8">
        <h1 class="text-lg font-bold"> Lista de Criptoativos </h1>
        
        <br>
        
        <div class="flex items-stretch">
        <input
            type="text"
            name="search"
            placeholder=" Search asset"
            autocomplete="off"
            aria-label="Seach asset"
            class = "px-3 py-2 font-semibold placeholder-gray-500 text-black rounded-2xl border-none ring-2 ring-gray-300 focus:ring-gray-500 focus:ring-2"
            v-model="input_data"
        >
        <button 
            v-on:click="add_asset"
            class="bg-blue-500 hover:bg-blue-400 text-white mx-6 font-semibold ring-2 ring-gray-200 px-8 rounded-full">
            Add
        </button>
        </div>
        <br>
        <br>
        <tr
        v-for="asset in assets_list"
        :key = "asset.id"
        class = "bg-white"
        >
        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
            <div class="flex flex-col"> 
                <div class= "flex items-center">
                <h1 class="text-lg font-bold"> {{ asset.name }} </h1>
                <h1 class="px-2"> {{ asset.asset_id }} </h1>
                </div>
                <div>
                <h1> ${{asset.price_usd}}  </h1>
                </div>
            </div>
            <a
              href="#"
              class="text-red-600 hover:text-red-900"
              @click.stop.prevent="delete_asset(asset.name)"
            > Excluir
            </a>
        </td>
        </tr>
    </div>
</template>

<script>
    export default {
        
        data() {
            return {
                api_key: 'D1B4E8CD-8F87-464A-8EC7-F6A75DD7A50B',
                all_assets_list:[],
                assets_list:[],
                input_data:null,
            };
        },

        async created(){
            const headers = { "X-CoinAPI-Key":this.api_key}
            const response = await fetch("https://rest.coinapi.io/v1/assets", {headers});
            const data = await response.json();
            this.all_assets_list = data
            this.assets_list = [this.all_assets_list.find(e => e.name === "Bitcoin")]
            console.log(this.assets_list)
            // setInterval(this.refresh_quotations, 10000)
        },

        methods: {
            add_asset(){
                const response = this.all_assets_list.find(e => e.name === this.input_data)
                if (response == null){ 
                    return alert("There's no assets with this name")
                }
                else {
                   this.assets_list = [...this.assets_list, response] 
                }
            },

            delete_asset(name){
                this.assets_list = this.assets_list.filter(asset => asset.name != name)
            },
            
        },
    };
</script>