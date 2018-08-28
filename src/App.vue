<template>
<div id="app">
  <ValidateForm />
  <ProductList/>
</div>
</template>

<script>

import ValidateForm from './components/ValidateForm'
import ProductList from './components/ProductList'

export default {
  name: 'App',
  components: {
    ValidateForm,
    ProductList
  }
}
</script>

<style lang="scss">

</style>
