<template>
    <div>
        <el-row>
            <el-col :span="20">
                <div>
                    <h1>Clientes</h1>
                </div>
                <div>
                    <el-table ref="singleTable" :data="customers" style="width: 100%" highlight-current-row @current-change="handleCurrentChange">
                        <el-table-column
                            prop="type"
                            label="Tipo"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            prop="name"
                            label="Nombre"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            prop="document"
                            label="Documento"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            prop="address"
                            label="Dirección">
                        </el-table-column>
                        <el-table-column type="expand">
                            <template slot-scope="props">
                                <h4>Ordenes</h4>
                                <p>Documento: {{ props.row.document }}</p>
                                <p>Nombre: {{ props.row.name }}</p>
                                <p>Dirección: {{ props.row.address }}</p>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
            </el-col>
            <el-col :span="4">
                <div>
                    <h2>Estadísticas</h2>
                </div>
                <div>
                    <span v-if="finded">{{ currentStadistic.total_pedidos }}</span>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
export default {
    data () {
        return {
            customers: [{
                    id: 1,
                    type: 'Jurídico',
                    name: 'Elbin',
                    document: '44957819',
                    address: 'Pj 20 de setiembre #214',
                    phone: '987300897',
                    email: 'elbingr@gmail.com',
                    line_credit: 200,
                    discount: 10
                },
                {
                    id: 2,
                    type: 'Natural',
                    name: 'Abigail',
                    document: '42957819',
                    address: 'Av. america 200999',
                    phone: '987300897',
                    email: 'abi@gmail.com',
                    line_credit: 200,
                    discount: 10
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
                    pedidos_cancelados: 5,
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
            finded: false
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
</style>