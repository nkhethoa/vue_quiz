<template>
    <div>
        <b-nav tabs>
        <b-nav-item disabled><b>Fancy Quiz app</b></b-nav-item>
        <b-nav-item disabled><b>Counter: {{numCorrect}}/{{numTot}}</b></b-nav-item>
      </b-nav>
    </div>
</template>

<script>
export default {
  props: [
     'numCorrect',
      'numTot'
  ]
}
</script>
