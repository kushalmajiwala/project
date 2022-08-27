<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import Nocv from "./nocv.svelte";
    import Usernotfound from "./usernotfound.svelte";
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
                <h1>Loading...</h1>
            </div>
            {:else if userFound == true && cvFound == false}
                 <Nocv />
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
</style>