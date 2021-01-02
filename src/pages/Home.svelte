<script>
    import {onDestroy} from 'svelte';
    import Header from '../components/Header.svelte';
    import Welcome from '../components/Welcome.svelte';
    import Charitylist from '../components/Charitylist.svelte';
    import Promo from '../components/Promo.svelte';
    import Footer from '../components/Footer.svelte';
    import Loader from '../components/Loader.svelte';

    onDestroy(function(){
        window.scrollTo(0,0);
    })
    
  
</script>

<Header/>
<Welcome/>
<Charitylist/>   
<Promo/>
<Footer/>