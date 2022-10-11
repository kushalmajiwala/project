<!-- <script>
    import { onMount } from "svelte";
    import axios from "axios";
    import Nocv from "./nocv.svelte";
    import Usernotfound from "./usernotfound.svelte";
    import { Spinner } from 'sveltestrap';
    let userFound = false;
    let cvFound = true;
    let showLoading = true;

    function checkUserExist() {
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/login/username/" + username,
        };
        axios
            .request(options)
            .then(function (response) {
                console.log(response.data);
                userFound = true;
                cvFound = false;
                showLoading = false;
            })
            .catch(function (error) {
                userFound = false;
                cvFound = false;
                showLoading = false;
            });
    }
    function checkCvExist() {

    }

    onMount(async () => {
        await checkUserExist();
    });

    export let username;
</script>

<main>
    <div>
        {#if showLoading == true}
            <div class="loading">
                <Spinner />
            </div>
        {:else if userFound == true && cvFound == false}
            <div class="home-image">
                <Nocv />
            </div>
        {:else if userFound == true && cvFound == true}
            <div>
                <h1>MyCV</h1>
            </div>
        {:else if userFound == false}
            <Usernotfound />
        {/if}
    </div>
</main>
<style>
    .loading {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 600px;
    }
    .home-image {
        background-image: url("../assets/images/homeimage.png");
        background-size: 100% 100%;
        background-repeat: no-repeat;
        height: 615px;
        width: 100%;
        background-position: center;
    }
    @media screen and (max-width: 1250px) {
        .home-image {
            height: 675px;
        }
    }
</style> -->

<body>
<header class="w3-display-container w3-content w3-center" style="max-width:1500px">
    <img class="w3-image" src="https://png.pngtree.com/thumb_back/fh260/back_pic/03/80/06/3757c30a762387b.jpg" alt="Me" width="1500" height="800">
    <div class="w3-display-middle w3-padding-large w3-border w3-wide w3-text-light-grey w3-center">
      <h1 class="w3-hide-medium w3-hide-small w3-xxxlarge">JANE DOE</h1>
      <h5 class="w3-hide-large" style="white-space:nowrap">JANE DOE</h5>
      <h3 class="w3-hide-medium w3-hide-small">PHOTOGRAPHER</h3>
    </div>
    
    <!-- Navbar (placed at the bottom of the header image) -->
    <div class="w3-bar w3-light-grey w3-round w3-display-bottommiddle w3-hide-small" style="bottom:-16px">
      <a href="#" class="w3-bar-item w3-button">Home</a>
      <a href="#mycv" class="w3-bar-item w3-button">My-CV</a>
      <a href="#myletter" class="w3-bar-item w3-button">My-Letters</a>
      <a href="#contact" class="w3-bar-item w3-button">Contact</a>
    </div>
  </header>
  
  <!-- Navbar on small screens -->
  <div class="w3-center w3-light-grey w3-padding-16 w3-hide-large w3-hide-medium">
  <div class="w3-bar w3-light-grey">
    <a href="#" class="w3-bar-item w3-button">Home</a>
    <a href="#mycv" class="w3-bar-item w3-button">My-CV</a>
    <a href="#myletter" class="w3-bar-item w3-button">My-Letters</a>
    <a href="#contact" class="w3-bar-item w3-button">Contact</a>
  </div>
  </div>
  <!-- Page content -->
  <div class="mycv" id="mycv">
    my CV
  </div>
  <div class="myletter" id="myletter">
    my Letters
  </div>
  
    <!-- Images (Portfolio) -->
    <!-- <img src="/w3images/ocean.jpg" alt="Ocean" class="w3-image" width="1000" height="500">
    <img src="/w3images/ocean2.jpg" alt="Ocean II" class="w3-image w3-margin-top" width="1000" height="500">
    <img src="/w3images/falls2.jpg" alt="Falls" class="w3-image w3-margin-top" width="1000" height="500">
    <img src="/w3images/mountainskies.jpg" alt="Skies" class="w3-image w3-margin-top" width="1000" height="500">
    <img src="/w3images/mountains2.jpg" alt="Mountains" class="w3-image w3-margin-top" width="1000" height="500">
   -->
    <!-- Contact -->
    <div class="w3-light-grey w3-padding-large w3-padding-32 w3-margin-top" id="contact">
      <h3 class="w3-center">Contact</h3>
      <hr>
      <p></p>
  
      <form action="/action_page.php" target="_blank">
        <div class="w3-section">
          <label>Name</label>
          <input class="w3-input w3-border" type="text" required name="Name">
        </div>
        <div class="w3-section">
          <label>Email</label>
          <input class="w3-input w3-border" type="text" required name="Email">
        </div>
        <div class="w3-section">
          <label>Message</label>
          <input class="w3-input w3-border" required name="Message">
        </div>
        <button type="submit" class="w3-button w3-block w3-dark-grey">Send</button>
      </form><br>
      <!-- <p>Powered by <a href="https://www.w3schools.com/w3css/default.asp" target="_blank" class="w3-hover-text-green">w3.css</a></p> -->
    </div>
  </body>

  <style>
    .mycv {
        border: 2px solid black;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 500px;
    }
    .myletter {
        border: 2px solid black;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 500px;
    }
  </style>
