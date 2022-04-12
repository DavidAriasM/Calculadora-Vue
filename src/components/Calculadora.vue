<template>
  <div class="calculadora">
      <div class="calculo">{{ calculo || 0 }}</div>
      <div class="resultados">{{ total || 0 }}</div>
      <div class="btn" @click="limpiar"><span>C</span></div>
      <div class="btn" @click="signo"><span>+/-</span></div>
      <div class="btn" @click="porcentaje"><span>%</span></div>
      <div class="btn operador" @click="dividir"><span>/</span></div>
      <div class="btn" @click="agregar(7)"><span>7</span></div>
      <div class="btn" @click="agregar(8)"><span>8</span></div>
      <div class="btn" @click="agregar(9)"><span>9</span></div>
      <div class="btn operador" @click="multiplicar"><span>X</span></div>
      <div class="btn" @click="agregar(4)"><span>4</span></div>
      <div class="btn" @click="agregar(5)"><span>5</span></div>
      <div class="btn" @click="agregar(6)"><span>6</span></div>
      <div class="btn operador" @click="restar"><span>-</span></div>
      <div class="btn" @click="agregar(1)"><span>1</span></div>
      <div class="btn" @click="agregar(2)"><span>2</span></div>
      <div class="btn" @click="agregar(3)"><span>3</span></div>
      <div class="btn operador" @click="sumar"><span>+</span></div>
      <div class="btn cero" @click="agregar(0)"><span>0</span></div>
      <div class="btn" @click="puntoDecimal"><span>.</span></div>
      <div class="btn operadorTotal" @click="resultado"><span>=</span></div>
  </div>
</template>
<script>
export default {
    data(){
        return {
            calculo: '',
            total: '',
            previo: null,
            operacion: null,
            operacionClickeada: false
        }
    },
    methods:{
        limpiar: function(){
            this.total = '';
            this.calculo = '';
        },
        signo: function(){
            this.total = this.total.charAt(0) === '-' ? this.total.slice(1) : `-${ this.total }`;
            this.calculo = this.total;
        },
        porcentaje: function(){
            this.total = `${ parseFloat(this.total) / 100 }`;
        },
        agregar: function(valor){
            if(this.operacionClickeada){
                this.total = ''
                this.operacionClickeada = false;
            }
            this.calculo += valor;
            this.total = this.total + valor;
            console.log(this.total);
        },
        obtenerPrevio: function(){
            this.previo = this.total;
            this.operacionClickeada = true;
        },
        dividir: function(){
            this.operacion = (a,b) => a / b;
            this.calculo += '/';
            this.obtenerPrevio();
        },
        multiplicar: function(){
            this.operacion = (a,b) => a * b;
            this.calculo += '*';
            this.obtenerPrevio();
        },
        restar: function(){
            this.operacion = (a,b) => (a - b) * -1;
            this.calculo += '-';
            this.obtenerPrevio();
        },
        sumar: function(){
            this.operacion = (a,b) => a + b;     
             this.calculo += '+';
            this.obtenerPrevio();
        },
        puntoDecimal: function(){
            if(this.total.indexOf('.') === -1){
                this.agregar('.'); 
            }
        },
        resultado: function(){
            this.total = `${this.operacion(parseFloat(this.total),parseFloat(this.previo))}`;
            this.previo = null;
            this.calculo = this.total;
        }
    }
}
</script>
<style scoped>
.calculadora{
    width: 400px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    font-size: 40px;
    transform: translateY(40%);
}
.operacionesPrevias{
    background: rgb(255, 255, 255);
    height: 100px;
    color: rgb(194, 194, 194);
    grid-column: 1/5;
    text-align: right;
    padding-right: 10px;
    padding-top: 10px;
    overflow-x: auto;
    font-size: 30px;
}
.calculo{
    height: 100px;
    background: rgb(255, 255, 255);
    color: rgb(134, 134, 134);
    grid-column: 1/5;
    padding: 10px 10px 10px 10px;
    overflow-x: auto;
    text-align: right;
    font-size: 25px;
    word-wrap: break-word;
    border-bottom: 1px solid #bebebe;
}
.resultados{
    height: 50px;
    background: rgb(255, 255, 255);
    color: rgb(41, 41, 41);
    grid-column: 1/5;
    padding: 10px 10px 10px 10px;
    overflow-x: auto;
    text-align: center;
    word-wrap: break-word;
}

.cero{
    grid-column: 1/3;
}

.btn{
    height: 70px;
    background-color: rgb(248, 248, 248);
    cursor: pointer;
    font-size: 25px;
    transition: .3s;
}
.btn:hover{
    background-color: rgb(235, 235, 235);
}

span{
    line-height: 70px;
}

.operador{
    background: rgb(202, 202, 201);
    color: #fff;
}
.operador:hover{
    background: rgb(179, 179, 179);
}
.operadorTotal{
    background: rgb(67, 167, 105);
    color: #fff;
}

.operadorTotal:hover{
    background: rgb(78, 189, 121);
}
</style>