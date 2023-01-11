<script lang="ts">    
    import { Checkbox } from 'flowbite-svelte';    
    import { Alert } from 'flowbite-svelte';
    import type { IGetHour} from '../../../models/interfaces'
    import { Circle} from 'svelte-loading-spinners';
    import { AlertService } from '../../../service/alerts';       
    
    let dateSelected:string="";
    let viewHours:boolean=false;       
    let dataHour:IGetHour[]=[];
    let hourSelected:number[]=[];

    let checkAllHour:boolean=false

    let alerts = new AlertService();

    $:dateSelected ==""? viewHours=false:viewHours=true; 
    

    //Obtenemos las horas para que sean seleccionadas
    async function getHours(){  

        let idStaff=localStorage.getItem("Staff")
        let idLocation=localStorage.getItem("Location")

        dataHour =[];       

        const getHour = await fetch(`https://andresmu91.bsite.net/api/SettingScheduleCtrl/GetHour/${dateSelected}/${idStaff}/${idLocation}`, 
        {
            method: 'GET',              
        });

        dataHour =await getHour.json();                     

        //console.log(dataHour);
           
    }       

    //Enviamos la configuración de las horas con sus respectivas validaciones
    async function setHoursAvailable(){

        let dataIdRowhour:any=[];
        let data:any=[];
        hourSelected=[];
        console.log(dataHour);
     
        dataHour.forEach(e=>{
            //debugger
            if(e.selected==true){
                hourSelected.push(e.idRowsHour)
                
            }
        })

        
        if(dataHour.length>0 ){
            debugger

            hourSelected.forEach(element => {
                
                dataIdRowhour.push({
                    idRows:element
                })
            });

            data.push({
                date:dateSelected,
                idStaff:localStorage.getItem("Staff"),
                idLocation:localStorage.getItem("Location"),
                hours:dataIdRowhour
            })

            console.log(data);
            

           const setdataHours=await fetch(`https://andresmu91.bsite.net/api/SettingScheduleCtrl/SetHour`, 
            {
                method: 'POST',          
                headers: { 
                'Content-Type': 'application/json' ,
                'Accept': 'application/json',
            },    
                body: JSON.stringify(data)                
            });

            if(setdataHours.ok){
                
                const content = await setdataHours.text();

               /*  if(parseInt(content) ==1 ){

                    alerts.ShowSwalBasicWarning("Advertencia","Este día ya tiene un horario configurado") 
                    
                }else{ */
                    alerts.ShowSwalBasicSuccess("Correcto","Programación asignada correctamente")
                    hourSelected=[];
                    dataHour =[];
                    dateSelected="";
                    checkAllHour=false;
              //  }                

            }else{                
                alerts.ShowSwalBasicError("Error","No se pudo asignar la programación")                
            }

        }else{
            alerts.ShowSwalBasicWarning("Advertencia","Por favor seleccionar algún horario") 
        }                   

    }  

    function selectChecked(idRowsHour:number){

        dataHour.forEach(e=>{
            if(e.idRowsHour == idRowsHour){
                e.selected==true? e.selected=false : e.selected=true
            }
        })

    }
    //En dado caso que se quiera seleccionar todas las horas
   function checkAll(){
        debugger
        checkAllHour==false? checkAllHour=true : checkAllHour=false
        dataHour.forEach(e=>{     
            if(e.idAvailable==0){
              checkAllHour==true? e.selected=true : e.selected=false              
            }
        })

        dataHour=dataHour;               
    }



</script>

<!--HTML-->
<div class="career-form mb-2">

    <!--Selección de la fecha para asignarle las horas-->
    <div  class="">
        <div class="py-2 mx-auto w-12/12 md:w-4/12 lg:w-4/12 ">
            <label for="first_name" class="text-center block mb-2 text-lg font-bold dark:text-gray-300 ">Fecha asignación horario</label>
            <input on:change={getHours} bind:value={dateSelected} placeholder="Seleccionar una hora" type="date" class="bg-gray-50 border  text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700  dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" >            
        </div>       
    </div>
     
    {#if viewHours}      
        
        {#if dataHour.length==0}
            <div class="flex flex-wrap justify-center  text-center p-4">    
                <Circle size="100" color="#FF3E00" unit="px" duration="1s" />    
            </div>              
            
            {:else}      

            <div class='flex items-center justify-center'>  
                <div class="rounded-xl border p-5 shadow-md w-12/12 bg-white mb-3">
        
                    <div class="flex w-full items-center justify-center border-b pb-3">
                        <div class=" items-center">        
                            <div class="text-lg font-bold">Horarios</div>                         
                        </div>           

                                 
                    </div>
                    <!--Disponibilidad-->
                    <div class="grid grid-cols-1 ">
                        <div class="flex justify-center mt-2">
                            <div class=" items-center">        
                                <Checkbox checked={checkAllHour} on:change={()=>checkAll()}>Todas las horas</Checkbox>
                            </div>
                        </div>  
                        <div class="mb-3 text-xl font-bold p-1">
                                                                               
                               <!-- <h5 class="text-center mb-2 mt-2 text-xl font-bold text-gray-900 dark:text-white">Disponibilidad</h5>    -->
                               <div class="flex flex-wrap mt-4"> 
                                       {#each dataHour as hours}
                                            <div class="w-4/12 md:w-2/12 rounded border border-gray-200 dark:border-gray-700 text-center">
                                                <Checkbox on:change={()=>selectChecked(hours.idRowsHour)} checked={hours.selected==true } disabled={hours.idAvailable!=0} name="hourSelected" class="w-full p-4" value="{hours.idRowsHour}">{hours.description}</Checkbox>
                                            </div>
                                       {/each}    
                                       
                               </div>
   
                               <!-- Podemos ver los horarios seleccionados(el ID de las horas) {hourSelected} -->                       
                                       
                           <div class="flex items-center justify-center mt-2"> 
                               <button             
                                 on:click={setHoursAvailable}     
                                 type="submit"
                                 class="inline-block rounded-lg bg-blue-500 px-5 py-3 text-sm font-medium text-white">
                                 Guardar
                               </button>
                           </div>
                       </div>                 
        
                    </div>   
   
              </div>
   
            </div>
                      

        {/if}             
                    
            
                    
    {:else }
        <div class="p-2">
            <Alert accent >
                <span slot="icon"><svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"></path></svg></span>
                <span class="font-medium">Por favor seleccionar una fecha.</span> 
            </Alert>
        </div>
    {/if}    
    
</div>





<style >
    .career-form {
    background-color: #E1E5E8;
    border-radius: 5px;
    padding: 0 16px;
    }   
</style>