  <script lang="ts">
    import { Tabs, TabItem } from 'flowbite-svelte';
    import Timetable from './timetable.svelte';  
    import DateAvailable from './dateAvailable.svelte';    
    import type { IGetHour} from '../../../models/interfaces'   
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    //Verificamos qu el usuario si se haya loggeado correctamente
    onMount(async()=>{

      let Staff = localStorage.getItem("Staff")
      if(Staff=="" || Staff==null || Staff==undefined)
      {
        goto("/login")
        //console.log("No");        
      }
    })
  /*   $:{
      debugger
      
    } */
    
  </script>  

  <Tabs style="underline" class="flex justify-center container m-auto mt-2">
    <TabItem  open>
      <div slot="title">
            <i class="fas fa-user-clock fa-2x"></i>       
            <div>
              Configuración     
            </div> 
            <div>
              Disponibilidad     
            </div> 
      </div>    

      <div class="container m-auto">
         <Timetable />       
      </div>
      
    </TabItem>

    <TabItem >
      <div slot="title">
          <i class="fas fa-calendar-week fa-2x"></i>    
            <div>
              Fechas
            </div> 
            <div>
              Disponibilidad     
            </div> 
      </div>    

      <div class="container m-auto">
         <DateAvailable />       
      </div>
      
    </TabItem>

    <TabItem >
      <div slot="title" class="">
        <i class="fas fa-clock fa-2x"></i>
        <div>
          Citas 
        </div>
        <div>
          Ocupadas
        </div>
      </div>
      <p class="text-sm text-gray-500 dark:text-gray-400"><b>Dashboard:</b> Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
    </TabItem>  
   
  </Tabs>


  