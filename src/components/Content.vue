<template>
    <div>
        <el-row>
            <el-col :span="20">
                <div>
                    <h1 class="negrita title-section">
                        Clientes
                        <button class="button-new"  @click="drawer = true">
                            <i class="el-icon-plus"></i> Nuevo Cliente
                        </button>
                    </h1>
                </div>
                <div>
                    <span class="label-activos">ACTIVOS</span>
                    <el-table ref="singleTable" :data="customers" style="width: 97%" highlight-current-row stripe
                    @current-change="handleCurrentChange"
                    @expand-change="handleExpandChange" class="table-clientes">
                        <el-table-column
                            prop="type"
                            label="Tipo"
                            width="90" class-name="cell-cliente">
                        </el-table-column>                        
                        <el-table-column
                            prop="name"
                            label="Nombre" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            label="Rating" class-name="cell-cliente">
                            <template slot-scope="scope">
                                <i :class="'el-icon-star-on rating ' + scope.row.clasification"></i>
                                <span style="margin-left: 10px">{{ scope.row.rating }}</span>
                            </template>                            
                        </el-table-column>
                        <el-table-column
                            prop="type_document"
                            label="T. Documento" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            prop="document"
                            label="Documento" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            prop="address"
                            label="Dirección" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            prop="phone"
                            label="Teléfono"
                            width="110" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            prop="email"
                            label="E-mail" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column
                            prop="line_credit"
                            label="Crédito"
                            width="135" class-name="cell-cliente">
                        </el-table-column>
                        <el-table-column type="expand">
                            <template slot-scope="props">
                                <div class="sombra">
                                    <div class="detail-orders">
                                        <span class="detail-title">Ordenes</span>
                                        <table class="order-detail">
                                            <tbody>
                                                <tr v-for="item in props.row.orders" v-bind:key="item.number" @click="getProducts(item.number)">
                                                    <td>{{ item.number }}</td>
                                                    <td> S/ {{ item.amount }}</td>
                                                    <td>{{ item.date }}</td>
                                                    <td><span :class="item.status.toLowerCase()">{{ item.status }}</span></td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="products" v-if="viewProduct">
                                        <div><span class="detail-products-title">{{ titleProducts }}</span></div>                                        
                                        <div class="detail-products">
                                            <el-row :gutter="10">
                                                <el-col :span="6" v-for="product in products" v-bind:key="product.id" class="detail-product-item">
                                                    <el-badge :value="product.quantity" class="item">
                                                        <el-card class="box-card" shadow="never" > 
                                                            <img :src="product.image" class="image">
                                                            <div class="text-center">
                                                                <span>{{ product.name }}</span>
                                                            </div>
                                                        </el-card>
                                                    </el-badge>                         
                                                </el-col>
                                            </el-row>
                                        </div>                                        
                                    </div>
                                </div>                                                          
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-col>
            <el-col :span="4">
                <div class="stadistic-section">
                    <div class="stadistic-title">
                        <span>RESUMEN</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Total de pedidos</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.total_pedidos }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Bidones no devueltos</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.bidones_no_devueltos }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Saldo a favor</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.saldo_favor }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Deuda</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.deuda }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Pedidos en la semana</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.pedidos_semana }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Pedidos en el mes</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.pedidos_mes }}</span>
                    </div>
                    <div class="stadistic-item">
                        <span class="stadistic stadistic-item-title">Pedidos cancelados</span>
                        <span v-if="finded" class="stadistic stadistic-item-number">{{ currentStadistic.pedidos_cancelados }}</span>
                    </div>
                </div>                
            </el-col>
        </el-row>
        <el-drawer
            title="Registro de nuevo cliente"
            :visible.sync="drawer"
            :direction="direction"
            size="40%">
            <div class="form-new">
                <el-form label-width="130px">
                    <el-form-item label="Tipo">
                        <el-select v-model="customers.type" placeholder="Seleccione el tipo de cliente">
                            <el-option label="Jurídico" value="Jurídico"></el-option>
                            <el-option label="Natural" value="Natural"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="Nombre">
                        <el-input v-model="customers.name"></el-input>
                    </el-form-item>
                    <el-form-item label="DNI">
                        <el-input v-model="customers.document"></el-input>
                    </el-form-item>
                    <el-form-item label="Dirección">
                        <el-input v-model="customers.address"></el-input>
                    </el-form-item>
                    <el-form-item label="Teléfono">
                        <el-input v-model="customers.font"></el-input>
                    </el-form-item>
                    <el-form-item label="E-mail">
                        <el-input v-model="customers.email"></el-input>
                    </el-form-item>
                    <el-form-item label="Linea de crédito">
                        <el-input v-model="customers.line_credit"></el-input>
                    </el-form-item>
                    <el-form-item label="Descuento">
                        <el-input v-model="customers.discount"></el-input>
                    </el-form-item>
                    <div class="demo-drawer__footer float-r">
                        <el-button @click="drawer = false">Cancelar</el-button>
                        <el-button type="primary" @click="addCustomer">Registrar</el-button>
                    </div>
                </el-form>
                
            </div>
        </el-drawer>
    </div>
</template>

