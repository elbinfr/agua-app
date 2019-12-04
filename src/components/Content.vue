<template>
    <div>
        <el-row>
            <el-col :span="20">
                <div>
                    <h1>Clientes</h1>
                </div>
                <div>
                    <el-table ref="singleTable" :data="customers" style="width: 97%" highlight-current-row @current-change="handleCurrentChange">
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

  .stadistic-section{
      background-color: #DBEAF1;
      padding: 10px;
      border-radius: 5px;
      height: 100%;
  }

  .stadistic-title{
      color: #035D8F;
      font-size: .9em;
      margin-bottom: 15px;
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
      font-size: .8em;
      font-weight: 500;
  }

  .stadistic-item-number{
      display: block;
      font-weight: bold;
      font-size: 1.3em;
  }
</style>