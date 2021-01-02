 <script>
     import router from 'page';
     import Header from '../components/Header.svelte';
     import Footer from '../components/Footer.svelte';
     import Loader from '../components/Loader.svelte';
     
     export let params;
     let charity,amount,
     name,
     email,
     agree=false,contribute=0;
     async function getCharity(id){
         const res = await fetch(`https://charity-api-bwa.herokuapp.com/charities/${id}`);
         return res.json();
     }
     let data = getCharity(params.id)
    
     async function handleForm(event){
       agree=false;
        charity.pledged = charity.pledged + parseInt(amount);
        try {
            const res = await fetch(`https://charity-api-bwa.herokuapp.com/charities/${params.id}`,{
            method:'PUT',
            headers: {
                'content-type':'application/json'
            },
            body:JSON.stringify(charity)
        });  
        const resMid = await fetch(`/.netlify/functions/payment`,{
          method: "POST",
          headers:{
            "content-type":"application/json"
          },
          body: JSON.stringify({
            id: params.id,
            amount: parseInt(amount),
            name,
            email,
          })
        });
        const midtransData = await resMid.json();
        console.log(midtransData)
        window.location.href = midtransData.url;
        } catch (error) {
            console.log(error)
        } 
     };
     
 </script>
 <style>
     #xs-input-checkbox{
         display: flex;
         align-items: center;
     }
     #xs-donate-agree{
         width: 35px;
     }
     label[for='xs-donate-agree']{
         margin: 0;
         margin-left: 10px;
     }
 </style>
 <Header/>
 <!-- welcome section -->
    <!--breadcumb start here-->
    {#await data}
    <Loader/>
    
    {:then charity}
    <section class="xs-banner-inner-section parallax-window" style=
    "background-image:url('/assets/images/backgrounds/about_bg.jpg')">
      <div class="xs-black-overlay"></div>
      <div class="container">
        <div class="color-white xs-inner-banner-content">
          <h2>Donate Now</h2>
        <p>{charity.title}</p>
          <ul class="xs-breadcumb">
            <li class="badge badge-pill badge-primary">
              <a href="/" class="color-white">Home /</a> Donate
            </li>
          </ul>
        </div>
      </div>
    </section><!--breadcumb end here--><!-- End welcome section -->
    <main class="xs-main">
      <!-- donation form section -->
      <section class="xs-section-padding bg-gray">
        <div class="container">
          <div class="row">
            <div class="col-lg-6">
              <div class="xs-donation-form-images"><img src=
              "{charity.thumbnail}" class="img-responsive" alt=
              "Family Images"></div>
            </div>
            <div class="col-lg-6">
              <div class="xs-donation-form-wraper">
                <div class="xs-heading xs-mb-30">
                  <h2 class="xs-title">{charity.title}</h2>
                  <p class="small">To learn more about make donate charity
                    with us visit our "<span class="color-green">Contact
                      us</span>" site. By calling <span class=
                      "color-green">+44(0) 800 883 8450</span>.</p>
                      <h5>Your donation will be contributing<strong>{contribute}%</strong>on total current donation</h5>
                      <span class="xs-separetor v2"></span>
                </div><!-- .xs-heading end -->
            <form action="#" on:submit={handleForm} method="post" id="xs-donation-form" class=
                "xs-donation-form" name="xs-donation-form">
                  <div class="xs-input-group">
                    <label for="xs-donate-name">Donation Amount <span class=
                    "color-light-red">**</span></label> 
                    <input type="text" name="name" bind:value={amount} required="true" id="xs-donate-name" class="form-control"placeholder="Your donation in Rupiah">
                  </div><!-- .xs-input-group END -->
                  <div class="xs-input-group">
                    <label for="xs-donate-name">
                        Your Name 
                        <span class="color-light-red">**</span>
                    </label>
                    <input type="text" name="name" bind:value={name} required="true" id="xs-donate-name" class="from-control" placeholder="Your awesome name">
                </div>
                  <div class="xs-input-group">
                      <label for="xs-donate-email">
                          Your Email 
                          <span class="color-light-red">**</span>
                      </label>
                      <input type="email" name="email" bind:value={email} required="true" id="xs-donate-email" class="from-control" placeholder="email@awesome.com">
                  </div>
                  <!-- <div class="xs-input-group">
                    <label for="xs-donate-charity">List of Evaluated Charities
                      <span class="color-light-red">**</span></label>
                    <select name="charity-name" id="xs-donate-charity" class=
                    "form-control">
                      <option value="">
                      Select
                      </option>
                      <option value="amarokSocity">
                      Amarok socity
                      </option>
                      <option value="amarokSocity">
                      Amarok socity
                      </option>
                      <option value="amarokSocity">
                      Amarok socity
                      </option>
                      <option value="amarokSocity">
                      Amarok socity
                      </option>
                    </select>
                  </div> -->
                  <div class="xs-input-group" id="xs-input-checkbox">
                    <input type="checkbox" name="agree" bind:checked={agree} id="xs-donate-agree">
                    <label for="xs-donate-agree">
                        I agree
                        <span class="color-light-red">**</span>
                    </label>
                  </div><!-- .xs-input-group END -->
                  <button type="submit" disabled={!agree} class="btn btn-warning"><span class=
                  "badge"><i class="fa fa-heart"></i></span> Donate
                now</button>
                </form><!-- .xs-donation-form #xs-donation-form END -->
              </div>
            </div>
          </div><!-- .row end -->
        </div><!-- .container end -->
      </section><!-- End donation form section -->
    </main><!-- footer section start -->
    {/await}
    <Footer/>