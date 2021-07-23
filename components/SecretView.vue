<template>

    <div>

        <button class="btn btn-success float-right" @click="onNew">
            New Form
        </button>

        <h5>Form View</h5>

        <hr>

        <form action="#" @submit.prevent="onSave">
            <b-form-group
                label="First Name"
                label-for="fname"
                >
                <b-form-input id="fname" v-model="asset.fname" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Last Name"
                label-for="lname"
                >
                <b-form-input id="lname" v-model="asset.lname" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Address"
                label-for="address"
                >
                <b-form-input id="address" v-model="asset.address" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Email"
                label-for="email"
                >
                <b-form-input id="email" v-model="asset.email" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Status"
                label-for="status"
                >
                <b-form-select id="status" v-model="asset.status" :options="statuses"></b-form-select>
            </b-form-group>

            <b-form-group
                label="Birth Date"
                label-for="acquired_on"
                >
                <b-form-input id="acquired_on" type="date" v-model="asset.acquired_on" trim></b-form-input>
            </b-form-group>


            <b-form-group>
                <button class="btn btn-primary" type="submit">Save</button>
                <button class="btn btn-danger" type="button" @click="onDelete" v-if="asset.id">Delete</button>
            </b-form-group>
                
        </form>

    </div>

</template>

<script>

export default {
    props: {
        asset: {}
    },
    data() {
        return {
            asset: {},
            statuses: [
                { value: 'Single', text: 'Single' },
                { value: 'Taken', text: 'Taken' },
                { value: 'Marriage', text: 'Marriage' },
                { value: 'Widow', text: 'Widow' },
                
            ]
        }
    },
    methods: {
        async onSave() {
            try {

                if(!this.asset.id) {
                    await this.$axios.post('http://localhost:8000/api/assets', this.asset)
                    alert('Adde to form')

                }else{
                    await this.$axios.put('http://localhost:8000/api/assets/' + this.asset.id, this.asset)
                    alert('Update Form')

                }

                this.$emit('saved');

            } catch (error) {
                alert(error.response.data.message)
            }
        },

        onNew() {
            this.$emit('newAsset')
        },

        async onDelete() {
            try {
                await this.$axios.delete('http://localhost:8000/api/assets/' + this.asset.id)
                alert('Delete Form')

                await this.$emit('deleted')
                
            } catch (error) {
                alert(error.response.data.message)
            }
            
        }
    }
}

</script>