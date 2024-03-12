<script setup>
    import {ref, reactive, onMounted, watch} from  'vue'
    import {db} from './data/guitarras'
    import guitarra from './components/guitarra.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'


    /*const state = reactive({
        guitarras: [db]
    })
            codigo con reactive
    console.log (state.guitarras)*/
    const carrito = ref([])
    const guitarraModelo = ref ({})
    const guitarras = ref([db])

    watch(carrito, ()=> {
        guardarLocalStorage()
    },{
        deep:true 
    })
    onMounted(()=> {
        guitarras.value = db
        guitarraModelo.value = db[3]

        const carritoStorage = localStorage.getItem('carrito')
        if(carrito.value){
            carrito.value = JSON.parse(carritoStorage)
        }
    })
    
    const guardarLocalStorage = () => {
        localStorage.setItem('carrito',JSON.stringify(carrito.value))
    }

    
    const agregarCarrito = (guitarra)=> {
       const existeCarrito = carrito.value.findIndex(producto=> producto.id === guitarra.id)

     if(existeCarrito>=0){
        carrito.value[existeCarrito].cantidad++
     }else{
        guitarra.cantidad = 1
        carrito.value.push(guitarra)
     }
        
    }
    const decrementarCantidad=(id)=>{
        const index = carrito.value.findIndex(producto=> producto.id === id)
        carrito.value[index].cantidad--
    }
    const aumentarCantidad=(id)=>{
        const index = carrito.value.findIndex(producto=> producto.id === id)
         carrito.value[index].cantidad++
    }
    const eliminarProducto =(id =>{
       carrito.value = carrito.value.filter(producto=>producto.id !==id)

    })
    const eliminarCarrito = () => {
    carrito.value=[]
}

</script>

<template>
    <Header
    :carrito="carrito"
    :guitarraModelo="guitarraModelo"
    @aumentar-cantidad="aumentarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @eliminar-carrito="eliminarCarrito"
    />


    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">

            <guitarra
            v-for="guitarra in guitarras"
            :guitarra ="guitarra"
            @agregarCarrito="agregarCarrito"
            />
           

            
        </div>
    </main>


   <Footer/>

</template>

<style >
 

</style>
