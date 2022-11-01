<template>

    <div class="container-fluid pt-5">
        <div class="row px-xl-5">
            <!-- Shop Sidebar Start -->
            <div class="col-lg-3 col-md-12">
                <!-- Price Start -->
                <div class="border-bottom mb-4 pb-4">
                    <h5 class="font-weight-semi-bold mb-4">Filter by Category</h5>
                    <form>
                        <div
                            class="custom-control custom-checkbox d-flex align-items-center justify-content-between mb-3">
                            <input type="checkbox" class="custom-control-input" checked id="price-all">
                            <label class="custom-control-label" for="price-all">All Category</label>
                            <span class="badge border font-weight-normal">1000</span>
                        </div>
                        <div class="custom-control custom-checkbox d-flex align-items-center justify-content-between mb-3"
                            v-for="category in categorises" :key="category.id">
                            <input type="checkbox" class="custom-control-input" :id="'cr' + category.id"
                                @change="getFilterProducts()" :value="category.id" v-model="categoryId">
                            <label class="custom-control-label" :for="'cr' + category.id">{{ category.name }}</label>
                        </div>
                    </form>
                </div>
                <!-- Price End -->


                <!-- Size Start -->
                <div class="mb-5">
                    <h5 class="font-weight-semi-bold mb-4">Filter by Brand </h5>
                    <form>


                        <div
                            class="custom-control custom-checkbox d-flex align-items-center justify-content-between mb-3">
                            <input type="checkbox" class="custom-control-input" checked id="color-all">
                            <label class="custom-control-label" for="color-all">All Brand</label>
                            <span class="badge border font-weight-normal">1000</span>
                        </div>
                        <div class="custom-control custom-checkbox d-flex align-items-center justify-content-between mb-3"
                            v-for="Brand in Brands" :key="Brand.id">
                            <input type="checkbox" class="custom-control-input" :id="'br' + Brand.id"
                                @change="getFilterProducts()" :value="Brand.id" v-model="BrandId">
                            <label class="custom-control-label" :for="'br' + Brand.id">{{ Brand.name }}</label>
                        </div>
                    </form>
                </div>
                <!-- Size End -->
            </div>
            <!-- Shop Sidebar End -->


            <!-- Shop Product Start -->
            <div class="col-lg-9 col-md-12">
                <div class="row pb-3">
                    <div class="col-12 pb-1">
                        <div class="d-flex align-items-center justify-content-between mb-4">
                            <form action="">
                                <div class="input-group">
                                    <input type="text" class="form-control" placeholder="Search by name">
                                    <div class="input-group-append">
                                        <span class="input-group-text bg-transparent text-primary">
                                            <i class="fa fa-search"></i>
                                        </span>
                                    </div>
                                </div>
                            </form>
                            <div class="dropdown ml-4">
                                <button class="btn border dropdown-toggle" type="button" id="triggerId"
                                    data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                    Sort by
                                </button>
                                <div class="dropdown-menu dropdown-menu-right" aria-labelledby="triggerId">
                                    <a class="dropdown-item" href="#">Latest</a>
                                    <a class="dropdown-item" href="#">Popularity</a>
                                    <a class="dropdown-item" href="#">Best Rating</a>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-lg-4 col-md-6 col-sm-12 pb-1" v-for="Product in Products" :key="Product.id">
                        <div class="card product-item border-0 mb-4">
                            <div
                                class="card-header product-img position-relative overflow-hidden bg-transparent border p-0">
                                <img class="img-fluid w-100" :src="'/public/Image/' + Product.image" alt="">
                            </div>
                            <div class="card-body border-left border-right text-center p-0 pt-4 pb-3">
                                <h6 class="text-truncate mb-3">{{ Product.name }}</h6>
                                <div class="d-flex justify-content-center">
                                    <h6>${{ Product.price }}</h6>
                                    <h6 class="text-muted ml-2"><del>$123.00</del></h6>
                                </div>
                            </div>
                            <div class="card-footer d-flex justify-content-between bg-light border">
                                <a href="" class="btn btn-sm text-dark p-0"><i
                                        class="fas fa-eye text-primary mr-1"></i>View
                                    Detail</a>
                                <a href="" class="btn btn-sm text-dark p-0"><i
                                        class="fas fa-shopping-cart text-primary mr-1"></i>Add To Cart</a>
                            </div>
                        </div>
                    </div>

                    <div class="col-12 pb-1">
                        <nav aria-label="Page navigation">
                            <ul class="pagination justify-content-center mb-3">
                                <li class="page-item disabled">
                                    <a class="page-link" href="#" aria-label="Previous">
                                        <span aria-hidden="true">&laquo;</span>
                                        <span class="sr-only">Previous</span>
                                    </a>
                                </li>
                                <li class="page-item active"><a class="page-link" href="#">1</a></li>
                                <li class="page-item"><a class="page-link" href="#">2</a></li>
                                <li class="page-item"><a class="page-link" href="#">3</a></li>
                                <li class="page-item">
                                    <a class="page-link" href="#" aria-label="Next">
                                        <span aria-hidden="true">&raquo;</span>
                                        <span class="sr-only">Next</span>
                                    </a>
                                </li>
                            </ul>
                        </nav>
                    </div>
                </div>
            </div>
            <!-- Shop Product End -->
        </div>
    </div>
</template>

<script>
import axios from 'axios';


export default {
    data() {
        return {
            categorises: [],
            Brands: [],
            Products: [],
            categoryId: [],
            BrandId: [],
        }
    },

    created() {
        this.getAllCategorises();
        this.getAllBrands();
        this.getAllProducts();
        // this.getFilterProducts();
    },
    methods: {
        getAllCategorises() {
            axios.get('/get/all/categorises')
                .then(response => {
                    this.categorises = response.data
                }).catch(error => {
                    console.log(error)
                })
        },
        getAllBrands() {
            axios.get('/get/all/brands')
                .then(response => {
                    this.Brands = response.data
                }).catch(error => {
                    console.log(error)
                })
        },
        getAllProducts() {
            axios.get('/get/all/products')
                .then(response => {
                    this.Products = response.data
                }).catch(error => {
                    console.log(error)
                })
        },
        getFilterProducts() {
            axios.post('/get/filter/products', { category_id: this.categoryId, brand_id: this.BrandId })
                .then(response => {
                    if (this.categoryId.length > 0) {
                        this.Products = response.data
                    } else {
                        this.getAllProducts()
                    }

                }).catch(error => {
                    console.log(error)
                })
        },
    }
}
</script>
