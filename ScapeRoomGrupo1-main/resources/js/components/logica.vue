<template>
    
	<div class="row">	
		<div class="col-xl-12 col-sm-12 col-12 text-center">
			<ul id="horizontal-list centrado">
				
				<!-- Aca va la lista de las letras digitadas -->
				<button v-for="item in palabra_escrita" type="button" class="btn btn-primary cuadro text-center"><span class="badge">{{item}}</span>
				</button> 							
				<button></button>

			</ul>	
		</div>


			<div class="container text-center">

			<div class="col-xl-12 col-sm-12 col-12">

				<!-- Aca va el teclado -->
				<span v-for="(letra,index) in letras">
				
				<button v-on:click="comparar(letra,index)" class="teclado" v-bind:key="index" v-bind:disabled="botones[index]" v-bind:class="{verde:color_botones[index]=='verde',rojo:color_botones[index]=='rojo'}">{{letra}}</button>
				</span>
							
			</div>	<!-- End segunda columna -->
			</div>	<!-- End container text-center -->
		</div>	<!-- End row -->

	<div class="row">

		<!-- <div class="col-xs-8 col-sm-8 col-md-8">
			<br>
			<div>
				<img v-bind:src="'imagenes/'+contador_errores+'.png'" alt="">
			</div>

		</div>	End tercera columna -->

		<div class="col-4 col-sm-4 col-xl-4 text-center offset-xl-4 offset-sm-4 offset-4">
			<br>
			<label class="text-primary">Aciertos:</label>
			<input type="text" class="form-control text-center" v-model="contador_aciertos" >
			<br>
			<label class="text-primary">Errores:</label>
			<input type="text" class="form-control text-center" v-model="contador_errores" size="3">
			<br><br>
            <p id="bot"></p>
            <br><br>
		</div>	<!-- En cuarta columna -->
        <br>				
	</div>	<!-- End row -->
</template>

<script>
export default{
    data(){
        return {			
            game:true,	
            win:false,
            lost:false,
            contador_aciertos:0,
            contador_errores:0,			
            aleatorio:0,			
            palabra_escrita:[],
            botones:[],
            color_botones:[],
            boton: document.getElementById("bot1"),
            letras:"ABCDEFGHIJKLMNOPQRSTUVWXYZ",
            frutas:["bacilluscereus","shigellasonnei","penicilliumspp","staphilococcusaureus","candidaalbicans","brucellaabortus","escherichiacoli","aspergillusniger","lactobacilluscasei","streptococcusthermophilus"]
        }
    },	//	End data
    methods:{	
        generarAleatorio:function (){
            this.game = true
            this.win = false
            this.lost=false
            this.palabra_escrita = []
            this.contador_aciertos = 0
            this.contador_errores = 0	
            this.botones = []
            this.color_botones = []			
            this.aleatorio = Math.floor(Math.random() * this.frutas.length)
            //	Crea un array de la misma longitud de
            for (var i=0;i< this.frutas[this.aleatorio].length;i++) {
                this.palabra_escrita.push(' ')
            }	
            
            return this.aleatorio					
        },
        siguente:function (){
            
            this.aleatorio = Math.floor(Math.random() * this.frutas.length)
            //	Crea un array de la misma longitud de
            for (var i=0;i< this.frutas[this.aleatorio].length;i++) {
                this.palabra_escrita.push(' ')
            }	
            
            return this.aleatorio					
        },										
        comparar: function(caracter,posicion){																
            
            if(this.game){

            let contadorFlag = 0

            this.botones[posicion] = true;										
            
            for(let i=0;i<=this.palabra_generada.length;i++){
                //	Si la letra se encuentra en la palabra
                if(caracter.toLowerCase()==this.palabra_generada[i]){
                 //app.$set(this.palabra_escrita, i, caracter)
                 this.palabra_escrita[i] = caracter
                 contadorFlag++
                 this.contador_aciertos++				     				    	
                }	//	End if					
            }	//	End For

                //	No se encontró la letra
                if(contadorFlag==0){
                    this.color_botones[posicion]='rojo'
                    this.contador_errores++				
                }
                else{
                    
                    this.color_botones[posicion]='verde'					
                }

            if(this.contador_aciertos==this.palabra_generada.length){
                this.win=true
                this.game = false
                var pp=document.getElementById("bot");
                var div1 = document.createElement("div");
                div1.setAttribute("style", "background-color:grey;border:4px solid green;"); 

                var text = document.createElement("h1");
                text.textContent="Ganaste";

                var a = document.createElement("a");
                a.setAttribute("href", "/juego4");

                var boton = document.createElement("button");
                boton.textContent="Continuar";
                
                pp.appendChild(div1);
                div1.appendChild(text);
                div1.appendChild(a);
                a.appendChild(boton);



            }

            if(this.contador_errores==8){
                this.lost = true
                this.game = false
                var pp=document.getElementById("bot");
                var div1 = document.createElement("div");
                div1.setAttribute("style", "background-color:grey;border:4px solid green;"); 

                var text = document.createElement("h1");
                text.textContent="Perdiste";

                var a = document.createElement("a");
                a.setAttribute("href", "/menuPrincipal");

                var boton = document.createElement("button");
                boton.textContent="Volver";
                
                pp.appendChild(div1);
                div1.appendChild(text);
                div1.appendChild(a);
                a.appendChild(boton);
                

            }
                        
          }//	End If Game
        
        }	//	End comparar				

        },	//	End methods	
    computed:{
        palabra_generada:function(){										
            return this.frutas[this.aleatorio]				
        },	//	End palabra_generada
            
    },	//	End computed
    created: function(){			
        this.generarAleatorio()				
    }
//	End vue
}
</script>