<template>

<div>
<h1>Promises testing</h1>
{{newArr}}
</div>

</template>

<script>


export default {

  data() {
    return {

      arr: ['test', 'test2', 'test3'],
      newArr: []


    }
  }, computed: {



  },

  // Async getting all the API calls

  created() {

    this.delayedIterator(this.arr);


  }, updated() {

  },

  mounted() {



  },

  methods: {

      concatenator(y) {

        return new Promise ((resolve) => {
          setTimeout(() => {

            const added = 'add ' + y;

            resolve (added);

          }, 500)
        })

      },

      async delayedIterator(x) {

        for (const elem of x) {
          const newElem = await this.concatenator(elem);
          this.newArr.push(newElem)
        }
      }

    }


    // https://jsonplaceholder.typicode.com/albums/1/photos
    // https://jsonplaceholder.typicode.com/albums/1/photos

}


</script>
