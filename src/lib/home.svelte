<script>
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
</style>