<template>
<div>
    <div class="container">
        <div class="container">
            <v-row>
                <v-col>
                    <v-card elevation="0">
                        <v-card-actions>
                            <v-btn @click="Refresh" color="black" text medium>
                                <div class="d-flex">
                                    <v-icon>mdi-refresh</v-icon> <span>Refresh store</span>
                                </div>

                            </v-btn>

                        </v-card-actions>
                        <v-row>
                            <v-col cols="12" md="3" lg="3" sm="3">
                                <v-card color="">

                                    <v-card-actions>
                                        <div class="container">
                                            <v-btn color="black" icon large>
                                                <v-icon>mdi-hanger</v-icon>
                                                <h2 id="o_count">{{ all_stock }}</h2>
                                            </v-btn>
                                            <h4 style="color: black">My stock</h4>
                                        </div>
                                    </v-card-actions>
                                </v-card>
                            </v-col>
                            <v-col cols="12" md="3" lg="3" sm="3">
                                <v-card color="">

                                    <v-card-actions>
                                        <div class="container">
                                            <v-btn color="green" icon large>
                                                <v-icon>mdi-check-decagram-outline</v-icon>
                                                <h2 id="o_count">{{ sold_stock }}</h2>
                                            </v-btn>
                                            <h4 style="color: green">Sold out</h4>
                                        </div>
                                    </v-card-actions>
                                </v-card>
                            </v-col>

                            <v-col cols="12" md="12" lg="12" sm="12">
                                <v-card-actions>
                                    <v-btn v-show="!edit" @click="edit = true" color="green" text small>
                                        <div class="d-flex">
                                            <v-icon small>mdi-pencil</v-icon> Add Discount
                                        </div>
                                    </v-btn>
                                    <v-btn @click="dialog_delete_sold = true" color="red" text small>
                                        <div class="d-flex">
                                            <v-icon small>mdi-delete-empty-outline</v-icon> Delete All Sold out
                                        </div>
                                    </v-btn>
                                    <v-btn v-show="edit" color="red" @click="edit = false" icon large>
                                        <v-icon>mdi-close</v-icon>
                                    </v-btn>
                                   <div class="container">
                                    <label v-show="edit" for="discount_per">Make sure you switch (Discount) on the item to upload the discount.</label>
                                    <label v-show="edit" for="discount_per">You can add discount % on different items.</label>

                                    <v-text-field v-show="edit" v-model="discount_per" :counter="10" :rules="nameRules2" label="Discount percentage %"></v-text-field>

                                   </div>
                                    <!-- <v-btn v-show="edit" color="black" class="white--text" @click="UploadDiscount">
                                    Upload Discount
                                    </v-btn> -->

                                </v-card-actions>
                            </v-col>

                            <v-col cols="12" md="12" lg="12" sm="12">
                              <br>
                              <br>
                              <div id="items_view">
            <v-card elevation="0">
                <div class="row">
                    <div class="logo py-0 row justify-center">
                        <div v-scroll.self="onScroll" class="with-header row overflow-y-auto" align="center" justify="center" height="500">
                            <div v-for="cart_list in all_products" :key="cart_list.id" :id="cart_list.id" :cart_list="cart_list.cart_list" class="col-sm-3 text-center" align="center" justify="center">
                                <v-card align="center" justify="center" elevation="0" id=""  class="car_card mx-auto my-12 text-center" max-width="300">
                                    <div id="clothImage">
                                        <v-img height="180" max-height="180" class="align-end" cover :contain="true" width="100%" :src="cart_list.cloth_image">
                                            <template v-slot:placeholder>
                                                <v-row class="fill-height ma-0" align="center" justify="center">
                                                    <v-progress-circular indeterminate color="secondary"></v-progress-circular>
                                                </v-row>
                                            </template>
                                            <v-chip color="red" v-show="cart_list.sold" style="color: white">SOlD OUT</v-chip>
                                        </v-img>
                                    </div>

                                    <v-card-actions>
                                        <div class="container">
                                            <div class="" style="margin: 0px">
                                                <div>
                                                    <h4>{{ cart_list.cloth_name }}</h4>
                                                    <div v-show="cart_list.discount_state">
                                                        <strike style="font-size: 0.8rem"><strong>{{ cart_list.cloth_price }}</strong></strike>
                                                    </div>
                                                    <div v-show="cart_list.discount_state">
                                                        <strong>{{
                                discount(cart_list.cloth_price, cart_list.discount)
                              }}</strong>
                                                    </div>
                                                </div>

                                                <div style="margin: 0px" id="pricing">
                                                    <span v-show="cart_list.discount_state">
                                                        <h4>
                                                            <span>{{ cart_list.discount }}</span>
                                                            <v-icon small>mdi-percent</v-icon>
                                                        </h4>
                                                    </span>
                                                </div>
                                                <div class="row">
                                                  <v-switch color="pink" @click="
                                updateStatus55(cart_list.discount_state, cart_list.id)
                              "  :label="`Discount: ${cart_list.discount}`"></v-switch>
                                                    <v-switch color="pink" @click="updateStatus4(cart_list.sold, cart_list.id)" v-model="cart_list.sold" :label="`Sold: ${cart_list.sold}`"></v-switch>
                                                    <v-switch @click="updateStatus3(cart_list.state, cart_list.id)" color="pink" v-model="cart_list.state" :label="`Published: ${cart_list.state}`"></v-switch>
                                                    <v-switch color="pink" @click="updateStatus1(cart_list.Negotiable, cart_list.id)" v-model="cart_list.Negotiable" :label="`Negotiable: ${cart_list.Negotiable}`"></v-switch>
                                                    
                                                </div>
                                                <div class="row text-center" style="background-color: black; border-radius: 2px">
                                                    <div>
                                                        <v-btn icon color="red" @click="(dialog_delete = true), (item_id = cart_list.id)">
                                                            <v-icon>mdi-delete</v-icon>
                                                        </v-btn>
                                                    </div>
                                                    <div>
                                                        <v-btn icon color="primary" @click="
                                  (updateItemDialog = true),
                                    (item_id = cart_list.id),
                                    (cloth_image = cart_list.cloth_image),
                                    (cloth_name = cart_list.cloth_name),
                                    (cloth_category = cart_list.cloth_category),
                                    (cloth_gender = cart_list.cloth_gender),
                                    (cloth_status = cart_list.cloth_status),
                                    (cloth_type = cart_list.cloth_type),
                                    (cloth_description = cart_list.cloth_desc),
                                    (cloth_price = cart_list.cloth_price),
                                    (cloth_size = cart_list.cloth_size),
                                    (cloth_discount = cart_list.cloth_discount),
                                    (S = cart_list.S),
                                    (M = cart_list.M),
                                    (L = cart_list.L)
                                ">
                                                            <v-icon>mdi-pencil-outline</v-icon>
                                                        </v-btn>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                        <v-spacer></v-spacer>
                                    </v-card-actions>
                                </v-card>
                            </div>
                        </div>
                    </div>
                </div>
            </v-card>
        </div>

                            </v-col>
                        </v-row>

                    </v-card>
                </v-col>
            </v-row>
        </div>
       
    </div>
    <v-dialog v-model="dialog_update" max-width="350">
        <v-card max-width="360">
            <div class="">
                <div class="container">
                    <div class="d-flex">
                        <div class="text-center container"></div>
                        <v-spacer></v-spacer>
                        <v-btn icon color="red" @click="dialog_update = false">
                            <v-icon>mdi-close</v-icon>
                        </v-btn>
                    </div>
                    <v-row class="text-center">
                        <v-col cols="12" md="12" lg="12" sm="12">
                            <uploadStock />
                        </v-col>
                    </v-row>
                </div>
            </div>
        </v-card>
    </v-dialog>
    <v-dialog v-model="dialog_delete" max-width="300">
        <v-card max-width="300">
            <div class="">
                <div class="container">
                    <div class="d-flex" style="align-items: center">
                        <v-spacer></v-spacer>
                        <div class="text-center" style="margin: 10px">
                            <v-img :src="header_cart2" width="60" height="60" cover></v-img>
                        </div>
                        <v-spacer></v-spacer>
                    </div>
                    <v-row class="text-center">
                        <V-col cols="12" md="12" lg="12" sm="12">
                            <div class="text-center container">
                                <h3 id="logout_text">Delete this item from my Stock.</h3>
                            </div>
                        </V-col>

                        <v-col cols="6" md="6" lg="6" sm="6">
                            <v-btn density="compact" color="black" text id="cart_btn" @click="dialog_delete = false">No
                            </v-btn>
                        </v-col>
                        <v-col cols="6" md="6" lg="6" sm="6">
                            <v-btn density="compact" color="red" text id="cart_btn" @click="DeleteStock2(item_id)">Yes
                            </v-btn>
                        </v-col>
                        <v-col cols="12" md="12" lg="12" sm="12" align="center" justify="center">
                            <v-progress-linear v-show="progress_bar" indeterminate color="pink"></v-progress-linear>
                        </v-col>
                    </v-row>
                </div>
            </div>
        </v-card>
    </v-dialog>
    <v-dialog v-model="dialog_delete_sold" max-width="300">
        <v-card max-width="300">
            <div class="">
                <div class="container">
                    <div class="d-flex" style="align-items: center">
                        <v-spacer></v-spacer>
                        <div class="text-center" style="margin: 10px">
                            <v-img :src="header_cart2" width="60" height="60" cover></v-img>
                        </div>
                        <v-spacer></v-spacer>
                    </div>
                    <v-row class="text-center">
                        <V-col cols="12" md="12" lg="12" sm="12">
                            <div class="text-center container">
                                <h3 id="logout_text">Delete all sold out items from my Stock.</h3>
                            </div>
                        </V-col>

                        <v-col cols="6" md="6" lg="6" sm="6">
                            <v-btn density="compact" color="black" text id="cart_btn" @click="dialog_delete_sold = false">No
                            </v-btn>
                        </v-col>
                        <v-col cols="6" md="6" lg="6" sm="6">
                            <v-btn density="compact" color="red" text id="cart_btn" @click="FetchSoldOutItemsAndDelete(), dialog_delete_sold = false">Yes
                            </v-btn>
                        </v-col>
                        <v-col cols="12" md="12" lg="12" sm="12" align="center" justify="center">
                            <v-progress-linear v-show="progress_bar" indeterminate color="pink"></v-progress-linear>
                        </v-col>
                    </v-row>
                </div>
            </div>
        </v-card>
    </v-dialog>
    <v-dialog color="white" v-model="updateItemDialog" width="800" min-width="400">
        <v-card>
            <div class="container">
                <div class="d-flex">
                    <div class="text-center container"></div>
                    <v-spacer></v-spacer>
                    <v-btn icon color="red" @click="updateItemDialog = false">
                        <v-icon>mdi-close</v-icon>
                    </v-btn>
                </div>
                <v-row>
                    <v-col cols="12" sm="12" md="12">
                        <div class="container">
                            <h2>Update Stock</h2>
                        </div>
                        <div class="">
                            <v-row>
                                <v-col cols="6" sm="6" md="6">
                                    <div>
                                        <v-img :src="cloth_image" contain height="300"></v-img>
                                    </div>
                                </v-col>
                                <v-col cols="6" sm="6" md="6">
                                    <div>
                                        <dropzone id="foo" ref="el" height="300" :options="options" @vdropzone-complete="afterCompletePoster"></dropzone>
                                        <div>
                                            <v-btn text @click="clearDropzone">Clear image</v-btn>
                                        </div>
                                    </div>
                                </v-col>
                            </v-row>

                            <div class="d-flex text-center">
                                <div>
                                    <v-chip id="cat_chip" :input-value="true" active-class="black--text">
                                        {{ cloth_category }}
                                    </v-chip>
                                </div>

                                <div>
                                    <v-chip id="cat_chip" :input-value="true" active-class="black--text">
                                        {{ cloth_type }}
                                    </v-chip>
                                </div>
                            </div>
                            <v-col cols="12" md="12" lg="12" sm="12">
                                <div class="row">
                                    <v-item v-for="cat in category" :key="cat" v-slot="{ active }">
                                        <v-chip-group row>
                                            <v-chip @click="cloth_category = cat.title" filter variant="outlined">
                                                <v-icon start icon="">{{ cat.icon }}</v-icon>
                                                {{ cat.title }}
                                            </v-chip>
                                        </v-chip-group>
                                    </v-item>
                                </div>
                            </v-col>
                            <v-form ref="form" lazy-validation>
                                <div style="padding:0px:">
                                    <v-card elevation="0" class="text-center"> </v-card>
                                    <br />
                                </div>

                                <div class="container">
                                    <h4>Select Type</h4>
                                </div>
                                <div class="row">
                                    <v-item v-for="(type, idx) in types" :key="idx" v-slot="{ active }">
                                        <v-chip-group row>
                                            <v-chip id="cat_chip" filter active-class="black--text" :input-value="active" @click="cloth_type = type.type">
                                                {{ type.type }}
                                            </v-chip>
                                        </v-chip-group>
                                    </v-item>
                                    <br />
                                </div>
                                <br />

                                <v-text-field v-model="cloth_type" :rules="nameRules2" label="Cloth Type"></v-text-field>

                                <div>
                                    <v-item-group multiple>
                                        <v-subheader>Select Brands</v-subheader>
                                        <v-item v-for="brand in brands" :key="brand" v-slot="{ active }">
                                            <v-chip id="cat_chip" active-class="black--text" :input-value="active" @click="item_name = brand.brand">
                                                {{ brand.brand }}
                                            </v-chip>
                                        </v-item>
                                    </v-item-group>
                                    <br />
                                </div>

                                <v-text-field v-model="cloth_name" :rules="nameRules2" label="Cloth Brand"></v-text-field>
                                <v-text-field v-model="cloth_size" :counter="10" :rules="nameRules2" label="Cloth size"></v-text-field>

                                <v-select :counter="10" :rules="nameRules2" v-model="cloth_status" label="Select status" :items="statusItems"></v-select>

                                <v-select :counter="10" :rules="nameRules2" v-model="cloth_gender" label="Select gender" :items="['Male', 'Female']"></v-select>
                                <v-textarea v-model="cloth_description" :rules="nameRules2" label="Description">
                                </v-textarea>

                                <v-text-field v-model="cloth_price" :counter="10" :rules="nameRules2" label="Price"></v-text-field>

                                <v-btn color="black" block class="white--text" @click="UploadItem(item_id)">
                                    Update item
                                </v-btn>
                            </v-form>
                        </div>
                    </v-col>
                </v-row>
                <br />
            </div>
        </v-card>
    </v-dialog>
    <v-snackbar color="white--text" :timeout="4000" v-model="snackbar" center>
        {{ snackbarText }}
    </v-snackbar>
    <v-snackbar color="red" :timeout="4000" v-model="snackbar2" outlined bottom center>
        {{ snackbarText2 }}
    </v-snackbar>
