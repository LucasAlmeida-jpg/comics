<template>
    <div class="container">
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#searchModal">
            Modal
        </button>

        <div class="modal fade" id="searchModal" tabindex="-1" aria-labelledby="searchModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-xl">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="searchModalLabel"></h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row d-flex align-items-center justify-content-center">
                            <div class="col-md-6">
                                <h3 class="fs-3 text mb-3 fw-bold">Search title and issue number</h3>
                                <div class="d-flex gap-2">
                                    <input v-model="search" class="form-control" type="text"
                                        aria-label="default input example">
                                    <input v-model="searchIssue" class="form-control" type="text"
                                        aria-label="default input example">
                                    <img role="button" @click="searchProducts" src="./icons/search.svg"
                                        alt="icon search">
                                </div>
                            </div>
                        </div>
                        <div class="row d-flex gap-2 align-items-center justify-content-center mt-4">
                            <div class="col-md-3">
                                <h4 class="fs-6 fw-bold">Metro/cc</h4>
                                <div class="border p-1">
                                    <div class="form-check" v-for="product in productsFiltered" :key="product.issue">
                                        <input class="form-check-input" type="checkbox"
                                            :id="'flexCheckDefault' + product.issue"
                                            @change="toggleProductSelection(product)">
                                        <label class="form-check-label" :for="'flexCheckDefault' + product.issue">
                                            {{ product.title }} - {{ product.issue }}
                                        </label>
                                    </div>

                                </div>
                            </div>
                            <div class="col-md-3">
                                <h4 class="fs-6 fw-bold">GCD</h4>
                                <div class="border p-1">
                                    <div class="form-check" v-for="product in productsFiltered" :key="product.issue">
                                        <input class="form-check-input" type="checkbox"
                                            :id="'flexCheckDefault' + product.issue"
                                            @change="toggleProductSelection(product)">
                                        <label class="form-check-label" :for="'flexCheckDefault' + product.issue">
                                            {{ product.title }} - {{ product.issue }}
                                        </label>
                                    </div>

                                </div>
                            </div>
                            <div class="col-md-3">
                                <h4 class="fs-6 fw-bold">MCS</h4>
                                <div class="border p-1">
                                    <div class="form-check" v-for="product in productsFiltered" :key="product.issue">
                                        <input class="form-check-input" type="checkbox"
                                            :id="'flexCheckDefault' + product.issue"
                                            @change="toggleProductSelection(product)">
                                        <label class="form-check-label" :for="'flexCheckDefault' + product.issue">
                                            {{ product.title }} - {{ product.issue }}
                                        </label>
                                    </div>

                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row d-flex align-items-center justify-content-center">
                        <div class="card col-md-5" v-for="i in arrayProduct">
                            <div class="d-flex gap-3 align-items-center ">
                                <div>
                                    <img class="thumb-product" :src="i.image_url"
                                        alt="product image_url">
                                </div>
                                <div>
                                    <h5>{{ i.title }} {{ i.issue }}</h5>
                                    <p>{{ i.description }}</p>
                                    <p><strong>PUBLISHED:</strong> March 01, 1989</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer d-flex justify-content-between">
                        <button type="button" class="btn btn-outline-dark">MERGE</button>
                        <button type="button" class="btn btn-outline-dark">REFERENCE</button>
                        <button type="button" class="btn btn-dark" data-bs-dismiss="modal">DONE</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            product: [],
            search: '',
            searchIssue: '',
            productsFiltered: [],
            arrayProduct: [],
            products: []

        };
    },

    mounted() {
        axios.get('http://localhost:3000/comics')
            .then(response => {
                if (response.data && Array.isArray(response.data)) {
                    this.products = response.data;
                    console.log('retorno da api no componente pai:', this.products );
                }
            })
            .catch(error => {
                console.error('Erro ao carregar dados do produto:', error);
            });
    },

    computed: {
        filteredProduct() {
            return this.productsFiltered[0].map(product => {
                return {
                    title: product.title,
                    id: product.issue
                };
            });
        }
    },

    methods: {
        searchProducts() {
            const searchTitle = this.search.toLowerCase();
            
            if (this.searchIssue) {
                const searchIssue = `#${parseInt(this.searchIssue.trim())}`;
                this.productsFiltered = this.products.filter(product =>
                    product.title.toLowerCase().includes(searchTitle) && product.issue === searchIssue
                );
            } else {
                this.productsFiltered = this.products.filter(product =>
                    product.title.toLowerCase().includes(searchTitle)
                );

                if (this.productsFiltered.length === 1) {
                    this.searchIssue = this.productsFiltered[0].issue.replace("#", ""); 
                }
            }
        },

        toggleProductSelection(product) {
            this.arrayProduct = [product];
            console.log(this.arrayProduct, 'retorno do arrayProduct');
        }
    }

};
</script>

<style scoped lang="scss">
.form-control:first-child {
    min-width: 400px;
}

.border {
    max-height: 136px;
    min-height: 136px;
    overflow-y: auto;
    overflow-x: hidden;
    border-radius: 5px
}

.form-check-label {
    font-size: 10px;
}

.thumb-product {
    width: 100px;
    height: auto;
    object-fit: cover;
}

.card {
    background-color: #EEEEEE;
    border: none;
    padding: 29px;

    h5 {
        font-size: 13px;
        font-weight: bold;
        text-transform: uppercase;
    }

    p {
        font-size: 10px;
    }
}

@media (max-width: 768px) {
    .form-control:first-child {
        min-width: 0px;
    }

}
</style>
