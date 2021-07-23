<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Cars</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Cars</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="cars in cars" :key="cars.id" @click="onSelected(cars)">
                            {{cars.name}} <span class="float-right">{{cars.description}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <CarView :cars="selectedCar" @saved="onChange" @newCar="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            cars: [],
            selectedCar: {}
        }
    },
    methods: {
        async getMyCars() {
            await this.$axios.get('/api/cars')
            .then((res)=>{
                if(res.status==200) {
                    this.cars = res.data
                }
            })
        },
        onChange() {
            this.getMyCars()
            this.selectedCar = {}
        },
        onSelected(cars) {
            this.selectedCar = cars;
        },
        onNew() {
            this.selectedCar = {}
        },
    },
    created() {
        this.getMyCars()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: antiquewhite;
}
</style>