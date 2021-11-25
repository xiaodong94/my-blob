   async function async1() {
      console.log('1');
      await async2();
      console.log('2');
   }

   async function async2() {
      console.log('3');
   }

   console.log('4');

   async1();

   new Promise(function(resolve) {
      console.log('5');
      resolve();
      throw new Error('error')
      console.log('6');
   }).then(function() {
      console.log('7');
   }).catch(function(){
      console.log('error');
   });

   console.log('8');