</div>
</template>

<script>
import Dropzone from "nuxt-dropzone";
import uploadStock from "/components/uploadStock";
export default {
    components: {
        uploadStock,
        Dropzone,
    },
    data() {
        return {
            all_stock: 0,
            sold_stock: 0,
            dialog_delete_sold: false,
            statusItems: ["New arrival", "Sale", "Best sellers", "New arrival"],
            updateItemDialog: false,
            category: [{
                    icon: "mdi-hanger",
                    title: "Clothing",
                },
                {
                    icon: "mdi-shoe-sneaker",
                    title: "Shoes",
                },
                {
                    icon: "mdi-ring",
                    title: "Accessories",
                },
                {
                    icon: "mdi-home-assistant",
                    title: "Home & Living",
                },
                {
                    icon: "mdi-sale",
                    title: "Sale",
                },
                {
                    icon: "mdi-cart-heart",
                    title: "Pre-order",
                },
            ],
            products: [],
            brands: [],
            categories: [],
            types: [],
            units: "",
            S: false,
            M: false,
            L: false,
            All: false,
            edit: false,
            switch1: true,
            item_id: null,
            scrollInvoked: 0,
            all_products: [],
            discount_state: false,
            snackbar: false,
            snackbarText: "No error message",
            snackbar2: false,
            showLogin: false,
            snackbarText2: "",
            discount_per: 0,
            dialog_delete: false,
            dialog_update: false,
            cloth_name: "",
            cloth_category: "",
            cloth_type: "",
            cloth_size: "",
            cloth_description: "",
            cloth_gender: "",
            cloth_price: "",
            cloth_status: "",
            cloth_status2: "",
            options: {
                url: "http://httpbin.org/anything",
            },
        };
    },
    methods: {
        FetchAllStockCount() {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .get()
                .then((queryResult) => {
                    queryResult.forEach((doc) => {
                        if (!doc.exists) {
                            throw "Document does not exist!";
                        }
                        console.log("order details", queryResult.size);
                        this.all_stock = queryResult.size;
                    });
                });
        },
        FetchSoldCount() {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .where("sold", "==", true)
                .get()
                .then((queryResult) => {
                    queryResult.forEach((doc) => {
                        if (!doc.exists) {
                            throw "Document does not exist!";
                        }
                        console.log("order details", queryResult.size);
                        this.sold_stock = queryResult.size;
                    });
                });
        },
        FetchSoldOutItemsAndDelete() {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .where("sold", "==", true)
                .get()
                .then((queryResult) => {
                    queryResult.forEach((doc) => {
                        if (!doc.exists) {
                            throw "Document does not exist!";
                        }
                        console.log("Delete sold out items", queryResult.size);

                        this.DeleteStock(doc.id);
                    });
                });
        },
        async DeleteStock(val) {
            const db = this.$fire.firestore;
            console.log(val);
            const docRef = db.collection("My_Stock").doc(val);

            const docSnapshot = await docRef.get();

            if (docSnapshot.exists) {
                await docRef.delete();
                this.dialog_delete_sold = false;
                this.Refresh();
            } else {
                console.warn("Document does not exist:", val);
            }
        },
        async DeleteStock2(val) {
            const db = this.$fire.firestore;
            console.log(val);
            const docRef = db.collection("My_Stock").doc(val);

            const docSnapshot = await docRef.get();

            if (docSnapshot.exists) {
                await docRef.delete();
                this.dialog_delete = false;
                this.Refresh();
            } else {
                console.warn("Document does not exist:", val);
            }
        },
        ClearInput() {
            this.cloth_category = "";
            this.cloth_type = "";
            this.posterUrl = "";
            this.cloth_name = "";
            this.cloth_size = "";
            this.cloth_gender = "";
            this.cloth_description = "";
            this.cloth_size = "";
            this.cloth_price = "";
            this.cloth_status = "";
            this.clearDropzone();
        },
        handleSuccess(file, response) {
            // Handle success event here
            console.log("File uploaded successfully!", response);
        },
        clearDropzone() {
            this.$refs.el.dropzone.removeAllFiles();
        },
        FetchTypes() {
            const db = this.$fire.firestore;
            this.types.splice(this.types);
            db.collection("Type")
                .get()
                .then((queryResult6) => {
                    queryResult6.forEach((doc) => {
                        const data = {
                            id: doc.id,
                            type: doc.data().type,
                        };

                        this.types.push(data);
                        console.log(this.types);
                    });
                });
        },
        Refresh() {
            this.FetchSoldCount();
            this.FetchAllStockCount();
            this.FetchProducts2();
            this.FetchPricing();
            this.FetchTypes();
        },
        FetchPricing() {
            const db = this.$fire.firestore;
            db.collection("Admin")
                .get()
                .then((queryResult) => {
                    queryResult.forEach((doc) => {
                        // this.dis_per = doc.data().discount;
                        this.discount_per = doc.data().discount;
                        console.log("Admin details", this.discount_per);
                    });
                });
        },
        async DeleteCartFromList(val) {
            const db = this.$fire.firestore;
            var cart_ref = db.collection("My_Stock").where("cloth_id", "==", val);
            let batch = db.batch();

            cart_ref.get().then((snapshot) => {
                snapshot.docs.forEach((doc) => {
                    batch.delete(doc.ref);
                    this.FetchProducts2();
                    this.dialog_delete = false;
                });
                return batch.commit();
            });
        },
        discount(val, val2) {
            let discount_rate = val2 / 100;
            let new_price = val * discount_rate;
            console.log("price", val, "dis", discount_rate, "new price", new_price);
            let totalPrice = val - new_price;
            return totalPrice;
        },
        DiscountLabel(val) {
            if (val == true) {
                return "Enabled";
            } else {
                return "Disabled";
            }
        },
        onScroll() {
            this.scrollInvoked++;
        },
        FetchProducts2() {
            const db = this.$fire.firestore;
            this.all_products.splice(this.all_products);
            db.collection("My_Stock")
                .get()
                .then((queryResult6) => {
                    queryResult6.forEach((doc) => {
                        const data = {
                            id: doc.id,
                            cloth_name: doc.data().cloth_brand,
                            cloth_category: doc.data().cloth_category,
                            cloth_image: doc.data().cloth_image,
                            cloth_size: doc.data().cloth_size,
                            cloth_price: doc.data().cloth_price,
                            cloth_type: doc.data().cloth_type,
                            cloth_status: doc.data().cloth_status,
                            cloth_gender: doc.data().cloth_gender,
                            cloth_description: doc.data().cloth_desc,
                            state: doc.data().state,
                            status: doc.data().status,
                            Negotiable: doc.data().Negotiable,
                            sold: doc.data().sold,
                            discount: doc.data().discount,
                            discount_state: doc.data().discount_state,
                        };

                        this.all_products.push(data);
                        console.log(this.all_products);
                    });
                });
        },
        updateStatus(val, val2) {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .doc(val2)
                .update({
                    discount: val,
                    discount_per: 60,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    if (val == true) {
                        this.snackbar = true;
                        this.snackbarText = "Enabled";
                    } else {
                        this.snackbar = true;
                        this.snackbarText = "Disabled";
                    }
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        updateStatus1(val, val2) {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .doc(val2)
                .update({
                    Negotiable: val,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    if (val == true) {
                        this.snackbar = true;
                        this.snackbarText = "Enabled";
                    } else {
                        this.snackbar = true;
                        this.snackbarText = "Disabled";
                    }
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        updateStatus2(val, val2) {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .doc(val2)
                .update({
                    status: val,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    if (val == true) {
                        this.snackbar = true;
                        this.snackbarText = "Enabled";
                    } else {
                        this.snackbar = true;
                        this.snackbarText = "Disabled";
                    }
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        updateStatus3(val, val2) {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .doc(val2)
                .update({
                    state: val,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    if (val == true) {
                        this.snackbar = true;
                        this.snackbarText = "Enabled";
                    } else {
                        this.snackbar = true;
                        this.snackbarText = "Disabled";
                    }
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        updateStatus4(val, val2) {
            const db = this.$fire.firestore;
            db.collection("My_Stock")
                .doc(val2)
                .update({
                    sold: val,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    if (val == true) {
                        this.snackbar = true;
                        this.snackbarText = "Enabled";
                    } else {
                        this.snackbar = true;
                        this.snackbarText = "Disabled";
                    }
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        updateStatus55(val, val2) {
            if (this.discount_per == 0) {
                this.snackbar2 = true;
                this.snackbarText2 = "Set a discount percentage.";
            } else {
                const db = this.$fire.firestore;
                db.collection("My_Stock")
                    .doc(val2)
                    .update({
                        discount: Number(this.discount_per),
                        discount_state: true,
                        Negotiable: false,
                    })
                    .then((docRef) => {
                        // this.dropState = true;
                        if (val == true) {
                            this.snackbar = true;
                            this.snackbarText = "Discount Enabled";
                        } else {
                            this.snackbar = true;
                            this.snackbarText = "Discount Disabled";
                        }
                    })
                    .catch((error) => {
                        console.error("Error adding document: ", error);
                        this.snackbar2 = true;
                        this.snackbarText2 = error;
                    });
            }
        },
        UploadDiscount() {
            const db = this.$fire.firestore;
            db.collection("Admin")
                .doc("pricings")
                .update({
                    discount: Number(this.discount_per),
                    discountState: true,
                })
                .then((docRef) => {
                    // this.dropState = true;
                    this.FetchPricing();
                    this.snackbar = true;
                    this.snackbarText = "Updated";
                    this.edit = false;
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                    this.snackbar2 = true;
                    this.snackbarText2 = error;
                });
        },
        UploadItem(ID) {
            if (this.cloth_category == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide category";
            } else if (this.posterUrl == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide a item image";
            } else if (this.cloth_name == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide a item name";
            } else if (this.cloth_type == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide a item type";
            } else if (this.cloth_description == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide a item description";
            } else if (this.cloth_size == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide a item size";
            } else if (this.cloth_gender == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide gender";
            } else if (this.cloth_price == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide item price";
            } else if (this.cloth_status == "") {
                this.snackbar2 = true;
                this.snackbarText2 = "Provide item status";
            } else {
                const db = this.$fire.firestore;
                db.collection("My_Stock")
                    .doc(ID)
                    .update({
                        cloth_category: this.cloth_category,
                        cloth_type: this.cloth_type,
                        cloth_image: this.cloth_image,
                        cloth_brand: this.cloth_name,
                        size: this.cloth_size,
                        cloth_gender: this.cloth_gender,
                        cloth_desc: this.cloth_description,
                        cloth_size: this.cloth_size,
                        cloth_price: Number(this.cloth_price),
                        cloth_status: this.cloth_status,
                        cloth_discount: 0,
                        cloth_id: ID,
                        item_id: ID,
                    })
                    .then((docRef) => {
                        // this.dropState = true;
                        this.snackbar = true;
                        this.snackbarText = "Updated";
                        this.updateItemDialog = false;
                        this.FetchProducts2();
                    })
                    .catch((error) => {
                        console.error("Error adding document: ", error);
                        this.snackbar2 = true;
                        this.dropState = true;
                        this.snackbarText2 = error;
                    });
            }
        },
    },
    created() {
        this.FetchSoldCount();
        this.FetchAllStockCount();
        this.FetchProducts2();
        this.FetchPricing();
        this.FetchTypes();
    },
};
</script>

<style lang="css"></style>
