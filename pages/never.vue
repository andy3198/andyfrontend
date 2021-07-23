<template>

    <div>

        <HeadBar />

        <div class="container">

            <h1>Form Application</h1>

            <div class="row">

                <div class="col-6">

                    <h5>List Form</h5>

                    <table class="table table-boredered table-hover">
                        <thead>
                            <tr class="bg-info text-white">
                                <th>First Name</th>
                                <th>Last Name</th>
                                <th>Address</th>
                                <th>Status</th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr v-for="asset in assets" :key="asset.id" @click="onSelected(asset)">
                                <td>{{asset.fname}}</td>
                                <td>{{asset.lname}}</td>
                                <td>{{asset.address}}</td>
                                <td>{{asset.status}}</td>
                            </tr>
                        </tbody>

                    </table>

                </div>

                <div class="col-4">
                    <SecretView :asset="selectedAsset" @saved="onSaveonDelete" @newAsset="onNew" @deleted="onSaveonDelete" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

export default {
    data() {
        return {
            assets: [],
            selectedAsset: {}
        }
    },
    methods: {
        async getAll() {
            await this.$axios.get('http://localhost:8000/api/assets')
            .then((res)=>{
                if(res.status==200) {
                    this.assets = res.data
                    console.log(this.assets)
                }
            })
        },

        onSaveonDelete() {
            this.getAll()
            this.selectedAsset = {}
        },

        onSelected(asset) {
            this.selectedAsset = asset
        },

        onNew() {
            this.selectedAsset = {}
        },
    },

    created() {
        this.getAll()
        this.selectedAsset = {}
    }
}

</script>
