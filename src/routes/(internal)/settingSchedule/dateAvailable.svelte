<script lang="ts">
    import { AccordionItem, Accordion } from 'flowbite-svelte'
    import type { IGetDateAvailableCab } from 'src/models/interfaces';
    import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
    import { Circle} from 'svelte-loading-spinners';
    import { AlertService } from '../../../service/alerts';   

    let alerts = new AlertService();

    let dataDateAvailable:IGetDateAvailableCab[]=[];

    async function getDateAvailable()
    {    
      let idStaff=localStorage.getItem("Staff")
      let idLocation=localStorage.getItem("Location")

      const getHour = await fetch(`https://localhost:7112/api/SettingScheduleCtrl/GetDateAvailable/${idStaff}/${idLocation}`, 
      {
          method: 'GET',              
      });

        dataDateAvailable =await getHour.json();      
        console.log(dataDateAvailable);
        
    }

    async function delteHourByDay(idAvailable:number,idHour:number,date:string) {
      let idStaff=localStorage.getItem("Staff")
      let idLocation=localStorage.getItem("Location")
      
      const getHourByDay = await fetch(`https://localhost:7112/api/SettingScheduleCtrl/DeleteHourByDay/${idStaff}/${idLocation}/${idAvailable}/${idHour}/${date}`, 
      {
          method: 'DELETE',              
      });
      
      const content = await getHourByDay.text();

      if(parseInt(content) == 1 ){
        
        alerts.ShowSwalBasicWarning("Advertencia","Este día ya tiene una cita asignada") 

      }else{
        alerts.ShowSwalBasicSuccess("Correcto","Eliminación exitosa") 
        getDateAvailable();
      }


    }
    
  

</script>

{#await getDateAvailable()}

    <div class="flex flex-wrap justify-center  text-center p-4">    
            <Circle size="100" color="#FF3E00" unit="px" duration="1s" />    
    </div>                      
              
  {:then res} 

    <div class="rounded-xl border p-5 shadow-md w-12/12 bg-white  ">

      <div class="flex w-full items-center justify-center border-b pb-3">
        <div class="flex items-center">        
            <div class="text-lg font-bold">Fechas con disponibilidad asignada</div>                         
        </div>            
      </div>
  
      <div class=" md:flex flex-wrap mt-5 ">
        {#each dataDateAvailable as date}

        <div class=" w-12/12 md:w-3/12 mt-2  border-gray-200 dark:border-gray-700 text-center">

            <Accordion>
              <AccordionItem>
                <span slot="header">{date.date}</span>
                <Table shadow color="default" hoverable={true}>
                  <TableHead>
                    <TableHeadCell><div class="flex justify-center">Hora</div></TableHeadCell>
                    <TableHeadCell><div class="flex justify-center">Acción</div></TableHeadCell>                    
                  </TableHead>
                    {#each date.dateAvailableLin as hours}                   
                     <TableBody >
                       <TableBodyRow>
                         <TableBodyCell><div class="flex justify-center">{hours.descriptionHour}</div></TableBodyCell>     
                         <TableBodyCell>
                          <div class="flex justify-center">
                          <button on:click={()=>delteHourByDay(hours.idRowsAvailable,hours.idRowsHour,date.date)}>
                            <i  style="cursor: pointer;" class="fas fa-trash-alt text-center fa-1x hover:text-gray-500"></i>
                          </button>
                          </div>
                        </TableBodyCell>                      
                       </TableBodyRow>
                      </TableBody>
                    {/each}
                   </Table>
                
              </AccordionItem>                
            </Accordion>

        </div>

        {/each}
      </div>
      
    </div>
                
{/await} 

  
   




