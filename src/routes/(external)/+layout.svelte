<script>
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger,Button } from 'flowbite-svelte'
</script>


    <Navbar let:hidden let:toggle >
      <NavBrand href="/">
        <img
          src="https://flowbite.com/docs/images/logo.svg"
          class="mr-3 h-6 sm:h-9"
          alt="Flowbite Logo"
        />
        <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
          Barber
        </span>
      </NavBrand >
      <div class="flex md:order-2">
        <a   class="inline-block rounded bg-indigo-600 px-4 py-2 text-sm font-medium text-white transition hover:scale-110 hover:shadow-xl focus:outline-none focus:ring active:bg-indigo-500"
             href="/login"
        ><i class="fas fa-user"></i></a>
        <NavHamburger on:click={toggle} />
      </div>
      <NavUl {hidden} class="order-1">
        <NavLi href="/" active={true}>Home</NavLi>
        <!-- <NavLi href="/about">About</NavLi>
        <NavLi href="/services">Services</NavLi>
        <NavLi href="/pricing">Pricing</NavLi>
        <NavLi href="/contact">Contact</NavLi> -->
      </NavUl>
    </Navbar>  


  <slot></slot>