<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Monte o seu açaí:</h1>
      <acaiForm />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import AcaiForm from '../components/AcaiForm'

export default {
  name: 'Home',
  components: {
    Banner,
    AcaiForm
  }
}
</script>