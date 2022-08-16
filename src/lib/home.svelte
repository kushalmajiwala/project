<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import Nocv from "./nocv.svelte";
    import Usernotfound from "./usernotfound.svelte";
    let userFound = false;
    let cvFound = false;

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
            })
            .catch(function (error) {
                userFound = false;
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
        {#if userFound == true}
           {#if cvFound == false}
                <Nocv />
            {:else}
                <div>
                    <h1>MyCV</h1>
                </div>
           {/if}
        {/if}
    </div>
    <div>
        {#if userFound == false}
            <Usernotfound />
        {/if}
    </div>
</main>
