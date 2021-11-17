<template>

<!--This is Albums by User-->

<div>
   The id is {{$route.params.albums}}

   {{albumsByIdData}}

   {{alertFired}}
</div>

</template>

<script>

export default {

  data() {
    return {
      albumsByIdData: [],
      alertFired: null
    }
  }

}
</script>
