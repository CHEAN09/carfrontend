<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Cars
        </button>
        <h5>Car View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Name">
                <b-form-input v-model="cars.name"></b-form-input>
            </b-form-group>

            <b-form-group label="Description">
                <b-form-input v-model="cars.description"></b-form-input>
            </b-form-group>

            <b-form-group label="Brand">
                <b-form-input v-model="cars.brand"></b-form-input>
            </b-form-group>

             <b-form-group label="Date Acquired" label-for="acquired_on">
                <b-form-input id="acquired_on" type="date" v-model="cars.acquired_on" trim></b-form-input>
                </b-form-group>

           <b-form-group label="Status" label-for="status">
                <b-form-select v-model="cars.status" :options="options"></b-form-select>
                </b-form-group>

            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="cars.id">Delete Car</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        cars: {},
        options: [
                {value: 'good', text: 'In good condition'},
                {value: 'damaged', text: 'Damaged'},
                {value: 'lost', text: 'Lost'},
            ]
    },
    methods: {
        async onSave() {
            try {
                if(!this.cars.id) {
                    await this.$axios.post('/api/cars', this.cars)
                }else{
                    await this.$axios.put('/api/cars/' + this.cars.id, this.cars)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newCar')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/cars/' + this.cars.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>