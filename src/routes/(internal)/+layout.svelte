<script lang="ts">
    let mainMenu:boolean=false;
    function mainMenuBar(){
        mainMenu= true;
    }
  
    function closeMenuBurger(){    
        mainMenu=false;
    }
  
  </script>
  
  <div class="bg-blue-500">
    <nav class="relative px-4 py-2 flex justify-between items-center bg-white">
        <a class="text-3xl font-bold leading-none" href="" >
            <img style="height: 70px;"  src="" alt="">
        </a>
        <div class="lg:hidden mt-6" >
            <button on:click={mainMenuBar} class=" flex items-center text-blue-600 p-3" >
                <svg class="block h-4 w-4 fill-current" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                    <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"></path>
                </svg>
            </button>
        </div>
        <ul class="hidden absolute top-1/2 left-1/2 transform -translate-y-1/2 -translate-x-1/2 lg:flex lg:mx-auto lg:flex lg:items-center lg:w-auto lg:space-x-6">
            <li><a class="text-sm text-gray-400 hover:text-orange-500" href="/settingSchedule">Horarios</a></li>
            <li><a class="text-sm text-gray-400 hover:text-orange-500" href="/setting">Recepción tejeduría</a></li>
            <li><a class="text-sm text-gray-400 hover:text-orange-500" href="/setting">Movimiento tela</a></li>
            <li><a class="text-sm text-gray-400 hover:text-orange-500" href="reports">Reporte</a></li>
        </ul>
    </nav>
    {#if mainMenu}
    
    <div class="navbar-menu relative z-50 " >
        <div class="navbar-backdrop fixed inset-0 bg-gray-800 opacity-25"></div>
        <nav class="fixed top-0 left-0 bottom-0 flex flex-col w-5/6 max-w-sm py-6 px-6 bg-white border-r overflow-y-auto">
            <div class="flex items-center mb-8">
                <a on:click={closeMenuBurger} class="mr-auto text-3xl font-bold leading-none" href="#">
                    <a class="text-3xl font-bold leading-none" href="" >
                        <img style="height: 70px;"  src="" alt="">
                    </a>
                </a>
                <button on:click={closeMenuBurger}>
                    <svg class="h-6 w-6 text-gray-400 cursor-pointer hover:text-gray-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
            <div>
                <ul>
                    <li class="mb-1">
                        <a on:click={closeMenuBurger} class="block p-4 text-sm font-semibold text-gray-400 hover:bg-blue-50 hover:text-blue-600 rounded" href="/settingSchedule">Horarios</a>
                    </li>
                    <li class="mb-1">
                        <a on:click={closeMenuBurger} class="block p-4 text-sm font-semibold text-gray-400 hover:bg-blue-50 hover:text-blue-600 rounded" href="/setting">Recepción rollo</a>
                    </li>
                    <li class="mb-1">
                        <a on:click={closeMenuBurger} class="block p-4 text-sm font-semibold text-gray-400 hover:bg-blue-50 hover:text-blue-600 rounded" href="/setting">Movimiento tela</a>
                    </li>
                    <li class="mb-1">
                        <a on:click={closeMenuBurger} class="block p-4 text-sm font-semibold text-gray-400 hover:bg-blue-50 hover:text-blue-600 rounded" href="/reports">Reporte</a>
                    </li>
                </ul>
            </div>
        </nav>
    </div>
    {/if}
  </div>
  
<slot/>