<script>
export default {
    data () {
        return {
            drawer: false,
            direction: 'rtl',
            customers: [{
                    id: 1,
                    type: 'Jurídico',
                    name: 'Elbin Flores',
                    type_document: 'RUC',
                    document: '12345678901',
                    address: 'Pj 20 de setiembre #214',
                    phone: '987300897',
                    email: 'elbingr@gmail.com',
                    line_credit: 200,
                    discount: 10,
                    rating: 10,
                    clasification: 'gold',
                    orders: [
                        {
                            number: '#00100',
                            amount: 120.00,
                            date: '09/12/2019',
                            status: 'Registrado'
                        },
                        {
                            number: '#00099',
                            amount: 100.00,
                            date: '08/12/2019',
                            status: 'Pagado'
                        },
                        {
                            number: '#00098',
                            amount: 80.00,
                            date: '07/12/2019',
                            status: 'Entregado'
                        }
                    ]
                },
                {
                    id: 2,
                    type: 'Natural',
                    name: 'Juan Perez',
                    type_document: 'DNI',
                    document: '42957819',
                    address: 'Av. america 200999',
                    phone: '987300897',
                    email: 'juan@gmail.com',
                    line_credit: 200,
                    discount: 10,
                    rating: 4,
                    clasification: 'silver',
                    orders: [
                        {
                            number: '#00089',
                            amount: 50.00,
                            date: '09/12/2019',
                            status: 'Registrado'
                        },
                        {
                            number: '#00088',
                            amount: 100.00,
                            date: '08/12/2019',
                            status: 'Pagado'
                        },
                        {
                            number: '#00098',
                            amount: 80.00,
                            date: '07/12/2019',
                            status: 'Entregado'
                        }
                    ]
                }
            ],
            currentRow: null,
            stadistics: [
                {
                    customer_id : 1,
                    total_pedidos: 100,
                    bidones_no_devueltos: 23,
                    saldo_favor: 250.00,
                    deuda: 120,
                    pedidos_semana: 10,
                    pedidos_mes: 250,
                    pedidos_cancelados: 8,
                    ranking: 5
                },
                {
                    customer_id : 2,
                    total_pedidos: 200,
                    bidones_no_devueltos: 15,
                    saldo_favor: 120.00,
                    deuda: 50,
                    pedidos_semana: 22,
                    pedidos_mes: 360,
                    pedidos_cancelados: 5,
                    ranking: 5
                }
            ],
            currentStadistic: null,
            finded: false,
            viewProduct: false,
            titleProducts: '',
            products: [
                {
                    id: 1,
                    name: 'Bidón',
                    quantity: 2,
                    image: 'images/products/producto1.jpg'
                },
                {
                    id: 2,
                    name: 'Laptop',
                    quantity: 1,
                    image: 'images/products/producto2.jpg'
                },
                {
                    id: 3,
                    name: 'Teclado',
                    quantity: 3,
                    image: 'images/products/producto3.jpg'
                },
                {
                    id: 1,
                    name: 'Bidón',
                    quantity: 2,
                    image: 'images/products/producto1.jpg'
                },
                {
                    id: 2,
                    name: 'Laptop',
                    quantity: 1,
                    image: 'images/products/producto2.jpg'
                },
                {
                    id: 3,
                    name: 'Teclado',
                    quantity: 3,
                    image: 'images/products/producto3.jpg'
                },
                {
                    id: 1,
                    name: 'Bidón',
                    quantity: 2,
                    image: 'images/products/producto1.jpg'
                },
                {
                    id: 2,
                    name: 'Laptop',
                    quantity: 1,
                    image: 'images/products/producto2.jpg'
                },
                {
                    id: 3,
                    name: 'Teclado',
                    quantity: 3,
                    image: 'images/products/producto3.jpg'
                },
                {
                    id: 1,
                    name: 'Bidón',
                    quantity: 2,
                    image: 'images/products/producto1.jpg'
                },
                {
                    id: 2,
                    name: 'Laptop',
                    quantity: 1,
                    image: 'images/products/producto2.jpg'
                },
                {
                    id: 3,
                    name: 'Teclado',
                    quantity: 3,
                    image: 'images/products/producto3.jpg'
                }
            ]
        }        
    },
    methods: {
      setCurrent(row) {
        this.$refs.singleTable.setCurrentRow(row);
      },
      handleCurrentChange(val) {
        this.currentRow = val;
        this.finded = true;
        this.currentStadistic = this.stadistics.find(stadistic => stadistic.customer_id == this.currentRow.id);
      },
      handleExpandChange(){
          this.viewProduct = false;
      },
      addCustomer: function (e) {
            e.preventDefault();
            this.customers.push({
                type: this.customers.type,
                name: this.customers.name,
                document: this.customers.document,
                address: this.customers.address,
                phone: this.customers.phone,
                email: this.customers.email,
                line_credit: this.customers.line_credit,
                discount: this.customers.discount,
            });
            this.customers.type = '';
            this.customers.name = '';
            this.customers.document = '';
            this.customers.address = '';
            this.customers.phone = '';
            this.customers.email = '';
            this.customers.line_credit = '';
            this.customers.discount = '';
            this.drawer = false;
        },
        getProducts: function(orderNumber){
            this.viewProduct = true;
            this.titleProducts = 'Productos en orden ' + orderNumber;
        }
    }
}
</script>

<style>
  .el-col {
    border-radius: 4px;
  }
.bg-purple {
    background: #d3dce6;
  }

  .stadistic-section{
      background-color: #DBEAF1;
      padding: 10px;
      min-height: 90vh;
  }

  .stadistic-title{
      color: #5C73F2;
      margin-bottom: 15px;
      margin-top: 25px;
      font-weight: 800;
    font-size: .8em;
  }

  .stadistic{
      padding: 3px;
      text-align: left;
  }

  .stadistic-item{
      margin-bottom: 12px;
  }

  .stadistic-item-title{
      display: block;
      font-size: .9em;
      font-weight: bold;
      color: #6E6B6B;
  }

  .stadistic-item-number{
      display: block;
      font-weight: bold;
      font-size: 1.3em;
      color: #000000;
  }
</style>