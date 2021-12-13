<template>
<div class="CardChecks">
    <div class="container py-5">
        <div class="card col-12">
            <div class="card-body">
                <h5 class="card-title text-center">OPCIONES DE FACTURACIÓN</h5>
                <div class="form-check form-switch">
                    <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault" disabled :value="'matricula'" checked>
                    <label class="form-check-label" for="flexSwitchCheckDefault">Matrícula: {{matricula}}</label>
                </div>
                <div class="form-check form-switch">
                    <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckChecked" @change="change" :value="'Anualidad'">
                    <label class="form-check-label" for="flexSwitchCheckChecked">Anualidad: {{anualidad}}</label>
                </div>

                <div class="form-check form-switch">
                    <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckChecked" @change="change" :value="'Asopaf'">
                    <label class="form-check-label" for="flexSwitchCheckChecked">Asopaf {{asopaf}}</label>
                </div>

                <div class="form-check form-switch">
                    <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckChecked" @change="change" :value="'Fundación'">
                    <label class="form-check-label" for="flexSwitchCheckChecked">Donación fundación: </label>
                    <div class="input-group mb-3 mt-3">
                        <span class="input-group-text">$</span>
                        <input type="text" class="form-control" aria-label="Amount (to the nearest dollar)" v-model="fundacion">
                        <span class="input-group-text">.00</span>
                    </div>
                </div>

            </div>
        </div>
        <!---->
        <div class="row m-5"></div>
        <div class="card col-12">
            <div class="card-body">
                <h5 class="card-title text-center">FACTURA</h5>
                <div v-for="item in checkedCategories" :key="item.title">
                    <label for="" class="mr-1">{{item.title}} :</label>
                    <input type="text" disabled :value="item.price">
                </div>
            </div>

            <div class="card-footer">
                <h6>Total: {{formatPrice(total)}}</h6>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'CardCheck',
    data() {
        return {
            checkedCategories: [],
            matricula: 2000000,
            anualidad: 20000000,
            asopaf: 180000,
            fundacion: 500000,
            total: 0
        }
    },
    mounted() {

        this.checkedCategories.push({
            "title": 'Matrícula',
            'price': this.matricula
        })
        this.total = this.totalCalc()
        console.log(this.checkedCategories)
    },
    methods: {
        formatPrice(value) {
            let val = (value / 1).toFixed(2).replace('.', ',')
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
        },
        totalCalc: function () {
            this.total = 0
            this.checkedCategories.forEach(e => {
                console.log("Total :" + this.total + " Price: " + e.price)
                this.total += e.price
            });
            return this.total
        },
        change(event) {
            //nextTICK es para que ejecute el método solo cuando cambie el valor del check.
            this.$nextTick(() => {
                console.log(event.target.value, event.target.checked)
                var price = 0
                if (event.target.value == 'Anualidad') {
                    price = this.anualidad
                } else if (event.target.value == 'Asopaf') {
                    price = this.asopaf
                } else {
                    price = this.fundacion
                }

                if (event.target.checked) {
                    var bandera = false
                    if (this.checkedCategories.length > 0) {
                        this.checkedCategories.forEach((e) => {
                            if ((e.title == event.target.value) == false) {
                                bandera = true

                            } else {
                                bandera = false
                            }
                        })
                        if (bandera) {
                            this.checkedCategories.push({
                                'title': event.target.value,
                                'price': price
                            })
                            this.totalCalc()
                        }
                    } else {
                        this.checkedCategories.push({
                            'title': event.target.value,
                            'price': price
                        })
                        this.totalCalc()
                    }
                } else {
                    console.log("Entra al false")
                    for (var i = 0; i < this.checkedCategories.length; i++) {
                        console.log("Validación " + (i + 1))
                        console.log(this.checkedCategories[i].title + " - " + event.target.value)
                        if (this.checkedCategories[i].title == event.target.value) {
                            console.log("Es igual")
                            let indice = this.checkedCategories.findIndex(elemento => elemento.title === event.target.value);
                            console.log("El elemento buscado está en el índice ", indice);
                            this.checkedCategories.splice(indice, 1)
                            this.totalCalc()
                        }
                    }
                }
            })
        }
    }
}
</script> 

<style scoped>
</style>
