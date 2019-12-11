<template>
    <el-menu :collapse="true">
        <el-menu-item>
            <i class="el-icon-menu"></i>
            <span slot="title">Dashboard</span>
        </el-menu-item>
        <el-menu-item>
            <i class="el-icon-s-custom"></i>
            <span slot="title">Clientes</span>
        </el-menu-item>
        <el-menu-item>
            <i class="el-icon-present"></i>
            <span slot="title">Productos</span>
        </el-menu-item>
        <el-menu-item>
            <i class="el-icon-truck"></i>
            <span slot="title">Unidades moviles</span>
        </el-menu-item>
    </el-menu>
</template>

<script>
export default {
    
}
</script>

<style>
.el-menu{
    min-height: 100vh;    
}
</style>